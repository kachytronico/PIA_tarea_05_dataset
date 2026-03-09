# ÍNDICE MAESTRO — PIA05

**Guía de navegación: Dónde encontrar qué en el ecosistema PIA05**

---

## 📍 ESTRUCTURA DEL WORKSPACE

```
f:\Datos_alf_ SDD1\OneDrive - Diabeticos Asociados Riojanos\
Documentos\GitHub\PIA_tarea_05_dataset\
│
├── PIA_05_Tarea_01.ipynb ..................... 🎯 NOTEBOOK PRINCIPAL
├── Docs/
│   ├── 📘 USUARIO (5 archivos)
│   │   ├── PIA_05_CONTEXTO_IA.md ........... Definición problema, negocio, dataset
│   │   ├── PIA_05_GUIA_ESTILO.md .......... Formato, standards, convenciones
│   │   ├── PIA_05_GUIA_OPERATIVA.md ...... Paso a paso implementación
│   │   ├── PIA_05_CHECKLIST.md ........... Validación sección por sección
│   │   └── README_PIA05.md ............... Índice usuario-facing
│   │
│   ├── 🤖 IA/GEMINI (5 archivos)
│   │   ├── GEMINI_CONTEXT_PIA05.md ....... Contexto para IA (restricciones, patrones)
│   │   ├── GEMINI_STYLE_GUIDE_PIA05.md .. CÓMO escribir (1era persona, natural)
│   │   ├── GEMINI_DEBUG_GUIDE_PIA05.md .. Errores comunes + soluciones
│   │   ├── GEMINI_CHECKLIST_PIA05.md .... QA para cada sección (IA side)
│   │   └── GEMINI_INSTRUCTIONS_PIA05.md . MAESTRO integrador (lee primero)
│   │
│   ├── 🎯 PROYECTO (2 archivos)
│   │   ├── ESTADO_PROYECTO_PIA05.md ...... Status actual (65% v2)
│   │   └── VALIDACION_PRE_ENTREGA.md .... Checklist antes GitHub + últimos pasos
│   │
│   └── (otros docs PIA04)
│
├── esca_dataset/
│   ├── esca/infected/
│   ├── esca/healthy/
│   └── annotations.csv
│
├── Notebooks/ ............................ [Vacío, usar PIA_05_Tarea_01.ipynb]
│
└── .github/agents/
    └── PIA_Tarea_05.agent.md ............ Agent instructions actualizado
```

---

## 🎯 "¿QUÉ DOCUMENTO LEO PARA...?"

### 🔰 **Soy NUEVO y quiero aprender rápido**
**Lectura recomendada** (30 minutos):
1. `README_PIA05.md` — Introducción amable (5 min)
2. `PIA_05_CONTEXTO_IA.md` — Problema detallado (10 min)
3. `GEMINI_INSTRUCTIONS_PIA05.md` — Resumen ejecutivo (10 min)
4. Salta al notebook y abre sección 1

---

### 💻 **Voy a DESARROLLAR código (eres el usuario entregable)**
**Lectura recomendada** (1 hora):
1. `PIA_05_CONTEXTO_IA.md` — Entiende el problema (15 min)
2. `PIA_05_GUIA_ESTILO.md` — Aprende convenciones (10 min)
3. `PIA_05_GUIA_OPERATIVA.md` — Sigue paso a paso (20 min)
4. `PIA_05_CHECKLIST.md` — Valida mientras desarrollas (15 min)
5. **Abre `PIA_05_Tarea_01.ipynb` y comienza**

---

### 🤖 **He delegado a Gemini/Claude y necesito INSTRUCCIONES para ELLO**
**Lectura recomendada** (10 minutos):
1. `GEMINI_INSTRUCTIONS_PIA05.md` — Pásale este archivo al IA (5 min)
2. Pásale también `GEMINI_CONTEXT_PIA05.md` — Contexto técnico (5 min)
3. Si hay errores, apunta a `GEMINI_DEBUG_GUIDE_PIA05.md`

---

### 🔧 **Tengo ERROR y necesito DIAGNOSTICAR**
**Lectura recomendada** (5-10 minutos):
1. `GEMINI_DEBUG_GUIDE_PIA05.md` — 5 errores FastAI comunes (5 min)
2. Si no está → `PIA_05_GUIA_OPERATIVA.md` → Sección "Troubleshooting" (5 min)

---

