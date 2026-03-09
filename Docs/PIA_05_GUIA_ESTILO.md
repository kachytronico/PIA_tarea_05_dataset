# GUÍA DE ESTILO — PIA05 (Visión por Computador)

> Unidad 05: Visión Artificial | Problema 1: Clasificación con FastAI

---

## 1) Estructura Obligatoria del Notebook

El notebook debe tener exactamente estas secciones, **en este orden**:

### 1. **Contexto del Problema** (Markdown)
```markdown
## Problema 1: Sistema de reconocimiento de ESCA

Juan es un agricultor que tiene cientos de vides... [breve resumen]
Objetivo: Construir un clasificador binario que determine si una planta está infectada.
```

### 2. **Setup de Entorno** (Código + Markdown)
- Importar FastAI: `from fastai.vision.all import *`
- Detectar GPU: `torch.cuda.is_available()`
- Fijar semilla: `set_seed(42)`
- Detectar rutas automáticamente (sin hardcoding)

### 3. **Carga y Validación del Dataset** (Código + Visualización)
- Ruta local del dataset
- Verificación de clases (`esca`, `healthy`)
- Conteo total de imágenes
- Visualización de muestras

### 4. **Modelo Base (Cuadernillo 502)** (Código + Conclusión)
- `DataBlock` con parámetros mínimos
- `ResNet18` + `fine_tune(3)`
- Métricas de accuracy y loss

### 5. **Learning Rate Óptimo** (Código + Gráfica + Análisis)
- `learn.lr_find()` con gráfica
- Justificación del LR elegido
- Re-entrenamiento con LR optimizado

### 6. **Aumento de Datos** (Código + Visualización)
- `batch_tfms=aug_transforms()`
- Visualizar transformaciones aplicadas
- Explicar impacto en generalización

### 7. **Prevención de Sobreajuste** (Código + Análisis)
- `EarlyStoppingCallback` o weight decay
- Curvas de entrenamiento (train vs. val loss)
- Análisis de impacto

### 8. **Progressive Learning** (Código + Comparación)
- Fase 1: `Resize(128)` (2-3 épocas)
- Fase 2: `Resize(224)` + `fine_tune`
- Comparativa de convergencia

### 9. **Segunda Arquitectura** (Código + Tabla Comparativa)
- `ResNet34` o `ResNet50`
- Tabla: accuracy, tiempo, parámetros
- Conclusión sobre trade-off

### 10. **Matriz de Confusión** (Código + Análisis Detallado)
- `ClassificationInterpretation.from_learner(learn)`
- `plot_confusion_matrix(figsize=(5,5))`
- Párrafo explicativo: FP, FN, contexto agrícola

### 11. **(Opcional) Interpretabilidad (Grad-CAM)** (Código + Visualización)
- Implementar Grad-CAM
- Generar attention maps
- Validar que detecta manchas (no fondo)

### 12. **Conclusión Final** (Markdown)
- Resumen de mejoras
- Métrica final de accuracy/F1
- Recomendación para Juan

---

## 2) Reglas de Código

### 2.1 Imports y Setup
```python
# ✅ CORRECTO
from fastai.vision.all import *
import torch
import gc

set_seed(42)
print(f"GPU disponible: {torch.cuda.is_available()}")

# ❌ INCORRECTO
import tensorflow as tf  # NO
from keras import *      # NO
import torch.nn as nn    # Evitar (usar FastAI)
```

### 2.2 Rutas de Archivo
```python
# ✅ CORRECTO
from pathlib import Path
path = Path('./esca_dataset/esca')
if not path.exists():
    path = Path('/content/PIA_tarea_05_dataset/esca_dataset/esca')

# ❌ INCORRECTO
path = 'C:\\Users\\alfle\\Documentos\\...'  # Hardcoded, no reproducible
path = '/home/user/local/path'              # Hardcoded local
```

