# GUÍA DE ESTILO — PIA05 (Visión por Computador)

> Archivo heredado de nombre `PIA_04_*` y adaptado a Unidad 05.

## 1) Estructura obligatoria del notebook
1. **Setup de entorno** (imports, semilla, detección de `device`).
2. **Carga y verificación del dataset ESCA**.
3. **Modelo base** (FastAI cuadernillo 502).
4. **`lr_find` + entrenamiento ajustado**.
5. **Aumento de datos + anti-overfitting**.
6. **Progressive learning**.
7. **Nueva arquitectura + comparativa**.
8. **Matriz de confusión + interpretación de errores**.
9. **(Opcional) Grad-CAM + conclusión final**.

## 2) Reglas de código
- Usar `from fastai.vision.all import *`.
- No usar rutas absolutas del equipo; preferir rutas relativas o variables de entorno.
- Definir variables claras: `path`, `dls`, `learn_base`, `learn_res34`, `interp`.
- Mantener funciones auxiliares pequeñas y reutilizables.

## 3) Reglas de entrenamiento
- Ejecutar `learn.lr_find()` antes del ajuste principal.
- Añadir augmentación con `aug_transforms()`.
- Incluir al menos una técnica anti-overfitting:
  - `EarlyStoppingCallback(patience=...)`
  - `SaveModelCallback(...)`
  - `wd=...`
- Para progressive learning, entrenar en tamaño pequeño y continuar en mayor resolución.

## 4) Reglas de análisis
- Incluir `ClassificationInterpretation.from_learner(learn)`.
- Mostrar matriz de confusión.
- Explicar explícitamente:
  - impacto de falsos positivos,
  - impacto de falsos negativos,
  - coste real para el agricultor.

## 5) Regla de redacción
- Conclusiones breves (4–8 líneas), técnicas y con métricas concretas.
- Evitar frases genéricas sin respaldo en outputs.
