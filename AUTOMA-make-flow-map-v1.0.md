# AUTOMA — Make Flow Map v1.0
> El plano exacto de qué construir en Make. Cada escenario, cada módulo, en orden.
> Última actualización: junio 2026

---

## Estado actual de Make

| Escenario | Estado | Slots Make |
|-----------|--------|------------|
| RSS → Gemini → Sheets → Telegram | Inactivo (funciona) | Slot 1 |
| Gmail → Sheets → Router → 5 respuestas | En prueba | Slot 2 |
| Orquestador 9am | Pendiente crear | Requiere Make Core ($9/mes) |

**Plan free: 2 slots activos.** Para el Orquestador se necesita Make Core.

---

## ESCENARIO 1 — Contenidos RSS (ya existe · ajustar)

**Nombre:** `AUTOMA - Contenidos RSS`  
**Trigger:** Watch RSS Feed · cada 6 horas  
**Objetivo:** Detectar novedades de IA, generar borrador de guión con voz de Lu, registrar en Sheets y notificar en Telegram

### Módulos en orden:

```
[1] RSS — Watch RSS Feed
    URL: [feeds de IA en español — ver lista abajo]
    Limit: 3 items por ejecución

[2] Gemini AI — Generate Content  ← AJUSTE PENDIENTE
    System instruction: [pegar AUTOMA-system-prompt-base + brand-bible resumida]
    Prompt: 
    "Basándote en esta noticia de IA: {{1.title}} - {{1.description}}
    
    Generá un guión de reel de 45-60 segundos para @soyautoma.
    Audiencia: emprendedoras latinoamericanas 40-55 años, sin conocimiento técnico.
    
    Estructura obligatoria:
    GANCHO (0-3 seg): [frase que para el scroll]
    PROBLEMA (3-15 seg): [lo que le pasa a la audiencia]
    SOLUCIÓN (15-45 seg): [qué puede hacer con esto]
    CTA (45-60 seg): [una sola acción]
    
    NUNCA usar: potenciar, disruptivo, sinergia, algoritmo, empoderamiento
    Sonar como Lu: cálida, directa, con ejemplos concretos, vos/hacés/podés"

[3] Google Sheets — Add a Row  ← AJUSTE PENDIENTE
    Archivo: AUTOMA CONTROL CENTRAL
    Pestaña: historial-contenido
    Columnas:
    - fecha_generado: {{now}}
    - plataforma: TikTok/IG
    - formato: Reel
    - pilar: 3 (educación práctica)
    - tema: {{1.title}}
    - estado: Borrador
    - link_borrador: (vacío — se completa cuando Lu lo revisa)

[4] Google Sheets — Add a Row  ← AJUSTE PENDIENTE
    Archivo: AUTOMA CONTROL CENTRAL
    Pestaña: ecosystem-log
    Columnas:
    - ID_Tarea: AUTO
    - Fecha_Hora: {{now}}
    - Agente_Descripción: Contenidos · borrador_generado · {{1.title}}
    - Estado: EJECUTADO
    - Enlace_Borrador: (vacío)
    - Logs_Error: (vacío)

[5] Telegram Bot — Send Message (ya existe · mantener)
    Chat ID: [tu chat ID de Telegram]
    Mensaje:
    "📝 BORRADOR LISTO — Agente Contenidos
    
    Tema: {{1.title}}
    Plataforma: TikTok + IG Reel
    
    --- GUIÓN ---
    {{2.result}}
    
    ¿Aprobás este borrador?
    ✅ aprobado | ❌ rechazado: [motivo]"
```

### Feeds RSS recomendados para configurar:
- `https://feeds.feedburner.com/oreilly/radar` (IA en inglés — filtrar con Gemini)
- `https://www.xataka.com/rss` (tecnología en español)
- `https://www.infobae.com/feeds/rss/` (actualidad LATAM)

---

## ESCENARIO 2 — Gmail Router (ya existe · ajustar)

**Nombre:** `AUTOMA - Filtros Gmail Web`  
**Trigger:** Watch New Emails · Gmail inbox · solo no leídos  
**Objetivo:** Capturar leads, clasificarlos, enviar auto-respuesta según perfil, registrar en AUTOMA CONTROL CENTRAL

