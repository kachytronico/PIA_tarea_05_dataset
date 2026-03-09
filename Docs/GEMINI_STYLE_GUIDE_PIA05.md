# GEMINI — GUÍA DE ESTILO DE REDACCIÓN (PIA05)

Este documento define **cómo debes escribir** conclusiones, comentarios y análisis en la Tarea PIA05.
Úsalo para asegurar que todo suene natural, técnico y humano (no como plantilla de IA).

---

## 1. Filosía General

- **Primera persona**: "observo", "veo", "creo que", "he notado"
- **Tono natural**: Como si lo escribiera un estudiante de ingeniería, no una IA
- **Técnico pero conversacional**: Números y conceptos, pero explicados sencillamente
- **Basado en datos**: Cada afirmación debe poder verificarse en el output

---

## 2. Comentarios en Código

### ✅ Estilo Correcto

```python
# Voy a usar ResNet18 para empezar (menos parámetros, menos overfitting)
learn = vision_learner(dls, resnet18, metrics=accuracy)

# Noto que el LR por defecto diverge en lr_find, voy a ser más conservador
lr_optimal = 1e-3

# Creo que 32 es buen batch size considerando la GPU disponible
dls = db.dataloaders(path, bs=32)

# He visto en la gráfica que aquí empieza a diverger, así que me quedo pre-valle
learn.fine_tune(4, lr_max=lr_optimal)

# Observo que train loss sigue bajando pero val loss se estabiliza
# → Señal de overfitting, voy a aplicar EarlyStoppingCallback
learn.fine_tune(10, cbs=[EarlyStoppingCallback(patience=3)])
```

### ❌ Estilo Incorrecto

```python
# Crear learner
learn = vision_learner(dls, resnet18, metrics=accuracy)

# Usar learning rate óptimo
lr_optimal = 1e-3

# Configurar batch size
dls = db.dataloaders(path, bs=32)

# Fine-tuning iterativo
learn.fine_tune(4, lr_max=lr_optimal)

# Prevenir sobreajuste
learn.fine_tune(10, cbs=[EarlyStoppingCallback(patience=3)])
```

### Reglas de Comentarios

| Aspecto | ✅ Bien | ❌ Mal |
|---------|--------|--------|
| Tono | "Voy a aplicar..." | "Se debe aplicar..." |
| Decisión | "Creo que porque..." | "Porque sí" |
| Sentido | Explica el por qué | Solo describe lo que hace |
| Longitud | 1-2 líneas | Más de 3 líneas (sección de código es para eso) |

---

## 3. Conclusiones Técnicas

Cada sección debe terminar con **una conclusión de 4-8 líneas** que contenga:
1. **Número/métrica concreta**
2. **Observación técnica**
3. **Interpretación de qué significa**
4. **Decisión/próximo paso o validación**

### ✅ Ejemplo Correcto: Modelo Base

> "El modelo base (ResNet18, 3 épocas) alcanza **82.4% accuracy en validación**. Observo que la pérdida de entrenamiento baja de 0.82 a 0.21, mientras que la de validación llega a 0.28, una diferencia de 7 puntos que sugiere overfitting leve pero controlable. La curva se estabiliza después de época 3, confirmando que no necesito más épocas en esta fase. Voy a usar esta métrica (82.4%) como baseline para medir el impacto de cada mejora posterior."

### ✅ Ejemplo Correcto: Learning Rate Óptimo

> "La gráfica de `lr_find()` muestra que la pérdida alcanza su mínimo alrededor de **1e-3**, donde la curva comienza a subir. Elijo LR = 1e-3 porque está justo pre-valle (zona de máxima pendiente descendente sin divergencia). Al re-entrenar con este LR, accuracy sube de 82.4% a **85.1% (+2.7 p.p.)**. El cambio es modesto pero sólido, sugiere que la tasa de aprendizaje anterior era subóptima. Voy a mantener LR = 1e-3 como estándar para el resto de experimentos."

### ✅ Ejemplo Correcto: Matriz de Confusión

> "La matriz muestra: TP=45 (enfermos bien detectados), TN=52 (sanos bien identificados), FP=8 (falsas alarmas), **FN=3 (enfermos pasados por alto)**. La sensibilidad es 94% (3 FN de 48 enfermos totales), excelente para Juan: casi toda enfermedad se detecta. Los 8 FP son tolerables (Juan hará inspecciones manuales). Recomendación: producción segura, riesgo bajo de contagio masivo por falsos negativos."

### ❌ Ejemploma Incorrecto

```
"El modelo funciona bien."
"Los resultados son buenos."
"La accuracy ahora es mejor."
"Se observan mejoras significativas."
"El modelo aprende correctamente."
```

---

## 4. Estructura de una Buena Conclusión

**Patrón General**:

```
[Métrica exacta en números] + [Observación de qué pasó] + [Por qué importa] + [Decisión tomada]
```

**Plantilla:**

