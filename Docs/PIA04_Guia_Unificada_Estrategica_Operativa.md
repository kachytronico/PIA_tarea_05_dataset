# PIA05 — Guía Unificada (Estratégica + Operativa)

> Archivo heredado de nombre `PIA04_*` y adaptado a Unidad 05.

## Capa estratégica (cómo pensar la tarea)
- Empezar por un modelo vanilla para tener línea base.
- Optimizar de forma incremental (una mejora por bloque).
- Medir impacto real en validación tras cada cambio.
- Priorizar reducción de falsos negativos sobre exactitud cosmética.
- Confirmar interpretabilidad cuando los resultados sean muy altos.

## Capa operativa (cómo ejecutarla)
1. Setup + carga de datos.
2. Modelo base con `resnet18`.
3. LR óptimo.
4. Augmentación y anti-overfitting.
5. Progressive learning.
6. Nueva arquitectura y comparación.
7. Matriz de confusión + análisis de errores.
8. Grad-CAM opcional.

## Mapa rúbrica -> evidencia
- Modelo base: celda de entrenamiento + accuracy inicial.
- LR óptimo: figura de `lr_find` + decisión argumentada.
- Overfitting: uso de callbacks/`wd` + evolución de losses.
- Progressive learning: tabla comparativa de resoluciones.
- Otra arquitectura: tabla baseline vs alternativa.
- Matriz de confusión: figura + lectura de FP/FN.
- Augmentación: definición `aug_transforms` + explicación.

## Errores frecuentes y prevención
- Accuracy 100% demasiado pronto: revisar fuga de datos o sesgo.
- CUDA OOM: bajar `bs`, reducir resolución, reiniciar runtime.
- Mejoras sin control: no aplicar todo a la vez.
- Conclusiones vagas: usar métricas y ejemplos concretos.

## Cierre recomendado
- Resumen de decisiones técnicas.
- Resultado final y limitaciones.
- Próximos pasos (más datos, mejor balance, validación externa).
