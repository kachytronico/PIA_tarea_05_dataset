# GEMINI — INSTRUCCIONES MAESTRAS PIA05

**Como Gemini/IA**: Este es tu documento de referencia para toda la Tarea PIA05.
**Usuario**: Pásale esto a cualquier IA que vayas a usar (Gemini, Claude, etc.) para contexto completo.

---

## 🎯 MISIÓN

Construir un notebook Jupyter reproducible que **diagnostique plantas de vid infectadas por ESCA** (clasificación binaria: Sana/Infectada) usando **FastAI v2 exclusivamente**.

**Stakeholder**: Juan, agricultor que pierde dinero eliminando plantas sanas.

---

## ✅ CHECKLIST DE REQUISITOS (Obligatorios: 10 puntos)

| # | Requisito | Puntos | Status |
|---|-----------|--------|--------|
| 1 | Modelo Base (ResNet18, cuadernillo 502) | 4 | ⏳ |
| 2 | Learning Rate Óptimo (lr_find) | 1 | ⏳ |
| 3 | Anti-Overfitting (callbacks/wd) | 1 | ⏳ |
| 4 | Aumento de Datos (augmentación) | 1 | ⏳ |
| 5 | Progressive Learning (dos resoluciones) | 1 | ⏳ |
| 6 | Segunda Arquitectura (ResNet34/50) | 1 | ⏳ |
| 7 | Matriz de Confusión (análisis FP/FN) | 1 | ⏳ |
| **TOTAL** | | **10** | ⏳ |
| (Opt) | Grad-CAM (interpretabilidad) | 1 | ⏳ |
| (Opt) | Insight del Problema | 1 | ⏳ |

---

## 📋 DOCUMENTOS DE REFERENCIA (EN `/Docs/`)

**Lee en este orden:**

1. **GEMINI_CONTEXT_PIA05.md** ← Contexto general + restricciones
2. **GEMINI_STYLE_GUIDE_PIA05.md** ← Cómo escribir (primera persona, natural)
3. **GEMINI_DEBUG_GUIDE_PIA05.md** ← Errores comunes y soluciones
4. **GEMINI_CHECKLIST_PIA05.md** ← Validación antes de entregar

**Documentos técnicos adicionales:**
- `PIA_05_CONTEXTO_IA.md` — Definición expandida
- `PIA_05_GUIA_ESTILO.md` — Normas Markdown/formato
- `PIA_05_GUIA_OPERATIVA.md` — Paso a paso con código
- `PIA_05_CHECKLIST.md` — Seguimiento detallado

---

## 🔑 REGLAS NO NEGOCIABLES

### ❌ PROHIBIDO
- ❌ TensorFlow, Keras, PyTorch puro → **Solo FastAI v2**
- ❌ Entrenar desde cero → **Siempre fine-tuning**
- ❌ Rutas hardcodeadas → **Rutas relativas, auto-detect**
- ❌ Sin `set_seed(42)` → **Reproducibilidad**
- ❌ Conclusiones sin números → **Métricas concretas**
- ❌ Comentarios genéricos → **Primera persona, natural**

### ✅ OBLIGATORIO
- ✅ `from fastai.vision.all import *` (SOLO esto)
- ✅ `DataBlock` + `dataloaders` + `vision_learner`
- ✅ `learn.lr_find()` siempre
- ✅ `batch_tfms=aug_transforms()`
- ✅ `EarlyStoppingCallback` Y/O `wd=0.1`
- ✅ Progressive learning (Resize 128 → 224)
- ✅ Matriz de confusión con análisis contextual
- ✅ Guardar checkpoints: `learn.save()`

---

## 📝 ESTRUCTURA DEL NOTEBOOK (10-12 secciones)

```
1. Contexto del Problema (Markdown)
2. Setup de Entorno (Código)
3. Carga Dataset (Código + Viz)
4. 🌟 Modelo Base (502) — 4 puntos
5. 🌟 Learning Rate Óptimo — 1 punto
6. 🌟 Aumento de Datos — 1 punto
7. 🌟 Anti-Overfitting — 1 punto
8. 🌟 Progressive Learning — 1 punto
9. 🌟 Segunda Arquitectura — 1 punto
10. 🌟 Matriz de Confusión — 1 punto
11. ✨ (Opcional) Grad-CAM
12. Conclusión General
```

**Patrón de cada sección** (excepto últimas):
```
- Markdown: Explicación técnica (3-5 líneas)
- Código: Implementación (con comentarios en primera persona)
- Conclusión: 4-8 líneas, con números, en primera persona
```

---

## 💬 ESTILO DE REDACCIÓN

### En Código (Comentarios)
```python
# ✅ BIEN: Primera persona, decisión técnica
# Voy a usar ResNet18 porque la GPU es limitada
# Observo en lr_find que diverge après 1e-2, así que elijo 1e-3
# Creo que batch_size=32 es buen balance velocidad/memoria

# ❌ MAL: Pasivo, genérico
# Crear modelo
# Fine-tuning
# Optimizar
```

### En Conclusiones
```
✅ BIEN:
"El modelo base alcanza 82.4% accuracy observando que la diferencia 
train/val es ~7 puntos. Esto sugiere overfitting leve controlable. 
Voy a usar esta métrica como baseline para medir mejoras posteriores."

❌ MAL:
"El modelo funciona bien. La accuracy mejora. Es un buen resultado."
```

### Reglas
- 1era persona: "observo", "veo", "creo que", "voy a"
- Evitar pasivo: "se observa" → NO. "observo" → SÍ
- Con números: accuracy debe ser 82.4%, NO "aproximadamente 82%"
- Justificación: Cada decisión respaldada en output

---

