# AUTOMA — LLM Policy v1.0
> Qué modelo usa cada agente, cómo se conecta y qué pasa si cambia.
> Última actualización: junio 2026

---

## Principio base

El ecosistema AUTOMA no depende de un solo LLM. Cada agente tiene un modelo principal y un fallback. Si el principal falla o sube de precio, el sistema sigue funcionando con el fallback sin reconfigurar todo.

---

## Asignación de LLM por agente

| Agente | LLM Principal | Cómo se usa | Fallback | Costo actual |
|--------|--------------|-------------|----------|--------------|
| Orquestador | Claude Pro (claude.ai) | Manual — Lu abre sesión y pega el prompt | Gemini web | $0 adicional (incluido en Pro) |
| Agente PM | Claude Pro (claude.ai) | Manual — sesión semanal lunes y viernes | Claude Haiku API | $0 adicional |
| Agente Contenidos | Gemini (Make) + Claude Pro | Gemini genera trigger RSS · Claude refina borradores complejos | Gemini solo | $0 |
| Agente Email | Claude Pro (claude.ai) | Manual — sesión por campaña o alerta de lead caliente | Claude Haiku API | $0 adicional |

---

## Cómo se inyecta el contexto según el modo de uso

### Modo manual (Claude Pro web / Antigravity)
```
Paso 1: Abrí nueva sesión en claude.ai o Antigravity
Paso 2: Copiá y pegá el contenido de AUTOMA-system-prompt-base-v1.0.md
Paso 3: Copiá y pegá el contenido de AUTOMA-brand-bible-v1.0.md
Paso 4: Copiá y pegá el contenido de AUTOMA-base-conocimiento-v1.0.md
Paso 5: Copiá y pegá el prompt específico del agente
Paso 6: Empezá a trabajar — el agente ya tiene todo el contexto
```

### Modo automático (Make + Gemini — ya configurado)
```
El system prompt de AUTOMA se inyecta como primer campo del módulo Gemini:
- Campo "System instruction": pegar el contenido de AUTOMA-system-prompt-base-v1.0.md
- Campo "Prompt": el contenido dinámico del RSS + instrucción de formato
IMPORTANTE: Sin este paso, Gemini genera contenido genérico sin voz de Lu
```

### Modo API (Make + Claude API — futuro)
```
Requiere: API key de Anthropic (gratis con $5 de crédito inicial)
Módulo en Make: HTTP → POST → https://api.anthropic.com/v1/messages
Headers: x-api-key: [tu API key] · anthropic-version: 2023-06-01
Body: model: claude-sonnet-4-6 · max_tokens: 1000 · system: [prompt base] · messages: [...]
```

---

## Regla de sustitución (cuándo cambiar de LLM)

| Situación | Acción |
|-----------|--------|
| Claude Pro sube de precio > $30 USD/mes | Evaluar migrar Orquestador y PM a Gemini Pro |
| Gemini falla en Make por más de 24hs | Usar módulo HTTP con Claude API como reemplazo |
| Claude API supera $5 USD/mes de uso | Revisar si Make Core ($9/mes) vale más que la API |
| Lu quiere 0 costos de LLM | Activar Ollama local (Dell i7, 32GB RAM) para tareas simples |

---

## Ollama — opción local de emergencia

Lu tiene un Dell i7 con 32GB RAM y NVIDIA GPU corriendo Windows 11. Ollama está disponible como opción de costo cero para tareas que no requieren salir a internet.

**Cuándo usar Ollama:**
- Clasificación de leads (tarea simple, no necesita la mejor calidad)
- Resúmenes de Sheets para el reporte del Orquestador
- Borrador inicial de emails antes de refinar con Claude

**Cuándo NO usar Ollama:**
- Contenido para publicar (la calidad importa — usar Claude)
- Respuestas a leads calientes (el tono delicado requiere Claude)
- Análisis estratégico del Agente PM

**Modelos recomendados para Ollama:**
- `llama3.2:3b` — tareas de clasificación y resumen (rápido, liviano)
- `mistral:7b` — redacción de borradores simples

---

## Lo que los LLMs NO pueden decidir solos

Independientemente del LLM usado, estas decisiones siempre requieren OK de Lu:
- Publicar contenido en cualquier red social
- Enviar emails a contactos reales
- Modificar precios o descripciones de oferta
- Responder quejas o conflictos
- Cambiar la prioridad de la semana

---

## Versioning de modelos

Cuando Anthropic o Google lance un modelo nuevo relevante, actualizar este documento:
- Testear el nuevo modelo con 3 outputs de cada agente
- Comparar contra el modelo actual
- Si mejora calidad → migrar y actualizar este campo
- Modelo activo actual: `claude-sonnet-4-6` (junio 2026)

---

*Versión 1.0 — junio 2026. Revisar si cambia el precio de Claude Pro o Make.*
