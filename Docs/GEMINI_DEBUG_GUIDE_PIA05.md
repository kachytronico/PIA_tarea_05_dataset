# GEMINI — GUÍA DE DEPURACIÓN (PIA05)

Este documento define cómo debo ayudarte cuando surgen errores o resultados inesperados
en la Tarea PIA05 (Visión por Computador con FastAI).

---

## 1. Errores Técnicos Comunes en PIA05

### Error 1: CUDA Out of Memory
**Síntoma**: `RuntimeError: CUDA out of memory`

**Causas probables**:
- Batch size demasiado grande (bs=128 o más)
- Resolución de imagen muy alta (>512)
- Modelo muy pesado (ResNet50+ con mucha GPU débil)

**Solución**:
1. Reducir batch size: `bs=16` o `bs=8`
2. Reiniciar kernel (muy importante: borra RAM caché)
3. Reducir resolución: `Resize(128)` en lugar de `Resize(224)`
4. Usar Google Colab Pro (100 compute units)

**Código de sanidad**:
```python
import torch
import gc
print(f"GPU Available: {torch.cuda.is_available()}")
torch.cuda.empty_cache()
gc.collect()
```

---

### Error 2: Loss = NaN en lr_find()
**Síntoma**: Gráfica muestra pérdida = NaN en resolución

**Causas probables**:
- Learning rate inicial demasiado alto
- Dataset está corrupto o mal normalizado
- Bug en augmentación (valores fuera de rango)

**Solución**:
1. Elegir LR en zona "plana" (pre-valle), no en la divergencia
2. Empezar con LR más bajo si falla: `1e-5` en lugar de `1e-4`
3. Verificar que no hay imágenes corruptas: `len(get_image_files(path))`
4. Probar sin augmentación temporalmente: comentar `batch_tfms=aug_transforms()`

**Código diagnóstico**:
```python
preds, _ = learn.get_preds()
print(f"Predicciones: min={preds.min()}, max={preds.max()}, mean={preds.mean()}")
# Si hay NaN o Inf, hay problema arriba
```

---

### Error 3: Accuracy Perfecta (100%) en Validación
**Síntoma**: `Accuracy = 1.0 o 0.99+` sin explicación

**Causas probables**:
- **Data leakage**: Las imágenes de test se parecen demasiado a las de train
- Dataset muy pequeño (todas las imágenes son casi idénticas)
- Split defectuoso (train y val no son realmente disjuntos)

**Investigación**:
```python
# Verificar split
print(f"Train items: {len(dls.train)}")
print(f"Valid items: {len(dls.valid)}")
print(f"Total: {len(get_image_files(path))}")
print(f"Ratio: {len(dls.valid) / (len(dls.train) + len(dls.valid))}")
# Debe ser ~0.2 (20% validation)

# Si ratio está distinto, el split es incorrecto
```

**Solución**:
- Verificar que `RandomSplitter(valid_pct=0.2, seed=42)` se aplicó correctamente
- Revisar que las imágenes train y val son realmente diferentes (no copias)
- Aumentar tamaño de dataset si es demasiado pequeño

---

### Error 4: Train Loss Baja, Val Loss Sube (Overfitting Severo)
**Síntoma**: Gráfica muestra divergencia: train ↓, val ↑

**Causas**:
- Modelo demasiado complejo para dataset pequeño
- Falta augmentación o regularización
- Demasiadas épocas sin detener

**Solución**:
```python
# Aplicar AMBAS técnicas anti-overfitting
learn.fine_tune(
    15,  # máximo, pero Early Stopping puede detener antes
    lr_max=1e-3,
    wd=0.2,  # más regularización si problema es severo
    cbs=[EarlyStoppingCallback(patience=3)]
)
```

---

### Error 5: Loss Baja Muy Lentamente (No Converge)
**Síntoma**: Después de 10 épocas, loss sigue en 0.8, sin mejorar

**Causas**:
- Learning rate demasiado bajo
- Dataset desbalanceado (una clase domina)
- Arquitectura insuficiente

