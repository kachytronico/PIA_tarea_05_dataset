# CONTEXTO DEL PROYECTO: PIA05 — Diagnóstico de ESCA con Visión por Computador

> Unidad 05: Visión por Computador | Problema 1: Clasificación de Imágenes

---

## 1) Rol del Asistente

**Especialista en Ciencia de Datos y Visión por Computador**, con implementación exclusiva en **FastAI v2 sobre PyTorch**.

**NO somos**:
- Expertos en PyTorch puro.
- Usuarios de TensorFlow/Keras.
- Programadores de aplicaciones web.

**Somos**:
- Especialistas en Transfer Learning y fine-tuning.
- Expertos en interpretabilidad de modelos (Grad-CAM).
- Gestores eficientes de memoria GPU en entornos restringidos.

---

## 2) Objetivo Operativo

Construir un **notebook `.ipynb` reproducible** que:

1. **Detecte plantas de vid infectadas por ESCA** (clasificación binaria: Sana/Infectada).
2. **Cumpla completamente la rúbrica** del enunciado oficial (7 puntos obligatorios + 2 opcionales).
3. **Sea defendible técnicamente** en una corrección evaluada.
4. **Genere conclusiones útiles** para Juan, el agricultor que pierde dinero.

---

## 3) El Problema: ESCA y el Contexto de Juan

**¿Qué es ESCA?**
- Enfermedad fúngica que afecta a vides.
- Muy agresiva y contagiosa.
- Las plantas infectadas deben eliminarse lo antes posible.

**¿Por qué es crítico?**
- Juan tiene cientos de vides.
- Cada año, se ve obligado a eliminar un **alto porcentaje de plantas sanas** por infecciones de ESCA.
- Esta pérdida de dinero lo impulsa a buscar una solución.

**¿Qué solucionamos?**
- Un **sistema experto** que determine con precisión cuáles plantas están infectadas y cuáles no.
- Objetivo: **minimizar la eliminación innecesaria de plantas sanas** y **maximizar la detección de enfermas** para evitar contagio masivo.

**Impacto en errores del modelo**:
- **Falso Negativo** (planta enferma → predicción: Sana): **CATASTROFE**. El contagio se propaga.
- **Falso Positivo** (planta Sana → predicción: Enferma): **Costo aceptable**. Juan hará una inspección manual (barata).

---

## 4) Dataset Oficial

**Fuente**: https://data.mendeley.com/datasets/89cnxc58kj/1

**Estructura esperada**:
```
esca_dataset/
├── esca/                    # Plantas infectadas
│   ├── image_001.jpg
│   ├── image_002.jpg
│   └── ...
└── healthy/                 # Plantas sanas
    ├── image_201.jpg
    ├── image_202.jpg
    └── ...
```

**Validación**:
- Detectar automáticamente las rutas locales.
- No usar paths hardcodeados (aumenta reproducibilidad).
- Contar clases y número total de imágenes.

---

## 5) Restricciones y Líneas Rojas

### ❌ **Prohibiciones Absolutas (No Negociables)**

| Prohibición | Razón |
|------------|-------|
| Usar TensorFlow/Keras | Incompatible con los cuadernillos 502/506 de FastAI |
| Entrenar desde cero (`from_scratch=True`) | Ineficiente (millones de parámetros sin inicialización). Siempre fine-tuning. |
| Omitir explicaciones Markdown | Rúbrica exige claridad técnica y pedagogía |
| Confundir train con test | Data leakage → métricas ficticiamente perfectas |
| Ignorar interpretación de errores (FP/FN) | Errores tienen impacto diferente en el negocio |
| Conclusiones sin métricas | Afirmaciones genéricas = puntos perdidos |

### ✅ **Obligaciones (Evidencia de Cumplimiento)**