### 2.3 Variables Claras y Reutilizables
```python
# ✅ CORRECTO
learn_base = vision_learner(dls, resnet18, metrics=accuracy)
learn_base.fine_tune(3)
learn_base.save('stage_1_base')

learn_res34 = vision_learner(dls, resnet34, metrics=accuracy)
learn_res34.load('stage_1_base')  # Cargar pesos previos si aplica

# ❌ INCORRECTO
l = vision_learner(dls, resnet18)  # Variable poco clara
fit(3)  # Método sin objeto
```

### 2.4 Funciones Auxiliares (Reutilizables)
```python
# ✅ CORRECTO
def plot_augmented_samples(dls, n_samples=6):
    dls.show_batch(max_n=n_samples, figsize=(12, 8))

def compare_models(model1_acc, model1_time, model2_acc, model2_time):
    print(f"Model 1: Acc={model1_acc:.3f}, Time={model1_time}s")
    print(f"Model 2: Acc={model2_acc:.3f}, Time={model2_time}s")

# Luego reutilizar
plot_augmented_samples(dls, 9)
compare_models(acc_base, 120, acc_res34, 140)
```

---

## 3) Reglas de Entrenamiento

### 3.1 Learning Rate Find (Obligatorio)
```python
# ✅ CORRECTO
learn.lr_find()
# [gráfica generada]
# Comentario: "Elegimos LR = 1e-3 (justo antes del valle mínimo)"

learn.fine_tune(4, lr_max=1e-3)

# ❌ INCORRECTO
learn.fine_tune(4)  # Sin lr_find previo
learn.fine_tune(4, lr_max=0.1)  # LR demasiado alto
```

### 3.2 Augmentación (Obligatoria)
```python
# ✅ CORRECTO
db = DataBlock(
    blocks=(ImageBlock, CategoryBlock),
    get_items=get_image_files,
    splitter=RandomSplitter(valid_pct=0.2, seed=42),
    get_y=parent_label,
    item_tfms=Resize(224),
    batch_tfms=aug_transforms()  # ← AÍ
)

# ❌ INCORRECTO
db = DataBlock(...)  # Sin batch_tfms
item_tfms=RandomResizedCrop(224)  # Solo item_tfms, no augmentación
```

### 3.3 Anti-Overfitting (Obligatorio, al menos uno)
```python
# ✅ CORRECTO — Opción A: EarlyStoppingCallback
learn.fine_tune(10, cbs=[EarlyStoppingCallback(patience=3)])

# ✅ CORRECTO — Opción B: Weight Decay
learn.fine_tune(10, wd=0.1)

# ✅ CORRECTO — Opción C: Ambas (recomendado)
learn.fine_tune(10, wd=0.1, cbs=[EarlyStoppingCallback(patience=3)])

# ❌ INCORRECTO
learn.fine_tune(3)  # Sin control de overfitting
```

### 3.4 Checkpoint y Salvaguarda
```python
# ✅ CORRECTO
learn.save('checkpoint_base_model')
learn.save('checkpoint_lr_optimized')
learn.save('checkpoint_augmented')

# Cargar si es necesario
learn.load('checkpoint_augmented')

# ❌ INCORRECTO
# No guardar nada (retrabajo si falla)
```

---

## 4) Reglas de Análisis

### 4.1 Métricas Concretas (NO Genéricas)
```python
# ✅ CORRECTO
print(f"Accuracy del modelo base: {preds[0]:.4f}")
print(f"Loss final: {learn.get_preds()[1].min():.4f}")
print(f"Tiempo de entrenamiento: {elapsed_time:.2f} segundos")

# ❌ INCORRECTO
print("El modelo funciona bien")
print("Excelentes resultados")
```

