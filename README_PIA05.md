# PIA05 — Índice de Documentos y Recursos

> Unidad 05: Visión por Computador | Problema 1: Clasificación de la Enfermedad ESCA

---

## 📚 Documentos de Referencia

### 1. **PIA_05_CONTEXTO_IA.md** ← 🎯 **COMIENZA AQUÍ**
- **Qué es**: Marco conceptual completo del proyecto
- **Contiene**:
  - Rol del especialista en IA
  - Definición del problema (Juan, viña, ESCA)
  - Dataset oficial y estructura
  - Restricciones técnicas (líneas rojas)
  - Requisitos evaluables (7 obligatorios + 2 opcionales)
  - Riesgos técnicos frecuentes
  - Patrón típico de FastAI v2
- **Cuándo leerlo**: **PRIMERO** — Para entender qué debes hacer y por qué
- **Tiempo de lectura**: 15 minutos

---

### 2. **PIA_05_GUIA_ESTILO.md** ← 📏 **Estándares de Código**
- **Qué es**: Normativa técnica para escribir el notebook
- **Contiene**:
  - Estructura obligatoria de secciones (12 capítulos)
  - Reglas de código (imports, variables, funciones)
  - Reglas de entrenamiento (LR, augmentación, callbacks)
  - Reglas de análisis (métricas concretas)
  - Reglas de redacción (conclusiones técnicas)
  - Líneas rojas no negociables
  - Checklist de estilo antes de entregar
  - Ejemplo completo de una sección bien hecha
- **Cuándo leerlo**: **SEGUNDO** — Para saber cómo escribir el código
- **Tiempo de lectura**: 20 minutos

---

### 3. **PIA_05_GUIA_OPERATIVA.md** ← 🛠️ **Cómo Hacer la Tarea**
- **Qué es**: Roadmap operativo paso a paso
- **Contiene**:
  - Entrega esperada (formato, estructura)
  - Plantilla recomendada por secciones (con código)
  - Código mínimo canónico (copy-paste ready)
  - Mejoras obligatorias con checklist
  - Líneas rojas (prohibiciones)
  - Extra recomendado (opcionales)
  - Errores frecuentes y prevención
  - Cronograma realista (3 horas total)
  - Criterios de aceptación
  - Instrucciones para pedir ayuda
- **Cuándo leerlo**: **TERCERO** — Para ejecutar la tarea
- **Tiempo de lectura**: 25 minutos

---

### 4. **PIA_05_CHECKLIST.md** ← ✅ **Seguimiento**
- **Qué es**: Lista de verificación detallada de cada requisito
- **Contiene**:
  - Checklist de configuración inicial
  - Desglose de cada sección evaluable (7 + 2)
  - Requisitos específicos con casillas
  - Evidencia esperada de cada sección
  - Validación final (calidad, documentación, completud)
  - Puntuación desglosada
  - Próximos pasos
- **Cuándo leerlo**: **DURANTE LA TAREA** — Para marcar progreso
- **Uso**: Actualiza casillas conforme avanzas
- **Tiempo de lectura**: 10 minutos inicialmente, consultarás frecuentemente

---

### 5. **.github/agents/PIA_Tarea_05.agent.md** ← 🤖 **Instrucciones del Agente**
- **Qué es**: Prompt para el asistente de IA (tú lo consultas)
- **Contiene**:
  - Rol del agente (especialista en FastAI)
  - Objetivo operativo
  - Restricciones, obligaciones
  - Flujo mínimo a cubrir
  - Código mínimo canónico
  - Tabla de mejoras obligatorias
  - Criterios de calidad
  - Errores comunes y soluciones
  - Modo de respuesta esperada
- **Cuándo leerlo**: **Opcional** — Es para que uses cuando hables conmigo
- **Uso**: Referencia si necesitas pedir secciones específicas del notebook

---

## 🎯 Flujo Recomendado (Plan de Ataque)

### Día 1 (Mañana) — Teoría y Fondos [1 hora]
1. Leer **PIA_05_CONTEXTO_IA.md** completo
2. Leer **PIA_05_GUIA_ESTILO.md** completo
3. Entender qué significa cada requisito

### Día 1 (Tarde) — Primer Setup [1 hora]
1. Leer **PIA_05_GUIA_OPERATIVA.md** (secciones 1-3)
2. Crear notebook base con setup (secciones 1-3 del notebook)
3. Validar dataset cargado correctamente
4. Marcar ✅ en CHECKLIST.md

### Día 2 (Mañana) — Modelo Base + LR [1.5 horas]
1. Implementar sección 4: Modelo Base (Cuadernillo 502)
2. Implementar sección 5: Learning Rate Óptimo
3. Guardar modelo
4. Marcar ✅ en CHECKLIST.md

### Día 2 (Tarde) — Mejoras Obligatorias [1.5 horas]
1. Implementar sección 6: Aumento de Datos
2. Implementar sección 7: Anti-Overfitting
3. Implementar sección 8: Progressive Learning
4. Marcar ✅ en CHECKLIST.md

### Día 3 (Mañana) — Arquitecturas y Confusion Matrix [1 hora]
1. Implementar sección 9: Segunda Arquitectura
2. Implementar sección 10: Matriz de Confusión (análisis detallado)
3. Marcar ✅ en CHECKLIST.md

### Día 3 (Tarde) — Opcionales y Conclusión [1 hora]
1. (Opcional) Sección 11: Grad-CAM
2. (Opcional) Sección 12: Insight
3. Sección final: Conclusión
4. Marcar ✅ CHECKLIST.md completo

