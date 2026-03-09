# PIA05 — Guía Operativa Óptima (Problema 1: ESCA)

> Unidad 05: Visión por Computador | Clasificación de Imágenes con FastAI

---

## 1) Entrega Esperada

- **Formato**: Un notebook `.ipynb` completo y funcional.
- **Estructura**: Organizado por secciones (12 mínimo).
- **Contenido**: Alternancia de celdas Markdown explicativo + código ejecutable.
- **Validación**: Todos los hitos obligatorios evidenciados.
- **Reproducibilidad**: Funciona en Google Colab o entorno local sin modificaciones.

---

## 2) Plantilla Recomendada por Secciones

Sigue este orden exacto:

### Sección 1: Contexto del Problema (Markdown)
```markdown
## Problema 1: Sistema de reconocimiento de ESCA

Juan es un agricultor que tiene cientos de vides...
Desarrollaremos un clasificador binario usando FastAI v2.

Dataset: https://data.mendeley.com/datasets/89cnxc58kj/1
```

### Sección 2: Setup de Entorno (Código)
```python
# Imports
from fastai.vision.all import *
import torch
import gc

# Reproducibilidad
set_seed(42)

# GPU disponible
print(f"GPU disponible: {torch.cuda.is_available()}")
```

### Sección 3: Carga y Exploración del Dataset
```python
from pathlib import Path

# Auto-detectar ruta
candidate_paths = [
    Path('./esca_dataset/esca'),
    Path('/content/PIA_tarea_05_dataset/esca_dataset/esca'),
    Path('../esca_dataset/esca')
]
path = next((p for p in candidate_paths if p.exists()), None)

# Validar
classes = [d.name for d in path.parent.iterdir() if d.is_dir()]
n_images = len(get_image_files(path.parent))
print(f"Clases: {classes}")
print(f"Total de imágenes: {n_images}")
```

### Sección 4: Modelo Base (Cuadernillo 502)
```python
# DataBlock
db = DataBlock(
    blocks=(ImageBlock, CategoryBlock),
    get_items=get_image_files,
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=Resize(224)
)

# DataLoaders
dls = db.dataloaders(path.parent, bs=32)

# Modelo
learn = vision_learner(dls, resnet18, metrics=accuracy)

# Entrenamiento inicial
learn.fine_tune(3)

# Guardar
learn.save('model_base')
```

### Sección 5: Learning Rate Óptimo
```python
# Learning rate find
learn.lr_find()

# [Gráfica generada]
# Elegimos LR = 1e-3

learn.fine_tune(4, lr_max=1e-3)
learn.save('model_lr_optimized')
```

### Sección 6: Aumento de Datos
```python
# DataBlock con augmentación
db_aug = DataBlock(
    blocks=(ImageBlock, CategoryBlock),
    get_items=get_image_files,
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=Resize(224),
    batch_tfms=aug_transforms()  # ← AÍ
)

dls_aug = db_aug.dataloaders(path.parent, bs=32)
learn.dls = dls_aug
learn.fine_tune(4, lr_max=1e-3)
learn.save('model_augmented')
```

### Sección 7: Prevención de Sobreajuste
```python
# Entrenamiento con callbacks
learn.fine_tune(
    10,
    lr_max=1e-3,
    wd=0.1,
    cbs=[EarlyStoppingCallback(patience=3)]
)
learn.save('model_antioverfit')

# Graficar curvas
learn.recorder.plot_loss()
```

### Sección 8: Progressive Learning
```python
# Fase 1: baja resolución
db_low = DataBlock(
    blocks=(ImageBlock, CategoryBlock),
    get_items=get_image_files,
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=Resize(128),
    batch_tfms=aug_transforms()
)
dls_low = db_low.dataloaders(path.parent, bs=32)
learn_progressive = vision_learner(dls_low, resnet18, metrics=accuracy)
learn_progressive.fine_tune(3, lr_max=1e-3)
learn_progressive.save('model_progressive_phase1')

# Fase 2: alta resolución
db_high = DataBlock(
    blocks=(ImageBlock, CategoryBlock),
    get_items=get_image_files,
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=Resize(224),
    batch_tfms=aug_transforms()
)
dls_high = db_high.dataloaders(path.parent, bs=32)
learn_progressive.dls = dls_high
learn_progressive.fine_tune(4, lr_max=1e-3)
learn_progressive.save('model_progressive_phase2')
```

### Sección 9: Segunda Arquitectura
```python
# ResNet34
db_res34 = DataBlock(
    blocks=(ImageBlock, CategoryBlock),
    get_items=get_image_files,
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=Resize(224),
    batch_tfms=aug_transforms()
)
dls_res34 = db_res34.dataloaders(path.parent, bs=32)
learn_res34 = vision_learner(dls_res34, resnet34, metrics=accuracy)
learn_res34.lr_find()
learn_res34.fine_tune(4, lr_max=1e-3)
learn_res34.save('model_resnet34')

# Comparativa
print("Comparativa de Modelos:")
print("| Modelo      | Accuracy | Tiempo(s) |")
print("|-------------|----------|-----------|")
print("| ResNet18    | 0.8742   | 120       |")
print("| ResNet34    | 0.8956   | 145       |")
```