### 4.2 Matriz de Confusión (Análisis Obligatorio)
```python
# ✅ CORRECTO
interp = ClassificationInterpretation.from_learner(learn)
interp.plot_confusion_matrix(figsize=(5,5))

# Análisis en Markdown:
"""
## Interpretación de la Matriz de Confusión

- **TP (Verdaderos Positivos)**: 45 plantas enfermas detectadas correctamente.
- **TN (Verdaderos Negativos)**: 52 plantas sanas detectadas correctamente.
- **FP (Falsos Positivos)**: 8 plantas sanas clasificadas como enfermas → Juan hace inspección extra (aceptable).
- **FN (Falsos Negativos)**: 3 plantas enfermas clasificadas como sanas → Riesgo de contagio (CRÍTICO).

Conclusión: El modelo es conservador (prefiere "falsa alarma" a dejar pasar enfermedad).
Recomendación para Juan: Confianza media-alta en predicciones positivas (enfermedad).
"""

# ❌ INCORRECTO
interp.plot_confusion_matrix()  # Sin análisis textual
```

### 4.3 Comparativa de Modelos
```python
# ✅ CORRECTO
import pandas as pd

results = pd.DataFrame({
    'Modelo': ['ResNet18', 'ResNet34', 'ResNet50'],
    'Accuracy': [0.8742, 0.8956, 0.9112],
    'Tiempo (s)': [120, 145, 165],
    'Parámetros': ['11M', '21M', '25M']
})

print(results.to_string())
# Conclusión: ResNet34 es el mejor trade-off.

# ❌ INCORRECTO
print("ResNet34 es mejor que ResNet18")  # Sin datos
```

---

## 5) Reglas de Redacción (Markdown)

### 5.1 Estructura de Conclusiones
```markdown
## Sección: Learning Rate Óptimo

[Código ejecutable]

### Análisis

La gráfica de `lr_find()` muestra:
- De 1e-5 a 1e-3: Pérdida decreciente (zona de aprendizaje).
- 1e-3: Valle mínimo (pérdida más baja).
- 1e-3 a 1e-2: Pérdida creciente (divergencia).

**LR elegido**: 1e-3 (justo antes del valle).

### Resultado tras re-entrenamiento

Accuracy anterior: 0.82
Accuracy nuevo: 0.87
**Mejora**: +5 puntos porcentuales.

### Conclusión

El learning rate óptimo aceleró convergencia y mejoró estabilidad del entrenamiento.
La zona de "valle mínimo" es crítica: valores mayores divergen, menores son lentos.
```

### 5.2 Longitud de Conclusiones
- **Mínimo**: 3-4 líneas técnicas.
- **Máximo**: 8 líneas (evitar divagaciones).
- **Estructura**: Observación → Análisis → Conclusión.

```markdown
# ❌ INCORRECTO (muy genérico)
"El modelo mejora con data augmentation."

# ✅ CORRECTO (específico)
"La augmentación aumentó accuracy de 0.82 a 0.85 (+3 p.p.). 
Las transformaciones (rotación ±10°, brillo ±0.1) fuerzan robustez ante variaciones 
de iluminación en campo. Esto es crítico para ESCA: las manchas de infección 
son visibles con diferentes ángulos de luz."
```

### 5.3 Títulos Alineados con la Rúbrica
```markdown
# ✅ CORRECTO
## 1. Modelo Básico de Clasificación (Cuadernillo 502)
## 2. Learning Rate Óptimo
## 3. Técnicas Anti-Overfitting
## 4. Progressive Learning
## 5. Segunda Arquitectura
## 6. Matriz de Confusión
## 7. Aumento de Datos

# ❌ INCORRECTO
## Mi Modelo Propuesto
## Entrenamiento
## Resultados
```

---

## 6) Reglas de Visualización

### 6.1 Gráficas Claras
```python
# ✅ CORRECTO
learn.recorder.plot_loss()
plt.title("Curvas de Entrenamiento (Base Model)")
plt.xlabel("Epoch")
plt.ylabel("Loss")
plt.legend(['Training', 'Validation'])
plt.grid(True)
plt.show()

# ❌ INCORRECTO
learn.recorder.plot_loss()  # Sin título ni ejes claros
```

### 6.2 Matriz de Confusión
```python
# ✅ CORRECTO
interp.plot_confusion_matrix(figsize=(6,6))
plt.title("Matriz de Confusión — ResNet18")
plt.show()

# ❌ INCORRECTO
interp.plot_confusion_matrix(figsize=(3,3))  # Muy pequeña
```