**Total**: ~6 horas de trabajo efectivo (distribuido en 3 días)

---

## 🔄 Ciclo de Trabajo Recomendado

Para cada sección:

1. **Consulta la guía** (PIA_05_GUIA_OPERATIVA.md)
2. **Lee el checklist** (PIA_05_CHECKLIST.md) de esa sección
3. **Pide ayuda específica** al asistente:
   > "Genera la sección **[NOMBRE]** del notebook de PIA05..."
4. **Ejecuta el código** en notebook
5. **Valida resultados** (métricas, gráficas)
6. **Actualiza CHECKLIST.md** (marca ✅)
7. **Continúa con siguiente sección**

---

## 💡 Tips Operativos

### Ahorra Tiempo
- ✅ Guarda modelos con `learn.save()` tras cada hito
- ✅ Ejecuta células una a una (no todas de golpe)
- ✅ Reinicia kernel si hay error CUDA

### Evita Errores
- ❌ No hardcodees rutas
- ❌ No uses TensorFlow/Keras
- ❌ No confundas train con test
- ❌ No olvides explicaciones Markdown

### Maximiza Puntuación
- ✅ Implementa ambas técnicas anti-overfitting
- ✅ Haz Grad-CAM (es fácil con FastAI)
- ✅ Descubre un insight real (explora Grad-CAM)
- ✅ Análisis de confusión contextualizado a Juan

---

## 🆘 Cuándo Pedir Ayuda

### Pregunta correcta:
> "Genera la sección **Modelo Base (Cuadernillo 502)** del notebook de PIA05 usando FastAI v2. 
> Incluye:
> 1) Markdown explicativo
> 2) Código ejecutable con comentarios
> 3) Conclusión técnica con métricas
> Contexto: Clasificación ESCA en vides, dataset Mendeley oficial."

### Pregunta incorrecta:
> "¿Cómo hago el notebook?" (muy vaga)
> "Dame todo el código" (no aprendes)
> "Ayúdame rápido" (sin contexto)

---

## 📊 Estructura del Notebook Esperado

El notebook final debe tener estas **12 secciones** en este orden:

1. ✅ Contexto del Problema (Markdown)
2. ✅ Setup e Imports (Código)
3. ✅ Carga y Exploración Dataset (Código + Viz)
4. ✅ **Modelo Base (502)** ← 4 puntos
5. ✅ **Learning Rate Óptimo** ← 1 punto
6. ✅ **Aumento de Datos** ← 1 punto
7. ✅ **Anti-Overfitting** ← 1 punto
8. ✅ **Progressive Learning** ← 1 punto
9. ✅ **Segunda Arquitectura** ← 1 punto
10. ✅ **Matriz de Confusión** ← 1 punto
11. ✨ (Opt) Interpretabilidad Grad-CAM ← 1 punto
12. ✅ Conclusión Final (Markdown)

**Total**: 10 puntos obligatorios + 2 opcionales = 12 puntos posibles

---

## 📁 Estructura de Carpetas

```
PIA_tarea_05_dataset/
├── Docs/
│   ├── PIA_05_CONTEXTO_IA.md ← COMENZAR AQUÍ
│   ├── PIA_05_GUIA_ESTILO.md
│   ├── PIA_05_GUIA_OPERATIVA.md
│   ├── PIA_05_CHECKLIST.md ← CONSULTAR SIEMPRE
│   └── (antiguos PIA_04_*.md) — referencias
├── .github/agents/
│   └── PIA_Tarea_05.agent.md ← Para pedir ayuda
├── PIA_05_Tarea_01.ipynb ← TU NOTEBOOK PRINCIPAL
├── esca_dataset/
│   ├── esca/ (plantas infectadas)
│   └── healthy/ (plantas sanas)
└── Notebooks/ (si hay otros adicionales)
```

---

## ✅ Validación Antes de Entregar

### Checklist Final

- [ ] ✅ Notebook ejecuta de inicio a fin sin errores
- [ ] ✅ Todas las 7 secciones obligatorias presentes
- [ ] ✅ Cada sección tiene Markdown + código + conclusión
- [ ] ✅ Métricas concretas (accuracy, F1, matriz)
- [ ] ✅ Tabla comparativa de arquitecturas
- [ ] ✅ Análisis de confusión contextualizado a Juan
- [ ] ✅ Rutas relativas (sin hardcoding)
- [ ] ✅ `set_seed(42)` en setup
- [ ] ✅ Sin TensorFlow/Keras
- [ ] ✅ Modelos guardados en checkpoints

---

## 🎓 Próximos Pasos

**ACCIÓN INMEDIATA**:
1. Lee **PIA_05_CONTEXTO_IA.md** (15 min)
2. Lee **PIA_05_GUIA_ESTILO.md** (20 min)
3. Busca ayuda: "Genera la sección **1. Contexto** del notebook..."

**ENTONCES**:
- Comienza con sección 1 del notebook (Contexto)
- Continúa con sección 2 (Setup)
- Avanza sección por sección usando CHECKLIST.md

---

## 📞 Contacto / Soporte

Si tienes dudas:
1. **Consulta CHECKLIST.md** (¿qué falta?)
2. **Consulta GUIA_OPERATIVA.md** (¿cómo hacerlo?)
3. **Consulta GUIA_ESTILO.md** (¿formalmente correcto?)
4. **Pide ayuda específica** al asistente con formato correcto

---

**Estado**: 🔵 Proyecto Iniciado
**Última actualización**: marzo 9, 2026
**Próximo paso**: Leer PIA_05_CONTEXTO_IA.md

¡A trabajar! 🚀