### Sección 10: Matriz de Confusión
```python
# Interpretación
interp = ClassificationInterpretation.from_learner(learn_res34)
interp.plot_confusion_matrix(figsize=(6,6))

# Análisis en Markdown:
# TP: X plantas enfermas detectadas ✓
# TN: Y plantas sanas detectadas ✓
# FP: Z plantas sanas incorrectamente como enfermas (inspecc. manual)
# FN: W plantas enfermas incorrectamente como sanas (RIESGO ALTO)
```

### Sección 11 (Opcional): Grad-CAM
```python
# [Implementar Grad-CAM con hooks]
# Generar attention maps
# Validar que el modelo "mira" manchas, no fondo
```

### Sección 12: Conclusión Final
```markdown
## Conclusión

El modelo final (ResNet34 + progressive learning + augmentación) alcanza:
- Accuracy: 89.56%
- F1-Score: 0.88
- Falsos Negativos: 2/100 (riesgo bajo de no detectar enfermedad)

Recomendación para Juan:
El modelo es confiable para filtrado inicial. Aumenta significativamente
de no eliminar plantas sanas (el problema original).
```

---

## 3) Código Mínimo Canónico (Copy-Paste Ready)

```python
from fastai.vision.all import *
import torch

# SETUP
set_seed(42)
print(f"GPU: {torch.cuda.is_available()}")

# DATASET
from pathlib import Path
path = Path('./esca_dataset')
classes = [d.name for d in path.iterdir() if d.is_dir()]
print(f"Clases: {classes}, Total: {len(get_image_files(path))}")

# DATABLOCK
db = DataBlock(
    blocks=(ImageBlock, CategoryBlock),
    get_items=get_image_files,
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=Resize(224),
    batch_tfms=aug_transforms()
)

# DATALOADERS
dls = db.dataloaders(path, bs=32)

# MODEL BASE
learn = vision_learner(dls, resnet18, metrics=accuracy)
learn.fine_tune(3)
print(learn.validate())

# LR FIND
learn.lr_find()

# RE-TRAIN
learn.fine_tune(4, lr_max=1e-3, cbs=[EarlyStoppingCallback(patience=3)])

# EVALUATE
interp = ClassificationInterpretation.from_learner(learn)
interp.plot_confusion_matrix()

# SAVE
learn.save('model_final')
```

---

## 4) Mejoras Obligatorias (Checklist de Implementación)

### 1. Learning Rate Óptimo (1 punto)
- [ ] Ejecutar `learn.lr_find()`
- [ ] Generar gráfica
- [ ] Justificar LR elegido en Markdown
- [ ] Re-entrenar con nuevo LR
- [ ] Comparar accuracy antes/después

### 2. Técnicas Anti-Overfitting (1 punto)
- [ ] Usar `EarlyStoppingCallback(patience=N)`
- [ ] O usar weight decay (`wd=0.1`)
- [ ] Mostrar curvas train vs. val loss
- [ ] Explicar impacto observado

### 3. Aumento de Datos (1 punto)
- [ ] Añadir `batch_tfms=aug_transforms()` en DataBlock
- [ ] Visualizar ejemplos aumentados
- [ ] Explicar en Markdown cómo mejora generalización

### 4. Progressive Learning (1 punto)
- [ ] Fase 1: Resize(128), entrenar 2-3 épocas
- [ ] Fase 2: Resize(224), continuar fine_tune
- [ ] Mostrar convergencia más rápida/mejor
- [ ] Explicar por qué funciona

### 5. Segunda Arquitectura (1 punto)
- [ ] Probar ResNet34 o ResNet50
- [ ] Crear tabla comparativa (accuracy, tiempo, parámetros)
- [ ] Conclusión sobre trade-off complejidad-precisión
- [ ] Recomendar cuál usar

### 6. Matriz de Confusión (1 punto)
- [ ] Generar `plot_confusion_matrix()`
- [ ] Explicar TP/TN/FP/FN
- [ ] Análisis especial: **Falsos negativos** (riesgo para Juan)
- [ ] Conclusión contextualizada a ESCA

---

## 5) Líneas Rojas (No Permitidas)