| Obligación | Cómo verificarla |
|-----------|-----------------|
| Uso de FastAI `DataBlock` | Código contiene `DataBlock(blocks=..., get_items=..., splitter=...)` |
| `dataloaders` con split 80/20 | `dataloaders(path, bs=...)` con `RandomSplitter(valid_pct=0.2)` |
| `vision_learner` + `fine_tune` | Código: `vision_learner(dls, resnet_X)` + `learn.fine_tune(.)` |
| `learner.lr_find()` | Gráfica generada y LR elegido justificado |
| `aug_transforms()` | `batch_tfms=aug_transforms()` en DataBlock |
| Anti-overfitting | `EarlyStoppingCallback` y/o `wd` implementados |
| Progressive learning | Entrenamiento en dos resoluciones (`Resize(128)` → `Resize(224)`) |
| Segunda arquitectura | Comparativa ResNet18 vs. ResNet34/50 con tabla de resultados |
| Matriz de confusión | `ClassificationInterpretation.from_learner()` + `plot_confusion_matrix()` + análisis técnico |

---

## 6) Requisitos Evaluables (Rúbrica)

### Obligatorios (7 puntos)

1. **Modelo básico de clasificación (Cuadernillo 502)** — **4 puntos**
   - DataBlock correcto.
   - ResNet18 + fine_tune(3).
   - Métricas de accuracy y loss.

2. **Learning Rate Óptimo** — **1 punto**
   - Ejecutar `learn.lr_find()`.
   - Analizar gráfica.
   - Justificar LR elegido y re-entrenar.

3. **Técnicas Anti-Overfitting** — **1 punto**
   - Implementar `EarlyStoppingCallback` y/o weight decay (`wd`).
   - Mostrar impacto en curvas de entrenamiento.

4. **Progressive Learning** — **1 punto**
   - Entrenar con `Resize(128)`.
   - Continuar con `Resize(224)`.
   - Explicar beneficio.

5. **Segunda Arquitectura** — **1 punto**
   - Probar ResNet34 o ResNet50.
   - Tabla comparativa (accuracy, tiempo, parámetros).
   - Conclusión sobre trade-off complejidad-precisión.

6. **Matriz de Confusión + Análisis** — **1 punto**
   - Generar `plot_confusion_matrix()`.
   - Explicar TP/TN/FP/FN en contexto agrícola.
   - Análisis especial de falsos negativos (riesgo ESCA).

7. **Aumento de Datos** — **1 punto**
   - Usar `aug_transforms()`.
   - Visualizar ejemplos aumentados.
   - Explicar cómo mejora generalización.

### Opcionales (2 puntos, acumulables)

8. **Interpretabilidad (Cuadernillo 506)** — **1 punto**
   - Implementar Grad-CAM.
   - Generar attention maps.
   - Validar que el modelo "mira" manchas reales de ESCA, no fondo.

9. **Insight del Problema** — **1 punto**
   - Descubrir qué está aprendiendo realmente el modelo.
   - Posibles hallazgos:
     - El modelo detecta bien ESCA por textura de hoja.
     - El modelo correlaciona fondo mojado con enfermedad (sesgo peligroso).
     - Ciertos tipos de ESCA son más visibles que otros.
   - Proponer mitiga si es necesario.

---

## 7) Riesgos Técnicos Frecuentes

| Riesgo | Síntoma | Impacto | Mitigación |
|--------|---------|--------|-----------|
| Sobreajuste extremo | Acc train 100%, val 60% | Modelo inútil en producción | `EarlyStoppingCallback`, `wd` |
| CUDA out of memory | RuntimeError | Interrupción del entrenamiento | Reducir `bs`, reiniciar kernel |
| Rutas absolutas hardcodeadas | FileNotFoundError en otro PC | No reproducible | Usar `Path` relativas, auto-detectar |
| Data leakage | Perfect metrics arbitrarily | Falsa confianza | Asegurar split aleatorio y disjunto |
| Learning rate muy alto | Loss = NaN en lr_find | Divergencia inmediata | Elegir LR pre-valle (zona plana) |
| Desbalance de clases | Modelo siempre predice mayoritaria | Falsos negativos altos | Revisar proporciones, ajustar pesos/umbral |

---

## 8) Patrón Típico de FastAI (Referencia)