**Solución**:
- Verificar LR: ejecutar `lr_find()` nuevamente
- Contar clases: `print(pd.Series([d.parent.name for d in dls.train_ds]).value_counts())`
- Si hay desbalance >80/20, considerar pesan de clases o stratified split

---

## 2. Señales de Alerta (Red Flags)

| Señal | Problema | Acción |
|-------|---------|--------|
| Accuracy = 1.0 (100%) | Leakage o dataset trivial | Investigar split y diversidad |
| Loss = NaN | LR demasiado alto | Bajar LR, reiniciar kernel |
| CUDA OOM | Memoria agotada | Reducir bs, reiniciar |
| Train/val divergentes | Overfitting severo | Aumentar wd, añadir callback |
| Accuracy no mejora | Loss stuck | Aumentar LR, revisar desbalance |

---

## 3. Matriz de Confusión: Interpretación

Si la matriz muestra:
- **Muchos FN (falsos negativos)**: Modelo es demasiado conservador, no detecta enfermedad
  - Solución: Ajustar umbral de decisión o aumentar threshold de probabilidad
- **Muchos FP (falsos positivos)**: Modelo es demasiado agresivo, ve enfermedad en sanas
  - Solución: Aumentar rigor del modelo (más wd, menos épocas)

**Para ESCA**: FN es CRÍTICO → priorizar sensibilidad (recall) > especificidad

---

## 4. Código Diagnóstico Rápido

Úsalo cuando algo no cierre:

```python
# 1. Verificar GPU
print(f"GPU: {torch.cuda.is_available()}")
print(f"GPU Memory: {torch.cuda.mem_get_info()}")

# 2. Verificar dataset
path = Path('./esca_dataset/esca')
print(f"Tot images: {len(get_image_files(path))}")
print(f"Classes: {[d.name for d in path.parent.iterdir() if d.is_dir()]}")

# 3. Verificar dataloaders
dls.show_batch(max_n=4)  # Ver si las imágenes se cargan bien
print(f"Batch shape: {dls.one_batch()[0].shape}")

# 4. Verificar predicciones
preds, _ = learn.get_preds()
print(f"Preds shape: {preds.shape}")
print(f"Min/Max: {preds.min()}, {preds.max()}")
print(f"NaN count: {preds.isnan().sum()}")

# 5. Verificar matriz
cm = interp.confusion_matrix()
print(cm)
```

---

## 5. Estilo de Ayuda (Cómo Respondo)

Cuando encuentro un error:

1. **Identificar** la causa probable (2-3 líneas)
2. **Explicar** por qué sucede (1-2 líneas)
3. **Solucionar** con código concreto (no reescribir todo)
4. **Validar** con diagnóstico (cómo verificar que se arregló)

**Ejemplo:**

> Error: `CUDA out of memory`
> 
> Causa: Batch size de 64 es demasiado para ResNet34 en esta GPU.
> 
> Solución: Cambiar `bs=64` → `bs=32`
>
> Verificación: Ejecutar entrenamiento nuevamente, debe completarse sin OOM.

---

## 6. Cuándo NO Reescribir Todo

Si el error es pequeño, **doy feedback puntual**:
- ❌ NO: "Aqui te pongo el código arreglado de principio a fin"
- ✅ SÍ: "Línea X: cambia `bs=64` a `bs=32` porque [razón]"

**Excepciones** donde sí reescribir:
- Cambio fundamental de arquitectura
- Reestructura de DataBlock (afecta a todo)
- Cuando el usuario lo pide explícitamente

---

## 7. Recursos para Autoayuda

Si el notebook da error:
1. Leer el mensaje de error completo (no solo primera línea)
2. Buscar en Google: `fastai [error message]`
3. Revisar documentación: https://docs.fast.ai/
4. Consultar GEMINI_CONTEXT_PIA05.md/GEMINI_STYLE_GUIDE_PIA05.md aquí en `/Docs/`

---

**Filosofía**: Te ayudo a entender Y arreglá el problema, no a hacer magia negra.

