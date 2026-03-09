# GEMINI — CONTEXTO GENERAL PIA04 (Programación de IA)

Este documento define el contexto completo de la **Tarea PIA04 (UD4 Programación de IA)**.
Debe leerse antes de ayudar a completar conclusiones, revisar resultados o depurar errores.

---

## 1. Descripción general de la tarea

La Tarea PIA04 consta de **dos problemas independientes**, cada uno desarrollado
en un notebook distinto:

### Problema 1 — Aprendizaje supervisado (Tesla)
Notebook: `PIA_04_P1_Tesla.ipynb`

Objetivo:
- Resolver un problema de clasificación supervisada.
- Analizar datos, preprocesar correctamente y entrenar modelos clásicos.
- Comparar modelos y crear ensembles según criterios definidos.

Modelos obligatorios:
- KNN (GridSearchCV)
- Decision Tree (RandomizedSearchCV + explicación antes y después)
- SVM (GridSearchCV)
- NL → MLPClassifier (RandomizedSearchCV)

Ensembles obligatorios:
- Ensemble por fiabilidad (>80%)
- Ensemble con Regresión Lineal como meta-modelo

---

### Problema 2 — Aprendizaje semisupervisado (Fallos)
Notebook: `PIA_04_P2_Fallos.ipynb`

Objetivo:
- Trabajar con un dataset con etiquetas faltantes.
- Aplicar etiquetado automático (LabelPropagation o LabelSpreading).
- Entrenar modelos supervisados y un ensemble final.

Restricción crítica:
- Validación y test SOLO con datos originalmente etiquetados.
- Las pseudo-etiquetas SOLO se usan para entrenamiento.

---

## 2. Reglas críticas (líneas rojas)

Estas reglas **NO pueden romperse**:

- ❌ Data leakage:
  - Nunca ajustar transformadores con todo el dataset.
  - `fit` SOLO con train, `transform` con valid/test.
- ❌ Problema 2:
  - Prohibido validar o testear con pseudo-etiquetas.
- ❌ Librerías:
  - No usar TensorFlow, Keras, PyTorch.
  - Solo scikit-learn y librerías estándar.

---

## 3. Rol de Gemini en esta tarea

Gemini **NO es el programador principal**.

Su rol es:
- Completar y mejorar las secciones:
  **“Conclusiones (a completar tras ejecutar)”**
- Basarse EXCLUSIVAMENTE en:
  - outputs reales del notebook
  - métricas
  - tablas
  - gráficos visibles
- Detectar:
  - incoherencias técnicas
  - posibles errores conceptuales
  - justificaciones débiles o incompletas

Gemini **NO debe**:
- Inventar valores
- Modificar código salvo que se solicite explícitamente
- Cambiar modelos definidos por el enunciado

---

## 4. Forma de trabajo esperada

Flujo correcto:
1. El usuario ejecuta el notebook.
2. El usuario proporciona a Gemini:
   - la sección de conclusiones
   - el output relevante del código
3. Gemini redacta conclusiones humanas, técnicas y justificadas.

Este flujo replica el trabajo de un alumno competente en examen.
