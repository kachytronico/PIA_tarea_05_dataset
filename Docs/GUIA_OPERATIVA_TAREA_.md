# GUÍA OPERATIVA DE LA TAREA — PIA05 (ESCA)

## Objetivo
Resolver el Problema 1 de la unidad usando FastAI y documentar cada criterio evaluable de la rúbrica.

## Secuencia de ejecución recomendada
1. **Contexto y setup**: objetivo, imports, semilla y dispositivo.
2. **Carga de datos**: validar estructura de clases y conteos.
3. **Modelo base**: `DataBlock` + `vision_learner(resnet18)`.
4. **Learning rate**: `lr_find` y selección justificada.
5. **Aumento de datos**: `aug_transforms`.
6. **Evitar sobreajuste**: early stopping / `wd` / control de epochs.
7. **Progressive learning**: entrenar en 128 y continuar en 224.
8. **Otra arquitectura**: resnet34 o resnet50 y comparativa.
9. **Matriz de confusión**: análisis de FP/FN.
10. **Opcional**: Grad-CAM e insight del problema.

## Evidencia mínima por bloque
- Código ejecutable.
- Resultado (métrica o figura).
- Conclusión técnica breve.

## Criterio de calidad
- Coherencia técnica.
- Reproducibilidad.
- Lenguaje claro.
- Enfoque de negocio agrícola (coste del error).
