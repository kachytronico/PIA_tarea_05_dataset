# PIA05 — Guía Operativa Óptima (Problema 1: ESCA)

> Archivo heredado de nombre `PIA04_*` y adaptado a Unidad 05.

## 1) Entrega esperada
- Notebook `.ipynb` completo, organizado por secciones.
- Alternancia de celdas de código y Markdown técnico.
- Evidencia de todos los hitos obligatorios.

## 2) Plantilla recomendada por secciones
1. Contexto del problema y objetivo.
2. Setup e imports FastAI.
3. Carga de dataset y validación de clases.
4. Modelo base (502).
5. LR óptimo (`lr_find`).
6. Data augmentation.
7. Anti-overfitting.
8. Progressive learning.
9. Segunda arquitectura.
10. Matriz de confusión + análisis.
11. (Opcional) Grad-CAM.
12. Conclusión final.

## 3) Código mínimo canónico
```python
from fastai.vision.all import *

db = DataBlock(
    blocks=(ImageBlock, CategoryBlock),
    get_items=get_image_files,
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=Resize(224)
)

dls = db.dataloaders(path, bs=32)
learn = vision_learner(dls, resnet18, metrics=accuracy)
learn.fine_tune(3)
```

## 4) Mejoras obligatorias
- `learn.lr_find()` y justificar LR elegido.
- `batch_tfms=aug_transforms()`.
- `EarlyStoppingCallback` y/o `wd`.
- Progressive learning con dos resoluciones.
- Nueva arquitectura (`resnet34` o `resnet50`) + comparativa.
- Matriz de confusión + explicación de FP/FN.

## 5) Líneas rojas
- No usar TensorFlow/Keras.
- No dejar secciones sin explicación.
- No sacar conclusiones sin métricas.
- No ignorar falsos negativos (riesgo alto en ESCA).

## 6) Extra recomendado (opcional)
- Grad-CAM para validar foco visual del modelo.
- Insight sobre sesgos de fondo o iluminación.