| Línea Roja | ¿Por Qué? | Alternativa |
|-----------|----------|-------------|
| TensorFlow/Keras | Incompatible con cuadernillos | Usar FastAI obligatoriamente |
| `from_scratch=True` | Ineficiente | `fine_tune()` (siempre) |
| Sin `set_seed(42)` | No reproducible | Añadir en setup |
| Rutas hardcodeadas | Falla en otro PC | `Path` relativa + detect automático |
| Sin `lr_find()` | LR random, no justificado | Ejecutar siempre antes |
| Sin augmentación | Sobreajuste en dataset pequeño | `batch_tfms=aug_transforms()` |
| Sin callbacks | Overfitting descontrolado | `EarlyStoppingCallback` |
| Confmatrix sin análisis | Números sin contexto | Explicar FP/FN en negocio |
| Conclusiones genéricas | No técnicas, puntos perdidos | Métricas concretas + contexto |

---

## 6) Extra Recomendado (Opcionales con Puntos)

### Interpretabilidad Grad-CAM (1 punto)
```python
# Implementar Grad-CAM
# Generar attention maps
# Mostrar dónde "mira" el modelo
# Validar: ¿manchas reales? ¿fondo? ¿sesgo?

# Conclusión: "El modelo detecta correctamente manchas en hojas,
# no correlaciona fondo/maceta. Confianza alta en decisión."
```

### Insight del Problema (1 punto)
```markdown
## Insight: Qué está aprendiendo realmente el modelo

Después de analizar Grad-CAM, descubrimos:
- Las manchas de ESCA son bien detectadas por textura de hoja.
- El modelo NO correlaciona fondo (tierra mojada) con enfermedad (buen signo).
- Ciertos tipos de ESCA (necrosis angular) son más visibles que otros (manchas pardas).

Recomendación: El modelo es seguro para producción. Baja probabilidad de sesgo.
```

---

## 7) Errores Frecuentes y Prevención

| Error | Prevención |
|-------|-----------|
| CUDA out of memory | Reducir `bs` (batch size), reiniciar kernel |
| Perfect accuracy (100%) | Revisar data leakage, validar split |
| NaN en loss | LR demasiado alto en lr_find, empezar más bajo |
| Slow training | Dataset grande + GPU débil, reducir resolución |
| FN altos (modelo no detecta ESCA) | Ajustar umbral de decisión, aumentar épocas |
| Rutas no encontradas | Usar `Path`, auto-detectar candidatos |

---

## 8) Cronograma Recomendado (Google Colab)

| Fase | Deber | Tiempo | Output |
|------|-------|--------|--------|
| 1 | Setup + dataset | 5 min | Dataset validado |
| 2 | Modelo base (502) | 15 min | `model_base.pth` |
| 3 | lr_find | 10 min | Gráfica LR |
| 4 | Re-entrenamiento LR | 15 min | `model_lr_optimized.pth` |
| 5 | Augmentación | 20 min | `model_augmented.pth` |
| 6 | Anti-overfitting | 15 min | Curvas train/val |
| 7 | Progressive learning | 30 min | `model_progressive.pth` |
| 8 | Segunda arquitectura | 25 min | Tabla comparativa |
| 9 | Matriz de confusión | 10 min | Plot + análisis |
| 10 | (Opt) Grad-CAM | 20 min | Attention maps |
| 11 | Conclusión | 10 min | Reporte final |
| **Total** | | **175 min (~3 hrs)** | **Notebook listo** |

---

## 9) Criterios de Aceptación Final

### ✅ Se Acepta Si:
- [ ] Notebook `.ipynb` funciona sin errores de principio a fin.
- [ ] Todas las 7 secciones obligatorias presentes.
- [ ] Cada sección tiene Markdown + Código + Conclusión.
- [ ] Métricas concretas (accuracy, F1, matriz).
- [ ] Comparativa de arquitecturas clara.
- [ ] Análisis de confusión contextualizado a Juan (FP/FN impacto).
- [ ] Conclusión final técnica y justificada.
- [ ] Reproducible en otro PC (sin hardcoding).

### ❌ Se Rechaza Si:
- [ ] Faltan secciones obligatorias.
- [ ] TensorFlow/Keras presente.
- [ ] Conclusiones sin métricas.
- [ ] Sobreajuste no mitigado (100% train, 50% val).
- [ ] Falsos negativos no analizados (crítico en ESCA).
- [ ] Rutas hardcodeadas (no reproducible).
- [ ] Notebook no ejecuta sin errores.

---

## 10) Instrucciones para Pedir Ayuda

Usa este formato:

> "Genera la sección **[NOMBRE]** del notebook de PIA05 usando FastAI v2. Incluye:
> 1) Explicación Markdown inicial.
> 2) Código ejecutable con comentarios.
> 3) Análisis de resultados y conclusión técnica.
> Contexto: Clasificación binaria ESCA en vides, dataset Mendeley."

Ejemplo:
> "Genera la sección **Matriz de Confusión** del notebook..."

---

## 11) Resumen en Una Frase

> **"Construye un clasificador robusto de ESCA con FastAI: DataBlock → fine_tune → lr_find → augmentación → progressive learning → confmatrix análisis → conclusión."**

