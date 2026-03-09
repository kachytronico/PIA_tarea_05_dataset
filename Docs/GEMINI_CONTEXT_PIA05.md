# GEMINI — CONTEXTO GENERAL PIA05 (Visión por Computador)

Este documento define el contexto completo de la **Tarea PIA05 (UD5 Visión por Computador)**.
Úsalo como referencia antes de ayudar a completar conclusiones, revisar resultados o depurar errores.

---

## 1. Descripción General de la Tarea

La Tarea PIA05 consta de **un problema central** (Problema 1: ESCA) desarrollado en un único notebook:

### Problema 1 — Clasificación de Imágenes (ESCA)
Notebook: `PIA_05_Tarea_01.ipynb`

**Objetivo**:
- Construir un clasificador binario de plantas infectadas por ESCA vs. sanas.
- Usar Fast AI v2 con transfer learning (fine-tuning en ResNet).
- Aplicar mejoras progresivas: LR óptimo, augmentación, anti-overfitting, progressive learning.
- Comparar dos arquitecturas y analizar errores (matriz de confusión).
- Contextualizar todo al caso de negocio (Juan, agricultor).

**Dataset**: https://data.mendeley.com/datasets/89cnxc58kj/1
- Estructura: `esca_dataset/esca/` (infectadas) y `esca_dataset/healthy/` (sanas)

**Requisitos Obligatorios (10 puntos)**:
1. Modelo básico con ResNet18 (Cuadernillo 502)
2. Learning rate óptimo (lr_find)
3. Aumento de datos (augmentación)
4. Técnicas anti-overfitting (callbacks, weight decay)
5. Progressive learning (dos resoluciones)
6. Segunda arquitectura (ResNet34 o ResNet50)
7. Matriz de confusión (análisis contextualizado)

**Requisitos Opcionales (2 puntos adicionales)**:
8. Interpretabilidad (Grad-CAM, cuadernillo 506)
9. Insight del problema (qué está aprendiendo realmente)

---

## 2. Reglas Críticas (Líneas Rojas)

Estas reglas **NO pueden romperse**:

### ❌ Prohibiciones Absolutas
- **No TensorFlow ni Keras**: Solo FastAI v2 exclusivamente.
- **No entrenar desde cero** (`from_scratch=True`): Siempre fine-tuning.
- **No rutas hardcodeadas**: Usar `Path` relativas, auto-detectar.
- **No sin lr_find()**: Todo debe pasar por búsqueda de LR.
- **No omitir explicaciones**: Cada sección necesita Markdown técnico.
- **No confundir train/test**: Split aleatorio, indices disjuntos, sin data leakage.
- **No conclusiones genéricas**: Todo debe tener números, no afirmaciones vagas.

### ✅ Obligaciones
- Usar `DataBlock`, `dataloaders`, `vision_learner`, `fine_tune`.
- Implementar `set_seed(42)` para reproducibilidad.
- Aplicar `batch_tfms=aug_transforms()`.
- Usar callbacks: `EarlyStoppingCallback(patience=3)` mínimo.
- Implementar progressive learning (dos fases de resolución).
- Generar matriz de confusión con análisis de FP/FN.
- Guardar checkpoints: `learn.save()` después de cada hito.

---

## 3. Contexto de Negocio (Crítico)

**Juan es un agricultor** con cientos de vides. Cada año pierde dinero porque:
- Elimina plantas sanas por sospechas de ESCA (falsos positivos del humano).
- No detecta tiempo todas las enfermas (falsos negativos).

**Impacto de errores del modelo**:
- **Falso Negativo** (enferma → predicción sana): CATASTROFE. El contagio se propaga masivamente.
- **Falso Positivo** (sana → predicción enferma): Costo aceptable. Juan hace inspección manual (barato).

**Por eso**:
- Priorizamos **baja tasa de falsos negativos** (sensibilidad >95%).
- Aceptamos falsos positivos moderados (<15%).
- Analizamos la matriz de confusión **siempre en contexto agrícola**.

---

## 4. Estructura del Notebook

El notebook debe tener **exactamente estas secciones**, en este orden:

1. Contexto del Problema (Markdown)
2. Setup e Imports (Código)
3. Carga y Exploración Dataset (Código + Viz)
4. **Modelo Base (502)** — 4 puntos
5. **Learning Rate Óptimo** — 1 punto
6. **Aumento de Datos** — 1 punto
7. **Anti-Overfitting** — 1 punto
8. **Progressive Learning** — 1 punto
9. **Segunda Arquitectura** — 1 punto
10. **Matriz de Confusión** — 1 punto
11. (Opcional) Grad-CAM
12. Conclusión General

Cada sección: Markdown explicativo + Código ejecutable + Conclusión técnica.

---

## 5. Patrones de Código Obligatorios

