# PLAN DE TRABAJO — PIA05 (Problema 1: ESCA)

> Archivo heredado de nombre `PIA_04_*` y adaptado a Unidad 05.

## FASE A: PREPARACIÓN
- [ ] **Hito 1: Entorno y dataset**
  - [ ] Verificar librerías FastAI/PyTorch.
  - [ ] Definir ruta local del dataset ESCA.
  - [ ] Comprobar estructura de carpetas por clase.

## FASE B: MODELO BASE
- [ ] **Hito 2: DataBlock inicial (502)**
  - [ ] `ImageBlock + CategoryBlock`.
  - [ ] `RandomSplitter(valid_pct=0.2, seed=42)`.
  - [ ] `Resize(224)` y `dataloaders`.
- [ ] **Hito 3: Entrenamiento base**
  - [ ] `vision_learner(..., resnet18, metrics=accuracy)`.
  - [ ] `fine_tune` inicial.
  - [ ] Registrar accuracy/loss base.

## FASE C: OPTIMIZACIÓN OBLIGATORIA
- [ ] **Hito 4: Learning Rate óptimo**
  - [ ] Ejecutar `lr_find`.
  - [ ] Documentar LR elegido y motivo.
- [ ] **Hito 5: Aumento de datos + anti-overfitting**
  - [ ] Activar `aug_transforms()`.
  - [ ] Añadir callback de early stopping.
  - [ ] Ajustar `wd` si procede.
- [ ] **Hito 6: Progressive learning**
  - [ ] Ciclo 1 con resolución baja (ej. 128).
  - [ ] Ciclo 2 con resolución alta (ej. 224).
  - [ ] Comparar métricas entre ciclos.
- [ ] **Hito 7: Segunda arquitectura**
  - [ ] Entrenar resnet34 o resnet50.
  - [ ] Comparar contra baseline.

## FASE D: EVALUACIÓN Y CIERRE
- [ ] **Hito 8: Matriz de confusión**
  - [ ] Generar con `ClassificationInterpretation`.
  - [ ] Analizar FP/FN con impacto de negocio.
- [ ] **Hito 9 (Opcional): Grad-CAM**
  - [ ] Crear mapa de calor en ejemplos relevantes.
  - [ ] Verificar si el modelo mira la hoja y no el fondo.
- [ ] **Hito 10: Revisión final**
  - [ ] Checklist de rúbrica completo.
  - [ ] Conclusión técnica final y limitaciones.
