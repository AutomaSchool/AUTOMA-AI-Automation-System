# AUTOMA Ecosystem — README
> Índice oficial del ecosistema multiagente de soyAUTOMA  
> Lu — Fundadora · soyautoma.com · Última actualización: junio 2026

---

## Qué es esto

Este repositorio contiene las especificaciones, reglas de comportamiento y documentos operativos del ecosistema de agentes autónomos de AUTOMA. No es código — es el cerebro escrito del sistema.

**Regla de oro:** si un agente no lo tiene en este repo, no lo sabe.

---

## Mapa del ecosistema

```
Lu (aprobadora final)
    │
    ▼ Telegram (#Aprobaciones · #Alertas · #Reportes)
    │
[ORQUESTADOR] ← trigger: 9am lunes a sábado
    │
    ├── [AGENTE PM]         → prioridad semanal, métricas
    ├── [AGENTE CONTENIDOS] → guiones, carruseles, copy
    └── [AGENTE EMAIL]      → leads, campañas, nurturing

Todos leen/escriben en → AUTOMA CONTROL CENTRAL (Google Sheets)
```

---

## Documentos del repo — leer en este orden

### Capa 1 — Identidad (leer primero siempre)
| Documento | Qué define | Cuándo leerlo |
|-----------|------------|---------------|
| `AUTOMA-brand-bible-v1.0.md` | Quién es Lu, voz, tono, audiencia | Antes de generar cualquier contenido |
| `AUTOMA-system-prompt-base-v1.0.md` | Reglas base de todos los agentes | Al iniciar cualquier sesión de agente |

### Capa 2 — Estructura
| Documento | Qué define | Cuándo leerlo |
|-----------|------------|---------------|
| `AUTOMA-ecosystem-spec-v1.0.md` | Arquitectura, agentes, roadmap | Al diseñar o modificar el ecosistema |
| `AUTOMA-sheets-schema-versioning-v1.0.md` | Columnas exactas de Sheets + versioning | Al configurar Make o agregar pestañas |

### Capa 3 — Operación
| Documento | Qué define | Cuándo leerlo |
|-----------|------------|---------------|
| `AUTOMA-protocolo-contingencia-v1.0.md` | Qué hacer cuando algo falla | Siempre que haya un error |
| `AUTOMA-lead-scoring-v1.0.md` | Cómo clasificar leads automáticamente | Al configurar el Agente Email |
| `AUTOMA-calendario-editorial-v1.0.md` | Pilares, cadencia, formatos por plataforma | Antes de generar contenido semanal |
| `AUTOMA-llm-policy-v1.0.md` | Qué LLM usa cada agente y cómo | Al configurar cualquier agente en Make |
| `AUTOMA-make-flow-map-v1.0.md` | Qué construir exactamente en Make | Al implementar o modificar escenarios |

### Capa 4 — Prompts de agentes
| Documento | Qué define | Cuándo usarlo |
|-----------|------------|---------------|
| `AUTOMA-prompt-orquestador-v1.0.md` | System prompt del Orquestador | Al abrir sesión del Orquestador |
| `AUTOMA-prompt-pm-v1.0.md` | System prompt del Agente PM | Al abrir sesión del Agente PM |
| `AUTOMA-prompt-contenidos-v1.0.md` | System prompt del Agente Contenidos | Al abrir sesión de Contenidos |
| `AUTOMA-prompt-email-v1.0.md` | System prompt del Agente Email | Al abrir sesión de Email |

### Capa 5 — Conocimiento
| Documento | Qué define | Cuándo leerlo |
|-----------|------------|---------------|
| `AUTOMA-base-conocimiento-v1.0.md` | Oferta, precios, FAQs, objeciones | Antes de hablar de ventas o cursos |

---

## Stack tecnológico

| Herramienta | Rol en el ecosistema | Costo |
|-------------|----------------------|-------|
| Claude Pro (claude.ai) | Cerebro de Orquestador, PM y Email | $20 USD/mes |
| Gemini (Make) | Trigger de contenidos RSS | Gratis |
| Make free | Automatización y flujos | $0 → $9 cuando se necesite 3er escenario |
| Google Sheets | Memoria compartida del ecosistema | Gratis |
| Telegram | Canal de comunicación con Lu | Gratis |
| GitHub | Versioning de este repo | Gratis |
| Antigravity | IDE de desarrollo | Gratis |
| Canva Pro / CapCut Pro / Gmail / Hostinger | Ejecución | Ya pagados |

---

## Convenciones

- **Aprobaciones:** siempre por Telegram → ✅ aprobado o ❌ rechazado: [motivo]
- **Versioning:** v1.0 → v1.1 cambio menor · v2.0 cambio estructural
- **Nombres de archivo:** AUTOMA-[nombre]-v[X.X].md — sin espacios
- **Versiones viejas:** archivar en /archivo/ — nunca borrar

---

## Cómo usar este repo en una sesión de agente

1. Abrí Antigravity
2. Abrí el prompt del agente correspondiente
3. Copiá el contenido completo
4. Pegalo como primer mensaje en Claude (claude.ai)
5. El agente ya está configurado — empezá a trabajar

---

*Repositorio: AutomaSchool/Automawebacademy · Rama principal: main*