---

## 7) Líneas Rojas (No Negociables)

| Línea Roja | ✅ Permitido | ❌ Prohibido |
|-----------|-----------|-----------|
| **Librería** | FastAI v2, PyTorch | TensorFlow, Keras, scikit-learn |
| **Training** | `fine_tune`, transfer learning | `from_scratch=True`, entrenar desde cero |
| **Splits** | `RandomSplitter(valid_pct=0.2)` | Mix train/val, sin semilla |
| **Augmentación** | `batch_tfms=aug_transforms()` | Sin augmentación o solo item_tfms |
| **LR** | `learn.lr_find()` + gráfica | LR random sin justificación |
| **Callbacks** | `EarlyStoppingCallback`, `wd` | Sin control de overfitting |
| **Arquitecturas** | ResNet18, ResNet34, ResNet50 | Custom CNN sin justificación |
| **Confmatrix** | `plot_confusion_matrix()` + análisis | Solo plot, sin interpretación |
| **Conclusiones** | Con métricas y análisis | Genéricas, sin soporte |
| **Reproducibilidad** | `set_seed(42)`, rutas relativas | Hardcoding, no reproducible |

---

## 8) Checklist de Estilo (Antes de Entregar)

- [ ] Notebook organizado en 12 secciones (o 10 si sin opcionales).
- [ ] Cada sección tiene Markdown + Código + Conclusión.
- [ ] Todos los títulos alineados con la rúbrica.
- [ ] Sin imports de TensorFlow/Keras.
- [ ] `set_seed(42)` en setup.
- [ ] Rutas del dataset detectadas automáticamente (sin hardcoding).
- [ ] `learn.lr_find()` ejecutado y LR justificado.
- [ ] Augmentación implementada (`batch_tfms`).
- [ ] Anti-overfitting aplicado (callback o wd).
- [ ] Progressive learning con dos resoluciones.
- [ ] Segunda arquitectura con tabla comparativa.
- [ ] Matriz de confusión con análisis de FP/FN.
- [ ] Todas las conclusiones con métricas concretas.
- [ ] Notebook funciona de inicio a fin sin errores.

---

## 9) Ejemplo Completo de Sección

```markdown
# 3. Técnicas Anti-Overfitting

En ciencia de datos, la memorización de datos de entrenamiento es un enemigo silencioso.
El modelo puede alcanzar 99% en entrenamiento pero fallar en datos nuevos.

## Implementación

Aplicamos dos técnicas:
1. **EarlyStoppingCallback**: Detiene el entrenamiento si la pérdida de validación 
   deja de mejorar durante 3 épocas consecutivas.
2. **Weight Decay (wd=0.1)**: Penaliza pesos grandes, forzando consistencia.
```

```python
# Entrenamiento con ambas técnicas
learn.fine_tune(
    10,
    lr_max=1e-3,
    wd=0.1,
    cbs=[EarlyStoppingCallback(patience=3)]
)
```

## Resultados

Las curvas de entrenamiento muestran:
- **Sin Anti-overfitting**: Train loss = 0.1, Val loss = 0.8 (diferencia alta).
- **Con Anti-overfitting**: Train loss = 0.3, Val loss = 0.35 (convergencia equilibrada).

Mejora en accuracy de validación: 0.82 → 0.86 (+4 p.p.).

## Conclusión

Las dos técnicas combinadas fuerzan robustez generalista del modelo.
`EarlyStoppingCallback` es crítica en datasets pequeños (ESCA tiene ~100 imágenes por clase).
El weight decay es especialmente útil en ResNet50 (más parámetros = más riesgo de memorización).
```

---

## 10) Resumen Final

**El notebook debe ser**:
- ✅ Claro (títulos alineados con rúbrica).
- ✅ Reproducible (sin hardcoding, semilla fija).
- ✅ Técnico (conclusiones con métricas).
- ✅ Defensible (cada decisión justificada).
- ✅ Completo (todas las 7 secciones obligatorias).

**En caso de duda**: Usa la estructura anterior como plantilla exacta.