> "Observo que **[métrica concreta]**. Esto se debe a **[causa técnica]**. 
> En contexto del proyecto significa **[implicación]**. 
> Por lo tanto, voy a **[decisión/próximo paso]**."

**Ejemplo Completado**:

> "Observo que **el training time para ResNet18 es 120s vs. ResNet34 145s (+20%)**. 
> Esto se debe a **mayor número de capas convolucionales en ResNet34**. 
> En contexto de producción para Juan significa **trade-off: +2% accuracy pero 20% más lento**. 
> Por lo tanto, **recomiendo ResNet18 para producción (suficientemente preciso y eficiente)**."

---

## 5. Elementos que DEBEN estar en una Conclusión

- ✅ Un número o métrica concreta
- ✅ Referencia a qué se compara (antes/después, modelo A/modelo B, etc.)
- ✅ Una reflexión técnica ("observo que", "noto que")
- ✅ Una decisión o siguiente paso consecuente

## Elementos que NO deben estar

- ❌ Frases genéricas ("es importante", "es necesario")
- ❌ Afirmaciones sin número de respaldo
- ❌ Teoría sin conexión al output actual
- ❌ Especulaciones no verificables

---

## 6. Longitud Recomendada

- **Mínimo**: 4 líneas (muy breve pierde contexto)
- **Máximo**: 8 líneas (muy largo se vuelve genérico)
- **Ideal**: 5-6 líneas (equilibrio entre detalle y concisión)

---

## 7. Redacción en Primera Persona

### Verbos Recomendados

| Verbo | Significado | Ejemplo |
|-------|-----------|---------|
| **observo** | Veo en el output | "Observo que accuracy es 85%" |
| **noto** | Detecto una diferencia | "Noto que train loss y val loss divergen" |
| **veo** | Constato visualmente | "Veo en la gráfica que hay dos picos" |
| **creo** | Es mi interpretación | "Creo que es por overfitting" |
| **he notado** | Reflexión de experiencia | "He notado que LR alto diverge" |
| **voy a** | Decisión de acción | "Voy a aplicar callbacks" |
| **elijo** | Decisión técnica deliberada | "Elijo ResNet34 porque..." |

### Evitar

| ❌ Incorrecto | ✅ Correcto |
|-------------|-----------|
| "Se observa que" | "Observo que" |
| "Se puede ver que" | "Veo que" |
| "Es importante mencionar" | "Destaco que" |
| "Cabe señalar que" | "Noto que" |
| "Debe usarse" | "Voy a usar" |

---

## 8. Cómo Estructurar Secciones Completas

Cada sección del notebook debe tener:

### A. Markdown Explicativo (Breve)
```markdown
## 5. Learning Rate Óptimo

La tasa de aprendizaje es el "paso" que da el optimizador.
Si es muy alta → divergencia (loss = NaN)
Si es muy baja → convergencia lenta
La función lr_find() ayuda a encontrar el valor óptimo.
```

### B. Código con Comentarios
```python
# Ejecuto lr_find para visualizar la pendiente
learn.lr_find()

# En la gráfica veo que antes de 1e-3 hay descenso fuerte
# A partir de 1e-3 empieza a divergir
# Elijo LR = 1e-3 (justo antes del valle)
learn.fine_tune(4, lr_max=1e-3)
```

### C. Conclusión Técnica
```
"La búsqueda de LR mostró mínimo en 1e-3. Al re-entrenar, accuracy mejora de 82% a 85%.
El impacto es positivo (+3p.p.), validando que el LR anterior era subóptimo.
Voy a mantener 1e-3 como estándar."
```

---

## 9. Ejemplo Completo: Sección Augmentación

```markdown
### Conclusión de Aumento de Datos

Implementé augmentación batch-level (`batch_tfms=aug_transforms()`).
Esto aplica transformaciones aleatorias (rotación ±10°, brillo, perspectiva)
en RAM durante cada época, sin almacenar imágenes sintéticas en disco.

Resultados:
- Sin augmentación: 85% accuracy
- Con augmentación: 87% accuracy (+2 p.p.)
- Train/val loss más cercanos (overfitting reducido)

Observo que la augmentación no solo mejora métrica sino que también hace 
el modelo más robusto ante variaciones del mundo real (diferentes ángulos, iluminación).
Para un agrícultor tomando fotos con teléfono, esto es crítico.
Voy a mantener augmentación en todos los experimentos posteriores.
```

---

## 10. Checklist de Redacción

Antes de finalizar cada conclusión:

- [ ] Contiene un número/métrica específica
- [ ] Está escrita en primera persona ("observo", "veo", "creo")
- [ ] Explica POR QUÉ ese número importa (no solo lo reporta)
- [ ] Tiene una decisión o próximo paso consecuente
- [ ] Entre 4-8 líneas (conciso pero completo)
- [ ] Suena como si la escribiera un estudiante, no una IA genérica
- [ ] Cada afirmación se puede verificar en el output actual

---

**Golden Rule**: Si lees tu conclusión y no suena como si la hubiera escrito TÚ (con TU análisis, TU decisión), reescribela en primera persona.

