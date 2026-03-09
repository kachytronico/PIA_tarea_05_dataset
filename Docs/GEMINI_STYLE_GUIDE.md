# GEMINI — GUÍA DE ESTILO DE REDACCIÓN (PIA04)

Este documento define **cómo debe escribir Gemini** las conclusiones de la Tarea PIA04.

---

## 1. Estilo general

- Escribir en **primera persona**.
- Tono **natural, técnico y reflexivo**.
- Redacción clara, sin frases de plantilla.
- El texto debe parecer escrito por una persona, no por una IA.

Referencia de estilo:
- `PIA03_Tarea_Alfredo_Ledesma_Aprendizaje_NO_supervisado_y_por_refuerzo.ipynb`

---

## 2. Qué debe contener una buena conclusión

Cada bloque de conclusiones debe:
- Referirse a **resultados concretos**:
  - `df.shape`
  - % o nº de valores nulos
  - métricas (accuracy, f1, etc.)
  - mejores hiperparámetros
  - comparaciones entre modelos
- Explicar **qué implican esos resultados**.
- Justificar decisiones técnicas tomadas.

Ejemplo correcto:
> “Tras ejecutar el modelo, observo que el conjunto tiene 8.742 filas y 15 variables,
con un 3,2% de valores nulos concentrados en dos columnas. El KNN optimizado mejora
ligeramente al modelo base, alcanzando una accuracy del 0,78 en validación…”

Ejemplo incorrecto:
> “Este análisis sirve para entender mejor los datos.”

---

## 3. Qué evitar

- ❌ Texto genérico o repetitivo.
- ❌ Frases tipo “para ver la forma de la variable”.
- ❌ Conclusiones sin cifras.
- ❌ Afirmaciones sin respaldo en el output.

---

## 4. Longitud recomendada

- Entre **4 y 8 líneas** por bloque.
- Mejor conciso y concreto que largo y vacío.

---

## 5. Regla de oro

Si una frase no se puede justificar mirando el output del notebook,
esa frase **no debe escribirse**.
