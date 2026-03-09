# PIA05 — CHECKLIST DE IMPLEMENTACIÓN

> Seguimiento de tareas para completa resolución del Problema 1: ESCA

---

## 📋 CHECKLIST GENERAL DEL PROYECTO

### 1. CONFIGURACIÓN INICIAL

- [ ] **1.1** Clonar/descargar dataset de Mendeley: https://data.mendeley.com/datasets/89cnxc58kj/1
- [ ] **1.2** Verificar estructura: `esca_dataset/esca/` y `esca_dataset/healthy/`
- [ ] **1.3** Contar imágenes por clase (validar dataset completo)
- [ ] **1.4** Confirmar FastAI instalado: `from fastai.vision.all import *`
- [ ] **1.5** Confirmar GPU detectada: `torch.cuda.is_available() = True`

**Estado**: ⬜ No iniciado | 🔵 En progreso | ✅ Completado

---

## 🎓 SECCIONES OBLIGATORIAS DEL NOTEBOOK (7 puntos)

### SECCIÓN 1: Contexto del Problema (Markdown)
- [ ] **1.1** Explicación clara del problema ESCA
- [ ] **1.2** Motivación: Juan y el viñedo
- [ ] **1.3** Objetivo: Clasificador binario (Sana/Infectada)
- [ ] **1.4** Dataset oficial referenciado

**Puntos asignados**: 0 (solo introducción)

---

### SECCIÓN 2: Setup de Entorno
- [ ] **2.1** Imports correctos: `from fastai.vision.all import *`
- [ ] **2.2** Semilla fija: `set_seed(42)`
- [ ] **2.3** GPU detectada
- [ ] **2.4** Rutas relativas (sin hardcoding)

**Puntos asignados**: 0 (solo setup)

---

### SECCIÓN 3: Modelo Básico de Clasificación (Cuadernillo 502) ⭐ **4 PUNTOS**

**Requisitos**:
- [ ] **3.1** `DataBlock` con parámetros correctos:
  - [ ] `blocks=(ImageBlock, CategoryBlock)`
  - [ ] `get_items=get_image_files`
  - [ ] `splitter=RandomSplitter(valid_pct=0.2, seed=42)`
  - [ ] `get_y=parent_label`
  - [ ] `item_tfms=Resize(224)`
- [ ] **3.2** Crear `dataloaders` con `bs=32`
- [ ] **3.3** `vision_learner` con `resnet18`
- [ ] **3.4** `learn.fine_tune(3)` ejecutado
- [ ] **3.5** Accuracy y loss registrados
- [ ] **3.6** Modelo guardado: `learn.save('model_base')`
- [ ] **3.7** Markdown: Explicación del algoritmo base
- [ ] **3.8** Conclusión: Métricas base (accuracy ≈ 80-85%)

**Evidencia esperada**:
```
Accuracy base: 0.XXX
Loss: 0.XXX
```

**Puntos**: ⬜ : 0 | 🟡 Parcial (2) | ✅ Completo (4)

---

### SECCIÓN 4: Learning Rate Óptimo ⭐ **1 PUNTO**

**Requisitos**:
- [ ] **4.1** Ejecutar `learn.lr_find()`
- [ ] **4.2** Gráfica generada y mostrada
- [ ] **4.3** LR elegido justificado (ej. "LR = 1e-3, justo antes del valle")
- [ ] **4.4** Nuevo entrenamiento: `learn.fine_tune(4, lr_max=1e-3)`
- [ ] **4.5** Comparativa: Accuracy anterior vs. nuevo
- [ ] **4.6** Modelo guardado: `learn.save('model_lr_optimized')`
- [ ] **4.7** Markdown: Explicación de lr_find y selección
- [ ] **4.8** Conclusión: Mejora en accuracy (esperado +2-3 p.p.)

**Evidencia esperada**:
```
Accuracy anterior: 0.82
Accuracy con LR óptimo: 0.85
Mejora: +3 p.p.
```

**Puntos**: ⬜ : 0 | 🟡 Parcial (0.5) | ✅ Completo (1)