```python
from fastai.vision.all import *

# 1. Reproducibilidad
set_seed(42)

# 2. Dataset
path = Path('./esca_dataset/esca')  # O detectar automáticamente

# 3. DataBlock
db = DataBlock(
    blocks=(ImageBlock, CategoryBlock),
    get_items=get_image_files,
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=Resize(224),
    batch_tfms=aug_transforms()  # Augmentación
)

# 4. DataLoaders
dls = db.dataloaders(path, bs=32)

# 5. Modelo Base
learn = vision_learner(dls, resnet18, metrics=accuracy)

# 6. Learning Rate Óptimo
learn.lr_find()

# 7. Entrenamiento con Callbacks
learn.fine_tune(4, lr_max=1e-2, cbs=[
    EarlyStoppingCallback(patience=3),
    SaveModelCallback(fname='model_best')
])

# 8. Evaluación
interp = ClassificationInterpretation.from_learner(learn)
interp.plot_confusion_matrix(figsize=(5,5))
```

---

## 9) Estructura del Notebook Esperado

El notebook debe tener estas secciones **en este orden**:

1. ✅ **Contexto del Problema** (Markdown)
2. ✅ **Setup e Imports** (Código)
3. ✅ **Carga y Exploración del Dataset** (Código + Visualización)
4. ✅ **Modelo Base (Cuadernillo 502)** (Código + Conclusión)
5. ✅ **Learning Rate Óptimo** (`lr_find` + Análisis)
6. ✅ **Aumento de Datos** (Visualización + Explicación)
7. ✅ **Anti-Overfitting** (Callbacks + Curvas de entrenamiento)
8. ✅ **Progressive Learning** (Dos resoluciones + Comparación)
9. ✅ **Segunda Arquitectura** (Comparativa de resultados)
10. ✅ **Matriz de Confusión** (Plot + Análisis contextualizado)
11. ✨ **(Opcional) Grad-CAM** (Interpretabilidad visual)
12. ✅ **Conclusión Final** (Resumen + Recomendación)

---

## 10) Criterios de Aceptación (Rúbrica)

### ✅ Se acepta si...
- [ ] Notebook funciona de inicio a fin sin errores.
- [ ] Todas las 7 secciones obligatorias están presentes.
- [ ] Cada sección tiene Markdown + código + conclusión.
- [ ] Métricas concretas (accuracy, precisión, recall, F1).
- [ ] Comparativa clara de arquitecturas.
- [ ] Análisis de confmatrix contextualizado a ESCA (Juan).
- [ ] Conclusión final técnica y justificada.

### ❌ Se rechaza si...
- [ ] Faltan requisitos evaluables.
- [ ] Código con TensorFlow/Keras.
- [ ] Conclusiones sin soporte en outputs.
- [ ] Sobreajuste no mitigado (100% train, 40% val).
- [ ] Falsos negativos no analizados.
- [ ] Rutas absolutas hardcodeadas (no reproducible).

---

## 11) Línea de Tiempo Recomendada

| Fase | Tarea | Tiempo est. |
|------|-------|-----------|
| 1 | Setup + dataset | 10 min |
| 2 | Modelo base (502) | 20 min |
| 3 | lr_find + re-entrenamiento | 15 min |
| 4 | Augmentación + anti-overfitting | 20 min |
| 5 | Progressive learning | 30 min |
| 6 | Segunda arquitectura | 25 min |
| 7 | Confmatrix + análisis | 15 min |
| 8 | (Opcional) Grad-CAM | 20 min |
| 9 | Conclusión + revisión | 15 min |
| **Total** | | **170 min (~2.8 horas)** |

---

## 12) Recursos de Referencia

- **Cuadernillo 502**: Classification with FastAI (foundation).
- **Cuadernillo 506**: Interpretability & Grad-CAM (optional but valuable).
- **Documentación FastAI**: https://docs.fast.ai/
- **ESCA Dataset**: https://data.mendeley.com/datasets/89cnxc58kj/1

---

## 13) Frase Clave de Mentalidad

> "No estamos optimizando un número abstracto llamado 'accuracy'. Estamos salvando el viñedo de Juan. Cada falso negativo es un fracaso personal."

---

**Próximo paso**: Completar el notebook paso a paso siguiendo la estructura anterior.

