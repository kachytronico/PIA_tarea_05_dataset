# GEMINI — CHECKLIST DE REVISIÓN PIA05

Este checklist sirve como guía para revisar la calidad técnica, narrativa y completud
del notebook PIA_05_Tarea_01.ipynb antes de la entrega final.

---

## 1. Estructura General

- [ ] El notebook existe y se llama `PIA_05_Tarea_01.ipynb`
- [ ] Tiene 10+ secciones (incluy Conclusión)
- [ ] Los títulos son literales (no resúmenes inventados)
- [ ] Cada sección tiene: Markdown + Código + Conclusión

---

## 2. Secciones Obligatorias (10 Puntos)

### Sección 1: Contexto del Problema
- [ ] Explica qué es ESCA y por qué importa
- [ ] Menciona impacto en Juan (agricultor)
- [ ] Dataset referenciado (Mendeley)

### Sección 2: Setup de Entorno
- [ ] Imports: `from fastai.vision.all import *`
- [ ] `set_seed(42)` presente
- [ ] GPU detectada con `torch.cuda.is_available()`
- [ ] NO rutas absoltas/hardcodeadas

### Sección 3: Carga Dataset
- [ ] Ruta auto-detectada (no hardcodeada)
- [ ] Clases validadas (`esca`, `healthy`)
- [ ] Conteo de imágenes por clase
- [ ] `dls.show_batch()` visualiza ejemplos

### Sección 4: Modelo Base (502) — ⭐ 4 PUNTOS
- [ ] `DataBlock` con parámetros correctos:
  - [ ] `blocks=(ImageBlock, CategoryBlock)`
  - [ ] `get_items=get_image_files`
  - [ ] `splitter=RandomSplitter(valid_pct=0.2, seed=42)`
  - [ ] `get_y=parent_label`
  - [ ] `item_tfms=Resize(224)`
- [ ] `vision_learner` con `resnet18`
- [ ] `learn.fine_tune(3)` ejecutado
- [ ] Modelo guardad: `learn.save('checkpoint_base')`
- [ ] Conclusión con accuracy concreto (ej. 82.4%)
- [ ] Conclusion en primera persona ("observo", "veo")

### Sección 5: Learning Rate Óptimo — ⭐ 1 PUNTO
- [ ] `learn.lr_find()` ejecutado
- [ ] Gáfica mostrada
- [ ] LR elegido justificado ("pre-valle", "1e-3")
- [ ] Nuevo entrenamiento: `learn.fine_tune(4, lr_max=...)`
- [ ] Modelo guardado: `learn.save('checkpoint_lr_optimized')`
- [ ] Conclusión con mejora en accuracy (+X p.p.)
- [ ] Explicación por qué LR óptimo importa

### Sección 6: Aumento de Datos — ⭐ 1 PUNTO
- [ ] `batch_tfms=aug_transforms()` en DataBlock
- [ ] DataLoaders recreados con augmentación
- [ ] `dls_aug.show_batch()` visualiza transformaciones
- [ ] Modelo con augmentación: `learn.fine_tune(...)`
- [ ] Modelo guardado: `learn.save('checkpoint_augmented')`
- [ ] Conclusión explica qué es augmentación y por qué funciona

### Sección 7: Anti-Overfitting — ⭐ 1 PUNTO
- [ ] `EarlyStoppingCallback(patience=3)` implementado Y/O
- [ ] `wd=0.1` (weight decay) implementado
- [ ] Idealmente AMBAS técnicas
- [ ] `learn.recorder.plot_loss()` muestra curvas
- [ ] Análisis visual: train/val convergen (no divergen mucho)
- [ ] Modelo guardado
- [ ] Conclusión comenta diferencia train/val

### Sección 8: Progressive Learning — ⭐ 1 PUNTO
- [ ] **Fase 1**: DataBlock con `Resize(128)`, entrenar 2-3 épocas
- [ ] **Fase 2**: Cambiar a `Resize(224)`, continuar entrenamiento
- [ ] Mostrar que Fase 2 refina los detalles aprendidos en Fase 1
- [ ] Comparar tiempo/convergencia
- [ ] Modelo guardado tras Fase 2
- [ ] Conclusión explica estrategia jerárquica (formas → detalles)

### Sección 9: Segunda Arquitectura — ⭐ 1 PUNTO
- [ ] Usar `ResNet34` o `ResNet50` (NO ResNet18)
- [ ] Mismos parámetros: augmentación, LR, callbacks
- [ ] Ejecutar `lr_find()` para nueva arquitectura
- [ ] Entrenar y guardar modelo
- [ ] **Tabla comparativa**:
  ```
  | Modelo   | Accuracy | Tiempo | Parámetros |
  |----------|----------|--------|-----------|
  | ResNet18 | 0.XX     | 120s   | 11M        |
  | ResNet34 | 0.XX     | 145s   | 21M        |
  ```
- [ ] Conclusión: trade-off complejidad vs. precisión
- [ ] Recomendación: cuál usar en producción

### Sección 10: Matriz de Confusión — ⭐ 1 PUNTO
- [ ] `ClassificationInterpretation.from_learner(learn)`
- [ ] `plot_confusion_matrix(figsize=(6,6))`
- [ ] Extraer números: TP, TN, FP, FN
- [ ] Calcular métricas: Accuracy, Sensibilidad (Recall), Especificidad
- [ ] **Análisis contextual a Juan:**
  - [ ] TP/TN explicados (positivo)
  - [ ] FP explicado (tolerable, inspección manual)
  - [ ] **FN explicado (CRÍTICO, riesgo conagio)**
  - [ ] Recomendación: ¿Es modelo producible? ¿Confianza?
- [ ] Conclusión en primera persona (observo, noto)

---