---

### SECCIÓN 5: Técnicas Anti-Overfitting ⭐ **1 PUNTO**

**Requisitos** (implementar al menos UNA):
- [ ] **5.1** `EarlyStoppingCallback(patience=3)` O
- [ ] **5.2** Weight decay: `wd=0.1` O
- [ ] **5.3** COMBO (ambas) — RECOMENDADO

- [ ] **5.4** Código:
  ```python
  learn.fine_tune(10, lr_max=1e-3, wd=0.1, 
                  cbs=[EarlyStoppingCallback(patience=3)])
  ```
- [ ] **5.5** Graficar curvas: `learn.recorder.plot_loss()`
- [ ] **5.6** Analizar convergencia (train vs. val)
- [ ] **5.7** Modelo guardado: `learn.save('model_antioverfit')`
- [ ] **5.8** Markdown: Explicación de overfitting y técnicas
- [ ] **5.9** Conclusión: Diferencia train/val reducida, estabilidad mejorada

**Evidencia esperada**:
```
Train Loss: 0.25
Val Loss: 0.30
(diferencia pequeña = generalización buena)
```

**Puntos**: ⬜ : 0 | 🟡 Parcial (0.5) | ✅ Completo (1)

---

### SECCIÓN 6: Aumento de Datos ⭐ **1 PUNTO**

**Requisitos**:
- [ ] **6.1** Crear nuevo `DataBlock` CON `batch_tfms=aug_transforms()`
- [ ] **6.2** Crear nuevos `dataloaders`
- [ ] **6.3** Código:
  ```python
  db_aug = DataBlock(..., batch_tfms=aug_transforms())
  dls_aug = db_aug.dataloaders(path, bs=32)
  learn.dls = dls_aug
  learn.fine_tune(4)
  ```
- [ ] **6.4** Visualizar ejemplos aumentados (opcional: `dls_aug.show_batch()`)
- [ ] **6.5** Modelo guardado: `learn.save('model_augmented')`
- [ ] **6.6** Markdown: Explicación de augmentación (rotación, brillo, etc.)
- [ ] **6.7** Conclusión: Impacto en accuracy (esperado +1-2 p.p.), robustez mejorada

**Evidencia esperada**:
```
Accuracy sin augmentación: 0.85
Accuracy con augmentación: 0.86
Impacto: Generalización mejorada
```

**Puntos**: ⬜ : 0 | 🟡 Parcial (0.5) | ✅ Completo (1)

---

### SECCIÓN 7: Progressive Learning ⭐ **1 PUNTO**

**Requisitos**:
- [ ] **7.1** Fase 1: DataBlock con `Resize(128)`
- [ ] **7.2** Fase 1: Entrenar 2-3 épocas
- [ ] **7.3** Fase 1: Guardar: `learn.save('progressive_phase1')`
- [ ] **7.4** Fase 2: DataBlock con `Resize(224)`
- [ ] **7.5** Fase 2: Reemplazar `dls` y continuar: `learn.fine_tune(4)`
- [ ] **7.6** Fase 2: Guardar: `learn.save('progressive_phase2')`
- [ ] **7.7** Comparativa: Velocidad de convergencia (fase 1 vs. fase 2)
- [ ] **7.8** Markdown: Explicación de por qué progressive learning es efectivo
- [ ] **7.9** Conclusión: Convergencia más rápida, mejora en detalles finos

**Evidencia esperada**:
```
Fase 1 (Resize 128): Converge en 3 épocas
Fase 2 (Resize 224): Refina detalles en 4 épocas
Beneficio: Aprendizaje jerárquico (formas → detalles)
```

**Puntos**: ⬜ : 0 | 🟡 Parcial (0.5) | ✅ Completo (1)

---

### SECCIÓN 8: Segunda Arquitectura ⭐ **1 PUNTO**

