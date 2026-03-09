---
name: PIA_05_tarea
description: Resuelve PIA05 (ESCA + FastAI v2). Clasifica plantas infectadas/sanas usando transfer learning.
argument-hint: Ej. "genera sección modelo base 502", "debug CUDA OOM", "matriz confusión análisis"
---

# PIA05: Detección de ESCA con FastAI v2

## 📌 Referencia Rápida

| Aspecto | Valor |
|---------|-------|
| **Framework** | FastAI v2 (obligatorio) |
| **Problema** | Clasificación binaria: Sana/Infectada |
| **PDF Cuadernillo** | 502 Clasificación de imágenes (en Notebooks/) |
| **Métrica Crítica** | Sensibilidad >95% (detectar FN mínimo) |
| **Impacto Negocio** | FN (contagio masivo) >> FP (inspección) |

## 🎯 Los 7 Requisitos Obligatorios

1. **Modelo Base 502** — DataBlock + ResNet18 + fine-tune
2. **LR Óptimo** — learn.lr_find() + re-train
3. **Anti-Overfitting** — EarlyStoppingCallback + wd=0.1
4. **Augmentación** — batch_tfms=aug_transforms()
5. **Progressive Learning** — Resize(128) → Resize(224)
6. **Segunda Architectura** — ResNet34 comparativa
7. **Confusión Matrix** — TP/TN/FP/FN contexto Juan

**Bonus** (2 puntos):
- Grad-CAM: Visualizar qué aprende la red
- Insight: Sesgo/error analysis

## 🗂️ Documentación Disponible

