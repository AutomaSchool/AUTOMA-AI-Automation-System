# AUTOMA — Sistema Multiagente de Automatización con IA

> Ecosistema de agentes autónomos diseñado para automatizar la generación de contenido, gestión de leads y operaciones de una academia de IA, con supervisión humana y arquitectura portable.

**Autora:** Lucrecia Yuzek · Especialista en Automatización con IA
**Stack:** Make · Google Gemini · Claude · Google Sheets · Telegram
**Estado:** Agente de Contenidos en producción · resto en desarrollo

---

## ¿Qué es este proyecto?

AUTOMA es un sistema multiagente real y funcionando que automatiza tareas repetitivas de un negocio digital. No es un prototipo teórico: el Agente de Contenidos genera contenido diario de forma autónoma, con la voz de marca definida, y registra cada acción en una base de datos central.

El sistema está diseñado bajo el enfoque **Spec-Driven Development**: cada agente, regla y flujo está documentado antes de implementarse, lo que hace el sistema mantenible, auditable y portable.

---

## Arquitectura general

```
Lu (supervisión humana — aprobaciones)
    │
    ▼ Telegram
    │
[ORQUESTADOR]
    │
    ├── [AGENTE PM]          → estrategia y prioridades
    ├── [AGENTE CONTENIDOS]  → guiones, carruseles, copy  ✅ FUNCIONANDO
    └── [AGENTE EMAIL]       → gestión de leads y campañas

Todos leen y escriben estado en → Google Sheets (memoria compartida)
```

Los agentes no se comunican directamente entre sí: usan Google Sheets como estado compartido, un patrón que mantiene el sistema desacoplado y fácil de depurar.

---

## Principios de diseño

- **Supervisión humana obligatoria:** ninguna acción de cara al público (publicar, enviar emails) se ejecuta sin aprobación.
- **Portable, sin vendor lock-in:** toda la lógica vive en archivos Markdown y Google Sheets.
- **Guardarriales explícitos:** protocolo de contingencia para cada tipo de fallo.
- **Voz de marca consistente:** todos los agentes heredan un system prompt con reglas de tono.
- **Progresivo:** un agente sólido en producción antes que cuatro a medias.

---

## Flujo en producción: Agente de Contenidos

```
RSS (noticias IA) → Gemini (genera guión con voz de marca) →
Google Sheets (historial) → Google Sheets (log) → Telegram (aprobación)
```

Corre automáticamente cada día. Genera un guión de reel de 45-60 segundos en el tono de la marca, lo registra y lo envía para aprobación humana.

---

## Documentación del sistema

| Documento | Qué define |
|-----------|------------|
| `AUTOMA-ecosystem-spec` | Arquitectura completa y roadmap |
| `AUTOMA-brand-bible` | Identidad de marca, voz y tono |
| `AUTOMA-system-prompt-base` | Reglas base de todos los agentes |
| `AUTOMA-prompts-agentes` | System prompt individual de cada agente |
| `AUTOMA-base-conocimiento` | Oferta, FAQs, objeciones |
| `AUTOMA-protocolo-contingencia` | Manejo de fallos (7 escenarios) |
| `AUTOMA-lead-scoring` | Clasificación automática de leads |
| `AUTOMA-calendario-editorial` | Pilares de contenido y cadencia |
| `AUTOMA-sheets-schema-versioning` | Esquema de base de datos |
| `AUTOMA-llm-policy` | Qué modelo de IA usa cada agente |
| `AUTOMA-make-flow-map` | Plano de implementación en Make |

---

## Habilidades demostradas en este proyecto

- Diseño de arquitectura de sistemas multiagente
- Automatización de procesos con Make (RSS, routers, multi-step)
- Integración de LLMs (Gemini, Claude) en flujos productivos
- Ingeniería de prompts con voz de marca consistente
- Diseño de bases de datos en Google Sheets
- Documentación técnica bajo enfoque Spec-Driven
- Diseño de guardarriales y manejo de errores

---

## Sobre la autora

Lucrecia Yuzek — formación en pedagogía y educación especial, diplomada en Inteligencia Artificial. Combina pensamiento lógico, capacidad de enseñar conceptos complejos de forma simple, y un enfoque metódico para construir sistemas de automatización confiables.

🌐 [soyautoma.com](https://soyautoma.com)
💼 [LinkedIn](https://www.linkedin.com/in/lucreciayuzek-edtch-ia)