### Módulos en orden:

```
[1] Gmail — Watch New Emails (ya existe · mantener)
    Carpeta: Inbox
    Filtro: solo no leídos
    Limit: 10

[2] Google Sheets — Add a Row (ya existe · mantener)
    Archivo: Contactos AUTOMA  ← este NO cambiar, sigue funcionando
    Pestaña: Hoja 1
    Columnas: FECHA, NOMBRE, EMAIL, ASUNTO, MENSAJE

[3] Google Sheets — Add a Row  ← MÓDULO NUEVO A AGREGAR
    Archivo: AUTOMA CONTROL CENTRAL
    Pestaña: control_Leads
    Columnas:
    - fecha_entrada: {{1.internalDate}}
    - nombre: {{1.fromName}}
    - email: {{1.fromEmail}}
    - negocio: {{1.snippet}}
    - fuente: Web/Gmail
    - estado_actual: NUEVO
    - score: 10 (punto de partida por completar formulario)
    - categoria: Frío
    - última_interacción: {{now}}

[4] Google Sheets — Add a Row  ← MÓDULO NUEVO A AGREGAR
    Archivo: AUTOMA CONTROL CENTRAL
    Pestaña: ecosystem-log
    Columnas:
    - ID_Tarea: AUTO
    - Fecha_Hora: {{now}}
    - Agente_Descripción: Email · lead_capturado · {{1.fromEmail}}
    - Estado: EJECUTADO
    - Enlace_Borrador: (vacío)
    - Logs_Error: (vacío)

[5] Router (ya existe · mantener)
    5 ramas según perfil detectado en el asunto/mensaje

[6-10] Gmail — Send Email x5 (ya existe · mantener)
    Las 5 auto-respuestas según rama
```

---

## ESCENARIO 3 — Orquestador 9am (crear cuando Make Core)

**Nombre:** `AUTOMA - Orquestador Diario`  
**Trigger:** Schedule · 9:00am · lunes a sábado  
**Objetivo:** Leer estado del ecosistema, actualizar scores de leads, generar reporte diario, notificar a Lu

### Módulos en orden:

```
[1] Schedule — Trigger 9am lunes-sábado

[2] Google Sheets — Search Rows
    Archivo: AUTOMA CONTROL CENTRAL
    Pestaña: control_Leads
    Filtro: estado_actual = NUEVO
    Objetivo: detectar leads sin procesar

[3] Google Sheets — Search Rows
    Archivo: AUTOMA CONTROL CENTRAL
    Pestaña: historial-contenido
    Filtro: estado = Borrador
    Objetivo: detectar borradores esperando OK

[4] Google Sheets — Search Rows
    Archivo: AUTOMA CONTROL CENTRAL
    Pestaña: ecosystem-log
    Filtro: últimas 24hs
    Objetivo: contar acciones ejecutadas

[5] Telegram Bot — Send Message
    Mensaje: reporte diario (ver formato en AUTOMA-prompt-orquestador-v1.0.md)

[6] Google Sheets — Add a Row
    Archivo: AUTOMA CONTROL CENTRAL
    Pestaña: ecosystem-log
    Columnas:
    - Agente_Descripción: Orquestador · reporte_enviado
    - Estado: EJECUTADO
```

---

## Guardarriles de Make

### Configurar en cada escenario:
- **Max errors:** 3 (después de 3 errores consecutivos, el escenario se detiene)
- **Auto commit:** ON (registra ejecuciones aunque haya errores parciales)
- **Incomplete executions:** guardar siempre (para debug)

### Alerta de consumo:
- En Make → Account → Notifications → activar alerta cuando quede 20% de operaciones mensuales

---

## Orden de implementación recomendado

1. ✅ Agregar módulos nuevos al Escenario 2 (Gmail) — sin cambiar lo que funciona
2. ✅ Ajustar Escenario 1 (RSS) — inyectar system prompt en Gemini
3. ⏳ Crear Escenario 3 (Orquestador) — cuando Make Core esté activo
4. ✅ Testear Escenario 2 con 1 email de prueba real
5. ✅ Activar Escenario 1 y verificar primer borrador generado

---

*Versión 1.0 — junio 2026. Actualizar cuando se agregue Make Core o Claude API.*