**Requisitos**:
- [ ] **8.1** Probar `ResNet34` o `ResNet50` (no ResNet18)
- [ ] **8.2** Crear nuevo `vision_learner`: `vision_learner(dls, resnet34)`
- [ ] **8.3** Ejecutar entrenamiento con mismos parámetros: `lr_max=1e-3`, augmentación, callback
- [ ] **8.4** Registrar accuracy, tiempo, # parámetros
- [ ] **8.5** Guardar: `learn.save('model_resnet34')`
- [ ] **8.6** Crear tabla comparativa:
  ```
  | Modelo    | Accuracy | Tiempo (s) | Parámetros |
  |-----------|----------|-----------|------------|
  | ResNet18  | 0.85     | 120       | 11M        |
  | ResNet34  | 0.87     | 145       | 21M        |
  ```
- [ ] **8.7** Markdown: Explicación de diferencias arquitectónicas
- [ ] **8.8** Conclusión: Trade-off complejidad-precisión, cuál elegir para producción

**Puntos**: ⬜ : 0 | 🟡 Parcial (0.5) | ✅ Completo (1)

---

### SECCIÓN 9: Matriz de Confusión + Análisis ⭐ **1 PUNTO**

**Requisitos**:
- [ ] **9.1** `interp = ClassificationInterpretation.from_learner(learn)`
- [ ] **9.2** `interp.plot_confusion_matrix(figsize=(6,6))`
- [ ] **9.3** Extraer TP/TN/FP/FN (numéricos)
- [ ] **9.4** Markdown: Definición de cada métrica
- [ ] **9.5** Análisis contextualizado:
  ```markdown
  TP (Verdaderos Positivos): X plantas enfermas detectadas → CORRECTO
  TN (Verdaderos Negativos): Y plantas sanas detectadas → CORRECTO
  FP (Falsos Positivos): Z plantas sanas como enfermas → Juan hace inspección extra (tolerable)
  FN (Falsos Negativos): W plantas enfermas como sanas → RIESGO: Contagio masivo (crítico)
  ```
- [ ] **9.6** Especial análisis de falsos negativos (riesgo agrícola)
- [ ] **9.7** Conclusión: Evaluación de riesgo para Juan, recomendación de confianza

**Evidencia esperada**:
```
TP = 45, TN = 52, FP = 8, FN = 3
Sensibilidad (Recall): 94% (bajo FN)
Especificidad: 87%
Conclusión: Confianza ALTA en efectividad del modelo
```

**Puntos**: ⬜ : 0 | 🟡 Parcial (0.5) | ✅ Completo (1)

---

## 🌟 SECCIONES OPCIONALES (2 puntos adicionales)

### SECCIÓN 10 (OPCIONAL): Interpretabilidad - Grad-CAM ⭐ **1 PUNTO**

**Requisitos**:
- [ ] **10.1** Implementar Grad-CAM con hooks (Forward + Backward)
- [ ] **10.2** Generar attention maps para imágenes de validación
- [ ] **10.3** Visualizar superposición de heatmap + imagen original
- [ ] **10.4** Analizar visualmente: ¿Qué mira el modelo?
- [ ] **10.5** Validar conclusiones:
  - [ ] ¿Detecta manchas reales de ESCA en hojas?
  - [ ] ¿O correlaciona fondo/maceta/tierra?
- [ ] **10.6** Markdown: Explicación técnica de Grad-CAM
- [ ] **10.7** Conclusión: Confianza en mecanismo de decisión

**Evidencia esperada**:
```
Grad-CAM muestra que el modelo activa neuronas en las regiones 
de las hojas donde hay manchas características de ESCA.
NO hay activación en fondo/cielo/maceta.
Conclusión: Modelo aprendió características reales (medicina vegetal),
no sesgos espurios.
```

**Puntos**: ⬜ : 0 | 🟡 Parcial (0.5) | ✅ Completo (1)

---

### SECCIÓN 11 (OPCIONAL): Insight del Problema ⭐ **1 PUNTO**

**Requisitos**:
- [ ] **11.1** Investigar: ¿Qué patrones aprendió realmente el modelo?
- [ ] **11.2** Análisis:
  - [ ] ¿Hay correlaciones sospechosas? (ej. iluminación, ángulo de cámara)
  - [ ] ¿Todos los tipos de ESCA se detectan por igual?
  - [ ] ¿Hay sesgo a favor de cierta clase?
