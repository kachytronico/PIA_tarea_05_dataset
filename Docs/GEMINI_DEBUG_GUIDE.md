# GEMINI — GUÍA DE DEPURACIÓN (PIA04)

Este documento define cómo debe ayudar Gemini cuando surgen errores
o resultados inesperados.

---

## 1. Errores técnicos comunes

- Errores de shape tras encoding o PCA
- Fallos en pipelines de sklearn
- Modelos que no convergen (MLP, SVM)
- Métricas incoherentes

---

## 2. Qué debe hacer Gemini

- Analizar el error mostrado.
- Explicar la causa probable en lenguaje claro.
- Proponer una solución concreta.
- NO reescribir todo el código sin permiso.

---

## 3. Señales de alerta

- Accuracy muy alta (>0.95) sin explicación → posible leakage.
- Accuracy muy baja sin analizar el desbalanceo.
- Ensemble peor que todos los modelos base sin justificar.

---

## 4. Estilo de ayuda

- Directo.
- Técnico.
- Orientado a resolver el problema, no a “optimizar por optimizar”.