### FastAI Setup
```python
from fastai.vision.all import *
set_seed(42)

path = Path('./esca_dataset/esca')
db = DataBlock(
    blocks=(ImageBlock, CategoryBlock),
    get_items=get_image_files,
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=Resize(224),
    batch_tfms=aug_transforms()  # SIEMPRE augmentación
)

dls = db.dataloaders(path, bs=32)
learn = vision_learner(dls, resnet18, metrics=accuracy)
learn.fine_tune(3)
```

### Learning Rate Find
```python
learn.lr_find()  # Mostrar gráfica
# Comentario: "Eleggo LR = 1e-3 (zona pre-valle)"
learn.fine_tune(4, lr_max=1e-3)
```

### Anti-Overfitting
```python
learn.fine_tune(
    10,
    lr_max=1e-3,
    wd=0.1,
    cbs=[EarlyStoppingCallback(patience=3)]
)
```

### Matriz de Confusión
```python
interp = ClassificationInterpretation.from_learner(learn)
interp.plot_confusion_matrix(figsize=(6,6))

# Análisis CONTEXTUALIZADO:
# TP: Enfermas detectadas ✓ 45
# TN: Sanas detectadas ✓ 52
# FP: Falsas alarmas (Juan inspecciona): 8 → Aceptable
# FN: Enfermas pasadas (RIESGO): 3 → Bajo ✓
```

---

## 6. Estilo de Comentarios en Código

El código debe tener comentarios **en primera persona, naturales y directos**:

### ✅ CORRECTO
```python
# Creo que 32 es buen batch size para esta GPU
bs = 32

# Voy a usar LR=1e-3 porque en la gráfica lr_find es donde empieza a diverger
learn.fine_tune(4, lr_max=1e-3)

# Noto que train y val loss están muy separados (0.1 vs 0.5)
# Necesito más regularización
learn.fine_tune(10, wd=0.2)
```

### ❌ INCORRECTO
```python
# Configurar batch size
bs = 32

# Fine-tuning con learning rate óptimo
learn.fine_tune(4, lr_max=1e-3)

# Entrenar con regularización adicional
learn.fine_tune(10, wd=0.2)
```

---

## 7. Estilo de Conclusiones

Cada sección debe terminar con una **conclusión técnica** (4-8 líneas):

### ✅ CORRECTO
> "El modelo base alcanza 82.3% accuracy en validación. Observo que la pérdida se estabiliza después de 3 épocas, lo que sugiere que ResNet18 es suficiente para este problema sin sobreajuste evidente. La diferencia train/val es de 5 puntos, aceptable para un dataset de ~200 imágenes. Voy a mantener esta baseline como referencia para medir impacto de mejoras posteriores."

### ❌ INCORRECTO
> "El modelo funciona bien. Los resultados son buenos. La red neuronal aprende adecuadamente."

### Elementos Obligatorios en Cada Conclusión:
- 📊 Números concretos (accuracy, loss, etc.)
- 🤔 Observación personal ("observo que", "noto que")
- 💡 Interpretación técnica de qué significa el número
- 🎯 Decisión o próximo paso tomado como consecuencia

---

## 8. Errores Frecuentes a Evitar

| Error | ❌ MAL | ✅ BIEN |
|-------|------|-------|
| Conclusión genérica | "El modelo mejora" | "Accuracy sube de 82% a 85% (+3 p.p.)" |
| Afirmación sin dato | "Es rápido" | "Tiempo de entrenamiento: 120s" |
| Comentario innecesario | "# Crear lista" | "# Filtro ejemplos con valor > 0.5" |
| Frase de plantilla | "Para optimizar el rendimiento usamos..." | "Voy a usar LR=1e-3 porque..." |
| Data leakage silencioso | Fit en train + val mezclados | Fit SOLO en train |

---

## 9. Checklist Antes de Enviar

- [ ] Todas las secciones tienen Markdown explicativo
- [ ] Comentarios en primera persona ("creo", "observo")
- [ ] Conclusiones con números concretos, no genéricas
- [ ] Ninguna afirmación sin respaldo en output
- [ ] Sin rutas hardcodeadas
- [ ] set_seed(42) en setup
- [ ] Todos los modelos tienen learn.save() tras entrenamiento
- [ ] Matriz de confusión analizada contextualizando a Juan

---

## 10. Recursos Internos

Consulta estos documentos en `/Docs/`:
- `PIA_05_CONTEXTO_IA.md` — Definición completa del problema
- `PIA_05_GUIA_ESTILO.md` — Normas de formato Markdown
- `PIA_05_GUIA_OPERATIVA.md` — Paso a paso con código
- `PIA_05_CHECKLIST.md` — Verificación de requisitos

---

**Referencia**: Este contexto es para Gemini (u otro asistente). Úsalo como guía al revisar, completar o depurar trabajos en la Tarea PIA05.