- [ ] **11.3** Usar resultados de Grad-CAM si disponible
- [ ] **11.4** Proponer mitigaciones si hay sesgo
- [ ] **11.5** Markdown: Hallazgos técnicos
- [ ] **11.6** Conclusión: Evaluación final de robustez y producibilidad

**Evidencia esperada**:
```
Hallazgo: El modelo detecta bien necrosis angular (manchas negras)
pero menos preciso en manchas pardas difusas.
Recomendación: Considerar dataset augmentation específicamente
para manchas pardas, o ajustar umbral de confianza.
```

**Puntos**: ⬜ : 0 | 🟡 Parcial (0.5) | ✅ Completo (1)

---

## 📊 SECCIÓN FINAL: Conclusión General

- [ ] **C.1** Resumen de todas las mejoras aplicadas
- [ ] **C.2** Métrica final de accuracy/F1/Recall
- [ ] **C.3** Comparativa: Modelo base vs. Modelo final
- [ ] **C.4** Análisis de riesgos para Juan (impacto de errores)
- [ ] **C.5** Recomendación final: ¿Es producible?
- [ ] **C.6** Posibles mejoras futuras

---

## 🔍 VALIDACIÓN FINAL (ANTES DE ENTREGAR)

### Checklist de Calidad del Código
- [ ] ✅ `set_seed(42)` en setup
- [ ] ✅ Rutas relativas (sin hardcoding)
- [ ] ✅ Sin imports de TensorFlow/Keras
- [ ] ✅ `learn.save()` ejecutado después de cada hito
- [ ] ✅ Todos los modelos guardados
- [ ] ✅ Notebook ejecuta sin errores de principio a fin
- [ ] ✅ Gráficas generadas (lr_find, loss curves, confmatrix)

### Checklist de Documentación
- [ ] ✅ Cada sección tiene Markdown explicativo
- [ ] ✅ Conclusiones con métricas concretas
- [ ] ✅ Títulos alineados con la rúbrica
- [ ] ✅ Lenguaje técnico, claro, sin genéricos
- [ ] ✅ Análisis contextualizado a Juan (agricultor)

### Checklist de Completud
- [ ] ✅ 7 secciones obligatorias implementadas
- [ ] ✅ Matriz de confusión analizada
- [ ] ✅ Comparativa de arquitecturas presente
- [ ] ✅ Progressive learning explicado
- [ ] ✅ (Opcional) Grad-CAM si el tiempo lo permite
- [ ] ✅ (Opcional) Insight descubierto si aplica

---

## 📈 PUNTUACIÓN ESPERADA

### Desglose de Puntos

| Sección | Puntos | Estado |
|---------|--------|--------|
| 1. Modelo Base (502) | 4 | ⬜/🟡/✅ |
| 2. Learning Rate Óptimo | 1 | ⬜/🟡/✅ |
| 3. Anti-Overfitting | 1 | ⬜/🟡/✅ |
| 4. Progressive Learning | 1 | ⬜/🟡/✅ |
| 5. Segunda Arquitectura | 1 | ⬜/🟡/✅ |
| 6. Matriz de Confusión | 1 | ⬜/🟡/✅ |
| 7. Aumento de Datos | 1 | ⬜/🟡/✅ |
| **TOTAL OBLIGATORIO** | **10** | - |
| (Opt) Interpretabilidad | 1 | ⬜/🟡/✅ |
| (Opt) Insight | 1 | ⬜/🟡/✅ |
| **TOTAL CON OPCIONAL** | **12** | - |

---

## 🎯 PRÓXIMOS PASOS

1. ✅ Documentación creada (está viendo esto)
2. ⏳ Completar notebook sección por sección
3. ⏳ Ejecutar y validar cada sección
4. ⏳ Revisar checklist regularmente
5. ⏳ Entregar notebook funcional y documentado

---

**Última actualización**: marzo 9, 2026
**Estado global**: 🔵 En Progreso

