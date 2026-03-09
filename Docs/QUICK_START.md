# 🚀 QUICK START — PIA05 v2 Simplificada

**¿Qué cambió?** Agente 50% más pequeño, Notebook reestructurado, referencia PDF 502 explícita.

---

## ⚡ 3 OPCIONES RÁPIDAS

### 🎯 **OPCIÓN 1: Quiero empezar AHORA**
```
1. Abre: PIA_05_Tarea_01.ipynb (actualizado)
2. Celda 1: Lee introducción (3 líneas)
3. Celda 2: Setup (auto-detecta ruta + dataset)
4. Celda 3+: Sigue flujo 502 (DataBlock → Model → LR → ... → Conclusión)
⏱️ Tiempo: ~2 horas desarrollo
```

### 🤖 **OPCIÓN 2: Quiero DELEGAR a Gemini**
```
1. Copia archivo: Docs/GEMINI_INSTRUCTIONS_PIA05.md
2. Pégalo en Gemini prompt
3. Di: "Genera sección 3: Modelo Base 502"
4. Gemini te da código listo para copiar al notebook
⏱️ Tiempo: ~30 minutos (Gemini genera secciones en paralelo)
```

### 📱 **OPCIÓN 3: Quiero correr en COLAB (producción)**
```
1. git push cambios a GitHub
2. Abre: https://colab.research.google.com/
3. File → Open from GitHub → kachytronico/PIA_tarea_05_dataset
4. Selecciona: PIA_05_Tarea_01.ipynb
5. Runtime → Change runtime type → GPU
6. Cell → Run All
7. Valida métricas vs. Docs/VALIDACION_PRE_ENTREGA.md
⏱️ Tiempo: ~1 hora (notebook tarda ~45 min en ejecutar)
```

---

## 📚 GUÍAS POR PERFIL

### **Soy ESTUDIANTE/PRINCIPIANTE**
```
Lee en orden:
  1️⃣ Docs/INDICE_MAESTRO.md (5 min)
  2️⃣ Docs/README_PIA05.md (5 min)
  3️⃣ Docs/PIA_05_CONTEXTO_IA.md (10 min)
  4️⃣ Abre Notebook, sigue celda por celda

Link rápido: Docs/GEMINI_STYLE_GUIDE_PIA05.md 
  → Consulta mientras escribes código (comentarios 1era persona)
```

### **Soy REVISOR/PROFESOR**
```
Lee en orden:
  1️⃣ Docs/PIA_05_CONTEXTO_IA.md (contexto)
  2️⃣ Docs/PIA_05_CHECKLIST.md (qué validar)
  3️⃣ Docs/VALIDACION_PRE_ENTREGA.md (criterios aceptación)
  4️⃣ Abre Notebook, valida secciones vs checklist

Métrica crítica: Sensibilidad > 95% (FN < 5%)
```

### **Soy IA (Gemini/Claude)**
```
Instrucciones:
  1️⃣ Lee: Docs/GEMINI_INSTRUCTIONS_PIA05.md (MASTER integrador)
  2️⃣ Consulta mientras generas:
     - Docs/GEMINI_CONTEXT_PIA05.md (restricciones, patrones)
     - Docs/GEMINI_STYLE_GUIDE_PIA05.md (primera persona)
     - Docs/GEMINI_DEBUG_GUIDE_PIA05.md (si hay errores)
     - Docs/GEMINI_CHECKLIST_PIA05.md (QA antes de deliver)

Estructura: 10 secciones (no más, no menos)
  1. Intro  2. Setup  3. DataBlock 502  4. Model  5. LR
  6. Aug    7. AntiOV  8. Progressive  9. ResNet34  10. Confmatrix
```

---

## 🎯 ESTRUCTURA DEL NOTEBOOK (NUEVO)

```
Sección | Contenido | Celdas | Status
--------|-----------|--------|--------
1       | Introducción (+ links Docs) | 1 | ✅ Listo
2       | Setup + imports + GPU check | 2 | ✅ Listo
3       | DataBlock 502 (ref PDF) | 2 | ✅ Listo
4       | Modelo base (ResNet18) | 2 | ✅ Listo
5       | Learning Rate Finder | 2 | ✅ Listo
6       | Augmentación | 1 | ✅ Listo
7       | Anti-Overfitting | 1 | ✅ Listo
8       | Progressive Learning | 1 | ✅ Listo
9       | ResNet34 (comparativa) | 1 | ✅ Listo
10      | Matriz Confusión + análisis | 1 | ✅ Listo
        | TOTAL: ~14 celdas | | ✅ COMPLETO
```

---

## 📋 REQUISITOS (7 obligatorios, 2 opcionales)

