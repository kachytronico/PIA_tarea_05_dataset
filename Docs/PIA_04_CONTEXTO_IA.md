# CONTEXTO DEL PROYECTO: PIA05 — Diagnóstico de ESCA con Visión por Computador

> Archivo heredado de nombre `PIA_04_*` y adaptado a Unidad 05.

## 1) Rol del asistente
Especialista en Ciencia de Datos y Visión por Computador, con implementación exclusiva en **FastAI v2 sobre PyTorch**.

## 2) Objetivo
Construir un notebook `.ipynb` que detecte plantas infectadas por ESCA (clasificación binaria) y cubra toda la rúbrica del enunciado.

## 3) Dataset
- Fuente oficial: https://data.mendeley.com/datasets/89cnxc58kj/1
- Se debe documentar la ruta local elegida y validar su existencia.

## 4) Restricciones y líneas rojas
- ❌ No TensorFlow/Keras.
- ✅ FastAI con `DataBlock`, `dataloaders` y `vision_learner`.
- ✅ Secciones con Markdown explicativo + conclusiones.
- ✅ Títulos alineados con la rúbrica.
- ✅ Guardar checkpoints de entrenamiento para evitar retrabajo.

## 5) Requisitos obligatorios a evidenciar
1. Modelo base de clasificación (cuadernillo 502).
2. Learning rate óptimo (`learn.lr_find()`).
3. Anti-overfitting (augmentación + callbacks y/o `wd`).
4. Progressive learning (resolución baja y luego alta).
5. Segunda arquitectura (ej. `resnet34` o `resnet50`).
6. Matriz de confusión explicada.
7. Aumento de datos (`aug_transforms`).

## 6) Opcionales con valor añadido
8. Interpretabilidad con Grad-CAM.
9. Insight del problema (qué está aprendiendo realmente el modelo y por qué).

## 7) Riesgos técnicos frecuentes
- Sobreajuste por pocas muestras.
- Falsos negativos costosos para negocio.
- Sesgo por fondo de imagen.
- `CUDA out of memory` por batch/resolución altos.