### ✅ **Voy a VALIDAR el notebook antes de GitHub**
**Checklist**:
1. Abre `VALIDACION_PRE_ENTREGA.md` (esta es tu rubrica)
2. Ejecuta sección por sección: `PIA_05_Tarea_01.ipynb`
3. Cruza cada output con expectativas en `PIA_05_CHECKLIST.md`

---

### 📤 **Voy a SUBIR a GitHub. ¿Qué hacer?**
**Pasos**:
1. Lee `VALIDACION_PRE_ENTREGA.md` → Matriz de progreso (1 min)
2. Si % < 100%, completa secciones pendientes (ve tabla)
3. Usa `PIA_05_CHECKLIST.md` para final validation
4. Sigue instrucciones GitHub en `VALIDACION_PRE_ENTREGA.md` → "Proceso de subida a GitHub"

---

### 📊 **¿Dónde está mi STATUS? ¿Cuánto falta?**
**Lectura**:
- `ESTADO_PROYECTO_PIA05.md` — Estado actual, porcentaje, fecha last update
- `VALIDACION_PRE_ENTREGA.md` → Sección "MATRIZ DE PROGRESO" — visual

---

## 🔑 ARCHIVOS POR FUNCIÓN

### Contexto & Negocio (Lee primero)
| Archivo | Propósito | Audiencia | Tiempo |
|---------|----------|-----------|--------|
| `README_PIA05.md` | Intro amable | Todo mundo | 5 min |
| `PIA_05_CONTEXTO_IA.md` | Problema técnico + negocio | Developers | 15 min |
| `ESTADO_PROYECTO_PIA05.md` | Status actual | Project managers | 5 min |

### Guías Operativas (Sigue estos para desarrollar)
| Archivo | Propósito | Audiencia | Tiempo |
|---------|----------|-----------|--------|
| `PIA_05_GUIA_ESTILO.md` | Convenciones código + Markdown | Developers | 10 min |
| `PIA_05_GUIA_OPERATIVA.md` | Paso a paso con código | Developers | 20 min |
| `PIA_05_CHECKLIST.md` | Validación sección/sección | Developers | 5-10 min |

### Guías para IA (Pásale estos a Gemini)
| Archivo | Propósito | Audiencia | Tiempo |
|---------|----------|-----------|--------|
| `GEMINI_INSTRUCTIONS_PIA05.md` | Instrucciones maestras | IA assistants | 5 min |
| `GEMINI_CONTEXT_PIA05.md` | Contexto detallado + restricciones | IA assistants | 10 min |
| `GEMINI_STYLE_GUIDE_PIA05.md` | Cómo escribir como humano | IA assistants | 10 min |
| `GEMINI_DEBUG_GUIDE_PIA05.md` | Errores FastAI + soluciones | IA assistants | 5 min |
| `GEMINI_CHECKLIST_PIA05.md` | QA antes de entregar | IA assistants | 5 min |

### Validación & Deployment
| Archivo | Propósito | Audiencia | Tiempo |
|---------|----------|-----------|--------|
| `VALIDACION_PRE_ENTREGA.md` | Checklist GitHub + últimos pasos | Developers | 10 min |
| `INDICE_MAESTRO.md` | Este archivo (navigation) | Todo mundo | 5 min |

---

## 🌳 ÁRBOL DE DECISIONES

```
        ¿QUÉ NECESITO?
              │
     ┌────────┼────────┐
     │        │        │
  APRENDER  DESARROLLAR  VALIDAR
     │        │        │
     1        2        3
     │        │        │
README_ETA ├─→ GUIA_ESTILO ──→ CHECKLIST
CONTEXTO ─→ GUIA_OPERATIVA   VALIDACION
         │                └─→ GitHub
         │
    ¿ERROR?
         │
      DEBUG_GUIDE
         │
      GEMINI_* docs
         │
      Pedir ayuda IA
```

---

## 📋 QUICK REFERENCE

### Rutas Importantes
```
Notebook principal:
  f:\...\PIA_tarea_05_dataset\PIA_05_Tarea_01.ipynb

Dataset raíz:
  f:\...\PIA_tarea_05_dataset\esca_dataset\esca\
  ├── healthy/
  └── infected/

Docs raíz:
  f:\...\PIA_tarea_05_dataset\Docs\
  
Agent instructions:
  f:\...\PIA_tarea_05_dataset\.github\agents\PIA_Tarea_05.agent.md
```

### Comandos Git
```bash
# Status
git status

# Ver cambios recientes
git log --oneline -5

# Preparar commit notebook
git add PIA_05_Tarea_01.ipynb Docs/

# Commit
git commit -m "feat: PIA05 v2 notebook completo estilo GEMINI"

# Push
git push origin main
```