Ve primero a **Docs/** (mantén todo simple):
- `GEMINI_INSTRUCTIONS_PIA05.md` — Resumen ejecutivo (5 min)
- `GEMINI_STYLE_GUIDE_PIA05.md` — Cómo escribir natural (1era persona)
- `GEMINI_DEBUG_GUIDE_PIA05.md` — Errores FastAI + soluciones
- `PIA_05_CONTEXTO_IA.md` — Problema técnico completo

## ✅ Estructura Notebook (Simple)

```
1. SETUP: import fastai, set_seed(42), detect GPU
2. LOAD: path auto-detect, get_image_files, clases
3. 502 BASE: DataBlock, vision_learner(resnet18), fine_tune(3)
4. LR: lr_find(), gráfica, re-train
5. AUG: batch_tfms=aug_transforms()
6. ANTIOV: EarlyStoppingCallback + wd=0.1
7. PROGR: Resize(128) → Resize(224)
8. RES34: Segunda arquitectura, tabla comparativa
9. CONFUS: Matriz + análisis FP/FN para Juan
10. CONCLU: Resumen + recomendación producción
```

## 🚀 Código Mínimo

```python
from fastai.vision.all import *
set_seed(42)

# Auto-detect path
path = Path('./esca_dataset/esca')

# DataBlock (siguiendo estructura 502)
db = DataBlock(
    blocks=(ImageBlock, CategoryBlock),
    get_items=get_image_files,
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=Resize(224),
    batch_tfms=aug_transforms()
)

dls = db.dataloaders(path, bs=32)

# Modelo + LR + Training
learn = vision_learner(dls, resnet18, metrics=accuracy)
learn.lr_find()
learn.fine_tune(10, lr_max=1e-3, wd=0.1, 
                cbs=[EarlyStoppingCallback(patience=3)])

# Evaluación
interp = ClassificationInterpretation.from_learner(learn)
interp.plot_confusion_matrix()
```

## ❌ Prohibido / ✅ Obligatorio

```
❌ NO: TensorFlow, Keras, PyTorch puro, random seed sin fijar
✅ SÍ: FastAI only, set_seed(42), rutas relativas, learn.save(), comentarios 1era persona
```

## 💡 Estilo de Código

**Comentarios 1era persona** (Juan es agricultor, no AI):
```python
# ✅ BIEN: "Voy a usar ResNet18 porque GPU es limitada"
# ❌ MAL: "ResNet18 es utilizado para clasificación"

# ✅ BIEN: "Observo que LR diverge tras 1e-2, elijo 1e-3"
# ❌ MAL: "El learning rate óptimo es configurado"
```

**Conclusiones con números**:
```
✅ "Accuracy 85.2% (train 87%, val 85%), FN=2%, confiable para Juan"
❌ "El modelo funciona bien, es un buen resultado"
```

## 🆘 Errores Comunes (Ver Docs/GEMINI_DEBUG_GUIDE_PIA05.md)

| Error | Fix |
|-------|-----|
| CUDA OOM | bs=32→16, reiniciar kernel |
| Loss=NaN | LR en pre-valle, no divergencia |
| Acc=100% | Revisar data leakage |
| Train/Val separadas | ↑wd, ↑patience, revisar dataset balance |

## 🎓 Usar Este Agente

Pregunta directo:
- "Genera sección **Modelo Base 502**"
- "¿Por qué CUDA OOM? Debug"
- "Matriz confusión: qué significa TP/TN/FP/FN?"

**Pasa referencia a**: Docs/GEMINI_INSTRUCTIONS_PIA05.md si necesitas intro completa

## 5) Mejoras Obligatorias — Justificación Técnica

| Mejora | ¿Qué es? | ¿Por qué? | Implementación |
|--------|----------|----------|-----------------|
| **LR Óptimo** | Tasa de aprendizaje / step de SGD | LR ↑ = divergencia; LR ↓ = lento | `learn.lr_find()` → gráfica → elegir pre-valle |
| **Augmentación** | Transformaciones aleatorias (rotación, brillo, perspectiva) | ↑ efectivas del dataset sin almacenar réplicas | `batch_tfms=aug_transforms()` |
| **Anti-Overfitting** | Callbacks + regularización | Evitar memorización (train 100%, val 50%) | `EarlyStoppingCallback(patience=3)` + `wd=0.1` |
| **Progressive Learning** | Entrenar en resoluciones crecientes | Aprender formas → detalles, acelerar convergencia | `Resize(128)` → `Resize(224)` |
| **Second Arch** | ResNet34 o ResNet50 vs. ResNet18 | Comparar complejidad vs. precisión | Tabla: accuracy, tiempo, parámetros |
| **Confusion Matrix** | Matriz de TP/TN/FP/FN | Entender errores en contexto de negocio | `interp.plot_confusion_matrix()` + párrafo analítico |

## 6) Criterios de Calidad y Checklist

### ✅ **Fases Completadas**
- [ ] Setup: imports, semilla, GPU detectada.
- [ ] Dataset: ruta automática, clases validadas, conteo de imágenes.
- [ ] Modelo base: DataBlock correcto, fine_tune(3) ejecutado, métricas registradas.
- [ ] LR): gráfica generada, LR elegido justificado, re-entrenamiento con nuevo LR.
- [ ] Augmentación: `batch_tfms` añadido, visualización de ejemplos aumentados.
- [ ] Anti-overfitting: `EarlyStoppingCallback` funcional, curvas de pérdida analizadas.
- [ ] Progressive: Resize(128) → Resize(224), cambio en learning explicado.
- [ ] Second arch: ResNet34, tabla comparativa, conclusión sobre trade-off.
- [ ] Confmatrix: plot generado, análisis de FP/FN contextualizados a Juan.
- [ ] Conclusión: resumen de mejoras, métrica final, recomendación (confianza del modelo).

### ✨ **Bonus Opcional**
- [ ] Grad-CAM: attention map generado, validación visual (qué mira la red).
- [ ] Insight: sesgo detectado (ej. fondo vs. hoja), explicado y mitiga propuesto.

## 7) Errores Comunes y Soluciones

| Error | Síntoma | Solución |
|-------|---------|----------|
| Sobreajuste extremo | Train: 100%, Val: 60% | ↑ `patience`, ↑ `wd`, revisar dataset size |
| CUDA out of memory | RuntimeError al correr celda | Reducir `bs`, reiniciar kernel, `del learn; gc.collect()` |
| Rutas hardcodeadas | FileNotFoundError en otro PC | Usar `Path` relativas, detectar automáticamente |
| LR demasiado alto | Loss = NaN en gráfica lr_find | Elegir LR en la "curva plana" pre-valle, no en la subida |
| FN altos (riesgo ESCA) | Modelo detecta poco enfermedad | Aumentar épocas, revisar desbalance de clases, ajustar umbral |
| Confusión train/val | Métricas perfectas pero falla en mundo real | Asegurar `RandomSplitter`, índices disjuntos, no data leakage |
| GPU no detectada | `torch.cuda.is_available() = False` | Asegurar runtime GPU en Colab, `nvidia-smi` en terminal |

## 8) Modo de Respuesta

- **Redacción**: Español técnico claro, alineado con la rúbrica oficial.
- **Precisión**: Código ejecutable, explicaciones concretas, outputs esperados.
- **Iterativo**: Si falta contexto (ruta, estructura), preguntar de forma precisa.
- **Trazabilidad**: Cada requisito de la rúbrica → evidencia clara en notebook.

## 9) Orden de Peticiones Recomendado

Cuando pidas ayuda, usa:

> "Genera la sección **[NOMBRE]** del notebook de PIA05 usando FastAI v2. Incluye:
> 1) Markdown explicativo.
> 2) Código ejecutable.
> 3) Análisis de resultados.
> Mantén coherencia con ESCA y la rúbrica oficial."

Ejemplo:
> "Genera la sección **Modelo Base (Cuadernillo 502)** del notebook de PIA05..."

---

**Estado inicial**: Dataset cargado desde GitHub. Procede a completar sección por sección respetando la estructura anterior.