## 🔍 CONTEXTO DE NEGOCIO (CRÍTICO)

**Juan, agricultor**: Tiene cientos de vides.
**Problema**: 
- Elimina plantas sanas por sospechas de ESCA (costo innecesario)
- No detecta a tiempo todas las enfermas (contagio masivo)

**Impacto de errores**:
- **FN (Falso Negativo)**: Enferma → predicción sana → **CATASTROFE contagio**
- **FP (Falso Positivo)**: Sana → predicción enferma → Costo tolerable (inspección manual)

**Para la matriz de confusión**:
> "TP=45, TN=52, FP=8 (tolerables), FN=3 (RIESGO BAJO). 
> Sensibilidad=94%, aceptable: casi toda enfermedad se detecta. 
> Recomendación para Juan: Confianza ALTA, producible. ✓"

---

## 🆘 ERRORES COMUNES (Ver GEMINI_DEBUG_GUIDE_PIA05.md)

| Error | Solución Rápida |
|-------|-----------------|
| CUDA OOM | `bs=32` → `bs=16`, reiniciar kernel |
| Loss=NaN | LR pre-valle no divergencia, empezar 1e-5 |
| Accuracy=100% | Verificar split (data leakage?) |
| Train↓ Val↑ | Aumentar wd, añadir callback |
| Loss no mejora | Aumentar LR, revisar desbalance |

---

## ✨ BONUS OPCIONAL (2 puntos)

### Grad-CAM (1 punto)
- Visualizar dónde "mira" la red (heatmap)
- Validar: ¿manchas reales en hojas? (✓ buen) ¿o fondo? (✗ sesgo)
- Conclusión: Confianza en decisión del modelo

### Insight (1 punto)
- Qué está aprendiendo realmente
- Tipos de ESCA más/menos visibles
- Sesgos detectados
- Mitiga propuestas

---

## 📊 VALIDACIÓN ANTES DE ENTREGAR

**Ejecutar a mano:**
```python
# GPU OK?
print(torch.cuda.is_available())

# Split OK?
print(f"Train: {len(dls.train)}, Valid: {len(dls.valid)}")

# Modelos OK?
# Path('checkpoint_base.pth') existe → learn.save() funcionó
```

**Notebook OK?**
- [ ] Ejecuta 100% sin errores (Restart Kernel + Run All)
- [ ] Todas 10 secciones presentes
- [ ] Conclusiones NO genéricas
- [ ] Comentarios en primera persona
- [ ] Matriz análisis contextualizado
- [ ] Rutas relativas

---

## 🎓 FLUJO RECOMENDADO

**Día 1 (1-2 horas)**: Setup + Lectura documentación
1. Lee GEMINI_CONTEXT_PIA05.md (20 min)
2. Lee GEMINI_STYLE_GUIDE_PIA05.md (20 min)
3. Descarga dataset de Mendeley, verifica en Colab

**Día 2 (2-3 horas)**: Ejecución
1. Implementa secciones  1-5 (Contexto + Base + LR)
2. Ejecuta y valida cada una
3. Marca progreso en GEMINI_CHECKLIST_PIA05.md

**Día 3 (2-3 horas)**: Mejoras + Análisis
1. Implementa secciones 6-10 (Augmentación + Anti-OV + Progressive + Arch2 + Confmatrix)
2. Ejecuta y valida
3. Realiza análisis contextualizado (Juan!)

**Día 4 (30 min-1h)**: Bonus + Revisión
1. (Opcional) Grad-CAM e Insight
2. Ejecuta todo 1x sin errores (Restart + Run All)
3. PaseRefresh GEMINI_CHECKLIST_PIA05.md completo

---

## 🚀 CÓMO PEDIR AYUDA

### Formato Correcto
> "Genera la sección **[NOMBRE]** del notebook de PIA05 usando FastAI v2.
> Incluye:
> 1) Markdown explicativo (3-5 renglones)
> 2) Código ejecutable con comentarios en primera persona
> 3) Conclusión técnica (4-8 líneas, con números)
> Contexto: Clasificación binaria ESCA en vides, dataset Mendeley."

### Ejemplos Válidos
- "Genera la sección **4. Modelo Base (502)** del notebook..."
- "Genera la sección **10. Matriz de Confusión** del notebook..."
- "¿Error CUDA OOM? Consulta GEMINI_DEBUG_GUIDE_PIA05.md"

### Ejemplos Inválidos
```
"Hazme el notebook" ← Demasiado vago
"Ayudame rápido" ← Falta contexto
"Código del modelo" ← Qué sección?
```

---

## 📞 RECURSOS

- **FastAI Docs**: https://docs.fast.ai/
- **Mendeley Dataset**: https://data.mendeley.com/datasets/89cnxc58kj/1
- **GitHub Repo**: https://github.com/kachytronico/PIA_tarea_05_dataset
- **Google Colab**: https://colab.research.google.com/

---

## 📋 RESUMEN EJECUTIVO

| Aspecto | Valor |
|---------|-------|
| **Tecnología** | FastAI v2 + PyTorch (transfer learning) |
| **Problema** | Clasificación binaria: Sana/Infectada |
| **Dataset** | ~200 imágenes (Mendeley official) |
| **Puntos** | 10 obligatorios + 2 opcionales |
| **Tiempo est.** | 6-8 horas efectivas |
| **Requisito crítico** | Baja tasa de FN (minimizar contagio masivo) |
| **Lenguaje** | 1era persona, natural, con números |
| **Reproducibilidad** | set_seed(42), rutas relativas |

---

**Próximo paso**: Lee GEMINI_CONTEXT_PIA05.md y comienza el notebook sección por sección.

¡Buena suerte! 🚀 (Juan te necesita 🍇)