### URLs Importantes
```
GitHub repo:
  https://github.com/kachytronico/PIA_tarea_05_dataset

Dataset Mendeley:
  https://data.mendeley.com/datasets/89cnxc58kj/1

FastAI docs:
  https://docs.fast.ai/

Google Colab (para testing):
  https://colab.research.google.com/
```

---

## 🎓 LECTURA RECOMENDADA POR PERFIL

### 👨‍🎓 Estudiante Implementando
```
Semana 1:
  Día 1: README + CONTEXTO (1h)
  Día 2: GUIA_ESTILO + GUIA_OPERATIVA (1.5h)
  Día 3-5: Implementar siguiendo CHECKLIST (3h)

Semana 2:
  Día 1-3: Validar + refinar (2h)
  Día 4-5: Testing Colab + GitHub push (1h)
```

### 👨‍💼 Profesor/Reviewer
```
  Primera vez: README + CONTEXTO (30 min)
  Review code: GUIA_ESTILO + CHECKLIST (20 min)
  Final approval: VALIDACION_PRE_ENTREGA (10 min)
```

### 🤖 IA Assistant (Gemini/Claude)
```
  Setup: GEMINI_INSTRUCTIONS_PIA05 (5 min) ← START HERE
  Context: GEMINI_CONTEXT_PIA05 (10 min)
  Develop: GEMINI_STYLE_GUIDE_PIA05 (10 min per section)
  Debug: GEMINI_DEBUG_GUIDE_PIA05 (on error)
  QA: GEMINI_CHECKLIST_PIA05 (before deliver)
```

---

## ✨ CARACTERÍSTICAS ESPECIALES

### 📊 Matriz de Requisitos
En `VALIDACION_PRE_ENTREGA.md`:
- Status de cada sección (% complete)
- Puntos asociados (10 obligatorios + 2 opcionales)
- Próximos pasos específicos

### 🔍 Debug Pathways
`GEMINI_DEBUG_GUIDE_PIA05.md` includes:
- 5 errores FastAI / PyTorch más comunes
- Síntomas
- Diagnóstico paso a paso
- Soluciones con código

### 💡 Business Context Everywhere
Todos los docs mencionan a **Juan** (agricultor) y por qué:
- FN es catastrófico (contagio masivo)
- FP es tolerable (inspección manual barata)
- Recomendación final debe ser confiada para que Juan confíe

---

## 🚀 FLUJO END-TO-END

```
START
  ↓
Leer README_PIA05.md .................... 5 min
  ↓
Leer PIA_05_CONTEXTO_IA.md .............. 15 min
  ↓
Leer GUIA_ESTILO + GUIA_OPERATIVA ....... 30 min
  ↓
Abrir NOTEBOOK y empezar Sección 1 ...... ⏱️ DEVELOPMENT
  ↓
Mientras develops: Consulta PIA_05_CHECKLIST.md
  ↓
Completar todas 10 secciones ........... 
  ↓
Ejecutar VALIDACION_PRE_ENTREGA.md .... 30 min QA
  ↓
Notebook 100% ejecutable?
  ├─ NO → Ir a GEMINI_DEBUG_GUIDE_PIA05
  └─ YES → Sigue
  ↓
Git commit + push to GitHub ........... 10 min
  ↓
END (Listo para Juan + profesores!)
```

---

## 📞 SOPORTE

**¿Lees esto y aún no sabes qué hacer?**

1. ✅ Este documento (INDICE_MAESTRO.md) — Navigation
2. ✅ Pasa a `README_PIA05.md` — Intro amable
3. ✅ Luego `PIA_05_CONTEXTO_IA.md` — Problem statement
4. ✅ Salta al notebook Sección 1

**¿Tienes error técnico?**

1. ✅ Abre `GEMINI_DEBUG_GUIDE_PIA05.md`
2. ✅ Si no está → Pide ayuda a Gemini pasando ese guía

**¿No avanzas?**

1. ✅ Revisa matriz progreso en `VALIDACION_PRE_ENTREGA.md`
2. ✅ Sigue próximo paso recomendado ("⏳ En progreso")

---

## 📈 ÚLTIMA ACTUALIZACIÓN

- **Creado**: [Insertar fecha]
- **Última revisión**: [Insertar fecha]
- **Version**: 2.0 (Con GEMINI guides)
- **Status**: Ready for development

---

**¡Ya puedes empezar! Buena suerte 🍇** 

Próximo: Abre `README_PIA05.md` o salta directo al notebook `PIA_05_Tarea_01.ipynb`

