# Contexto operativo — PIA05 (Problema 1: Visión por Computador)

## 1) Rol del asistente

Actúa como especialista en Ciencia de Datos y Visión por Computador **usando exclusivamente FastAI v2 sobre PyTorch**.

Objetivo: construir un cuaderno Jupyter (`.ipynb`) que resuelva el problema de reconocimiento de ESCA y cumpla la rúbrica completa.

---

## 2) Reglas estrictas (líneas rojas)

- ❌ No usar TensorFlow ni Keras.
- ✅ Todo el código debe implementarse con FastAI.
- ❌ No omitir explicaciones.
- ✅ Incluir celdas Markdown con explicación técnica y conclusiones (especialmente en matriz de confusión e interpretabilidad).
- ✅ Respetar títulos literales solicitados en el enunciado.
- ✅ Usar patrón típico de FastAI: `DataBlock`, `dataloaders`, `vision_learner`.

---

## 3) Enunciado del problema (literal)

Juan es un agricultor que tiene cientos de vides y, cada año, se ve obligado a eliminar un alto porcentaje de plantas sanas por infecciones de ESCA. Se necesita un sistema experto que determine qué plantas están infectadas y cuáles no.

Dataset oficial: https://data.mendeley.com/datasets/89cnxc58kj/1

### Hitos evaluables

1. Modelo básico de clasificación de imagen (cuadernillo 502). **(4 puntos)**
2. Obtener learning rate óptimo. **(1 punto)**
3. Aplicar técnicas para evitar sobreajuste. **(1 punto)**
4. Aplicar progressive learning. **(1 punto)**
5. Probar otra arquitectura. **(1 punto)**
6. Mostrar y explicar matriz de confusión. **(1 punto)**
7. Aplicar aumento de datos. **(1 punto)**

### Opcional

8. Interpretabilidad sobre gradiente (cuadernillo 506). **(1 punto)**
9. Descubrir qué está pasando en el problema (insight). **(1 punto)**

---

## 4) Guía técnica de implementación

### 4.1 Configuración inicial

- Importar FastAI:

```python
from fastai.vision.all import *
```

- El notebook debe indicar cómo acceder al dataset (ruta local tras descarga de Mendeley).

### 4.2 Modelo base (cuadernillo 502)

Implementar `DataBlock` con esta estructura mínima:

```python
db = DataBlock(
     blocks=(ImageBlock, CategoryBlock),
     get_items=get_image_files,
     splitter=RandomSplitter(valid_pct=0.2, seed=42),
     get_y=parent_label,
     item_tfms=Resize(224)
)
```

Entrenamiento base:

```python
dls = db.dataloaders(path)
learn = vision_learner(dls, resnet18, metrics=accuracy)
learn.fine_tune(3)
```

### 4.3 Mejoras obligatorias (progresivas)

1. **Learning rate óptimo**
    - Usar `learn.lr_find()`.
    - Elegir LR razonable según la gráfica y usarlo en entrenamiento.

2. **Aumento de datos**
    - Añadir `batch_tfms=aug_transforms()` en el `DataBlock`.
    - Explicar en Markdown cómo mejora la generalización.

3. **Evitar sobreajuste**
    - Mantener data augmentation.
    - Añadir `EarlyStoppingCallback(patience=3)`.
    - Se puede incluir `wd` (weight decay).

4. **Progressive learning**
    - Paso A: entrenar con tamaño pequeño (ej. `Resize(128)`).
    - Paso B: reemplazar `dls` por otro con mayor resolución (ej. `Resize(224)`).
    - Paso C: continuar `fine_tune`.

5. **Otra arquitectura**
    - Repetir con `resnet34` o `resnet50`.
    - Comparar resultados con el modelo base.

6. **Matriz de confusión + interpretación**
    - Generar:

```python
interp = ClassificationInterpretation.from_learner(learn)
interp.plot_confusion_matrix(figsize=(5,5))
```

    - Concluir en Markdown el impacto de:
      - Falsos positivos (planta sana clasificada como enferma).
      - Falsos negativos (planta enferma clasificada como sana).

### 4.4 Opcional: interpretabilidad (cuadernillo 506)

Objetivo: aplicar Grad-CAM para verificar dónde “mira” la red.

Pasos esperados:

1. Tomar una imagen de validación/test.
2. Capturar activaciones y gradientes de la última capa convolucional.
3. Ejecutar backward sobre la clase predicha.
4. Construir heatmap y superponerlo con `matplotlib`.
5. Concluir si el modelo mira manchas reales de la hoja (deseable) o fondo/maceta/tierra (fuga de datos).

Hook base de referencia:

```python
class Hook():
     def __init__(self, m): self.hook = m.register_forward_hook(self.hook_func)
     def hook_func(self, m, i, o): self.stored = o.detach().clone()
     def __enter__(self, *args): return self
     def __exit__(self, *args): self.hook.remove()

class HookBwd():
     def __init__(self, m): self.hook = m.register_backward_hook(self.hook_func)
     def hook_func(self, m, gi, go): self.stored = go[0].detach().clone()
     def __enter__(self, *args): return self
     def __exit__(self, *args): self.hook.remove()
```

---

## 5) Criterio de entrega del notebook

El cuaderno final debe:

- Estar organizado por secciones con títulos claros.
- Alternar celdas de código y Markdown explicativo.
- Incluir comparativas y conclusiones justificadas.
- Mantener estilo técnico, ordenado y reproducible.

---

## 6) Plantilla de instrucción reutilizable (para pedir ayuda al asistente)

Puedes reutilizar este bloque tal cual cuando quieras generar partes del notebook:

> "Genera la sección **[NOMBRE DE LA SECCIÓN]** del notebook de PIA05 usando FastAI v2 (sin TensorFlow/Keras). Incluye: 1) celda Markdown explicativa, 2) celda(s) de código ejecutable, 3) breve conclusión técnica de resultados, manteniendo coherencia con el dataset ESCA y la rúbrica del enunciado." 