## 3. Secciones Opcionales (2 Puntos Bonus)

### Sección 11: Grad-CAM (Interpretabilidad) — ✨ 1 PUNTO
- [ ] Implementar Grad-CAM con hooks
- [ ] Generar attention maps para imágenes
- [ ] Visualizar heatmap superpuesto en imagen original
- [ ] Analizar: ¿Qué mira realmente el modelo?
  - [ ] ¿Detecta manchas en hojas? (BIEN ✓)
  - [ ] ¿O correlaciona fondo/maceta? (SESGO ✗)
- [ ] Conclusión: Confianza en mecanismo de decisión
- [ ] Si hay sesgo: proponer mitiga

### Sección 12: Insight del Problema — ✨ 1 PUNTO
- [ ] Análisis profundo de qué aprendió el model
- [ ] Patrones descubiertos:
  - [ ] ¿Hay tipos de ESCA más/menos detectables?
  - [ ] ¿Hay correlaciones sospechosas (ángulo,iluminación)?
  - [ ] ¿Desbalance de clases? ¿Sesgo?
- [ ] Si es necesario: proponer valor-agregado (ej. "podríamos usar Grad-CAM para descubrir...")
- [ ] Conclusión: Robustez del modelo, limitaciones, mejoras futuras

---

## 4. Conclusión General (Sección Final)

- [ ] Resumen de todas las mejoras aplicadas
- [ ] Métrica final de accuracy/F1/Sensitivity
- [ ] Comparativa base vs. final (mejora total en %)
- [ ] Análisis de riesgos para Juan (FN vs. FP)
- [ ] Recomendación FINAL: ¿Es producible? ¿Confianza?
- [ ] Próximos pasos (deployment, feedback de campo, re-entrenamiento)

---

## 5. Calidad de Código

- [ ] NO TensorFlow/Keras (solo FastAI)
- [ ] `set_seed(42)` al inicio
- [ ] Rutas relativas, sin hardcoding
- [ ] `learn.save()` después de cada hito
- [ ] Comentarios en código son en **primera persona**:
  - [ ] "Voy a aplicar…" (NO "se aplicar…")
  - [ ] "Observo que…" (NO "se observa que…")
  - [ ] "Creo que…" (NO "es recomendado…")

---

## 6. Calidad de Conclusiones

Para CADA conclusión:
- [ ] Contiene número específico (accuracy, loss, tiempo, etc.)
- [ ] Explicación de qué significa ese número
- [ ] Decisión o próximo paso como consecuencia
- [ ] Escrita en primera persona
- [ ] Entre 4-8 líneas
- [ ] Suena como si la escribiera un humano, no una IA

**Red Flag**: Si una conclusión es solo "el modelo está mejorando" → REESCRIBIR

---

## 7. Errores Típicos a Vigilar

- [ ] ❌ Data leakage (train/valid mezclados)
- [ ] ❌ Accuracy perfecta (100%) sin explicación
- [ ] ❌ Conclusiones genéricas ("es importante", "funciona bien")
- [ ] ❌ Afirmaciones sin número de respaldo
- [ ] ❌ Overfitting severo no analizado (train 100%, val 50%)
- [ ] ❌ Sobreajuste extremo en ResNet34 vs. ResNet18 sin comparar
- [ ] ❌ Matriz de confusión sin análisis contextual a Juan
- [ ] ❌ Lenguaje pasivo ("se observó", "fue encontrado")

---

## 8. Validación Técnica

Antes de entregar, ejecutar estas verificaciones:

```python
# GPU OK?
print(f"GPU: {torch.cuda.is_available()}")

# Dataset OK?
path = Path('./esca_dataset/esca')
print(f"Total imágenes: {len(get_image_files(path))}")

# Split OK?
print(f"Train: {len(dls.train)}, Valid: {len(dls.valid)}")

# Modelos guardados?
import os
print(os.listdir('.') if Path('checkpoint_base.pth').exists() else "FALTAN CHECKPOINTS")

# Notebook ejecuta sin errores de principio a fin?
# → Ejecutar 100% una vez como verificación final
```

---

## 9. Resumen de Puntuación

| Requisito | Puntos | Cumple? |
|-----------|--------|---------|
| Modelo Base (502) | 4 | ✅/❌ |
| Learning Rate Óptimo | 1 | ✅/❌ |
| Anti-Overfitting | 1 | ✅/❌ |
| Aumento de Datos | 1 | ✅/❌ |
| Progressive Learning | 1 | ✅/❌ |
| Segunda Arquitectura | 1 | ✅/❌ |
| Matriz de Confusión | 1 | ✅/❌ |
| **TOTAL OBLIGATORIO** | **10** | ✅/❌ |
| Grad-CAM (Opcional) | 1 | ✅/❌ |
| Insight (Opcional) | 1 | ✅/❌ |
| **TOTAL CON BONUS** | **12** | ✅/❌ |

---

## 10. Antes de Enviar

Checklist Final:
- [ ] Notebook ejecuta de principio a fin SIN ERRORES
- [ ] Todas las 10 secciones obligatorias presentes
- [ ] Cada sección tiene Markdown + Código + Conclusión
- [ ] Conclusiones en primera persona, con números
- [ ] Tabla comparativa de arquitecturas presente
- [ ] Matriz de confusión analizada contextualizadamente
- [ ] Comentarios en código son naturales, no de IA
- [ ] Rutas relativas (sin C:\Usuario\... , etc)
- [ ] set_seed(42) aplicado
- [ ] Sin TensorFlow/Keras
- [ ] ✨ Bonus: Grad-CAM OK (opcional)
- [ ] ✨ Bonus: Insight descubierto (opcional)

---

**Estado Final:** Si toda la lista está ✅, el notebook está listo para enviar.