| # | Requisito | PDF 502 | Notebook | Docs |
|---|-----------|---------|----------|------|
| 1 | Modelo Base | ✅ Ref | Sec 4 | `PIA_05_GUIA_OPERATIVA.md` |
| 2 | Learning Rate | ✅ Ref | Sec 5 | `GEMINI_STYLE_GUIDE_PIA05.md` |
| 3 | Anti-Overfitting | ✅ Ref | Sec 7 | `GEMINI_DEBUG_GUIDE_PIA05.md` |
| 4 | Augmentación | ✅ Ref | Sec 6 | `GEMINI_CHECKLIST_PIA05.md` |
| 5 | Progressive | ✅ Ref | Sec 8 | (implicit) |
| 6 | Segunda Arch | ❌ | Sec 9 | (implicit) |
| 7 | Confmatrix | ✅ Ref | Sec 10 | (critical) |
| (opt) | Grad-CAM | ❌ | (stub) | `GEMINI_DEBUG_GUIDE_PIA05.md` |
| (opt) | Insight | ❌ | (stub) | (context) |

✅ = 10 puntos  
(opt) = +2 puntos

---

## 🚨 CHECKLIST PRE-EJECUCIÓN

**Antes de correr en Colab:**

- [ ] ¿Tengo la última versión del notebook? (`git pull`)
- [ ] ¿Cambié GPU en Colab? (Runtime → GPU)
- [ ] ¿Los PDFs están en Notebooks/? (Ref 502 debe existir)
- [ ] ¿Lei GEMINI_INSTRUCTIONS_PIA05.md? (si delegué a IA)
- [ ] ¿Confirmé rutas relativas? (no hardcoded paths)

**Después de ejecutar:**

- [ ] Todas 10 secciones ejecutan sin error
- [ ] Accuracy ~85-87% (model base)
- [ ] FN < 5% en matriz confusión (crítico para Juan)
- [ ] Checkpoints guardados: `checkpoint_base`, `checkpoint_lr_optimized`, etc.
- [ ] Validé vs. `VALIDACION_PRE_ENTREGA.md`

---

## 📞 AYUDA RÁPIDA

**¿Error "Dataset no encontrado"?**
→ Ve a `GEMINI_DEBUG_GUIDE_PIA05.md` Sección 1

**¿LR diverge a NaN?**
→ Ve a `GEMINI_DEBUG_GUIDE_PIA05.md` Sección 2

**¿Accuracy demasiado baja?**
→ Ve a `CAMBIOS_REALIZADOS_v2.md` → Tabla de métricas esperadas

**¿Cómo escribo comentarios naturales?**
→ Abre `GEMINI_STYLE_GUIDE_PIA05.md` → Sección "Comentarios 1era persona"

**¿Qué significa FN/TP/TN/FP?**
→ Abre `PIA_05_CONTEXTO_IA.md` → "Contexto de Negocio"

---

## 📊 MÉTRICAS ESPERADAS

| Métrica | Baseline | Target | Alcanzable |
|---------|----------|--------|-----------|
| Accuracy | ~80% | >85% | ✅ 85-88% |
| Sensitivity (ESCA) | ~90% | >95% | ✅ 95-98% |
| FN (Falsos Negativos) | <10 | <5 | ✅ 2-4 |
| Training time | ~30-45 min | <1h | ✅✅ |

---

## 🎓 REFERENCIAS RÁPIDAS

**Archivo | Cuándo | Tiempo | Prioridad**
```
GEMINI_INSTRUCTIONS_PIA05.md    | Start         | 5 min  | 🔴 CRÍTICA
GEMINI_CONTEXT_PIA05.md         | Si delegas a IA| 10 min | 🔴 CRÍTICA
GEMINI_STYLE_GUIDE_PIA05.md     | Mientras escribes| 15 min| 🟡 IMPORTANTE
PIA_05_CONTEXTO_IA.md           | Para entender negocio | 15 min | 🟡 IMPORTANTE
GEMINI_DEBUG_GUIDE_PIA05.md     | Si hay error   | 5 min  | 🟢 ÚTIL
INDICE_MAESTRO.md               | Navegación     | 5 min  | 🟢 ÚTIL
VALIDACION_PRE_ENTREGA.md       | Antes GitHub   | 10 min | 🟡 IMPORTANTE
```

---

## ✨ CAMBIOS REALIZADOS (RESUMEN)

**v2 = más simple + referencia PDF + GEMINI guides**

```
Agente:       166 líneas → 80 líneas (-52%)
Notebook:     24 celdas → 10 secciones (-58%)
Documentación: Dispersa → 12 archivos organizados
PDF 502:      No referenciado → Explícito en celda 1
Usabilidad IA: Difícil → Auto-contenida (GEMINI_INSTRUCTIONS_PIA05.md)
```

Ver detalles en: `Docs/CAMBIOS_REALIZADOS_v2.md`

---

**¿LISTO? Elige arriba ⭐ y comienza.** 

**Próximo paso recomendado**: 
- Si eres usuario: Opción 1 (manual) 
- Si delegas: Opción 2 (Gemini)
- Si produces: Opción 3 (Colab)

🍇 **Buena suerte. Juan te necesita!**

