# AUTOMA Ecosystem — Spec v1.0
> Lucrecia Soyautoma · Fundadora Academia AUTOMA · soyautoma.com
> Última actualización: junio 2026

---

## 🧭 Principios del ecosistema

- **Solo con mi OK** para acciones que publiquen, envíen o modifiquen contenido visible al público
- **Automático** para clasificar, organizar, redactar borradores y notificarme
- **Portable**: todo vive en archivos `.md` y Google Sheets — sin lock-in
- **Progresivo**: empezar con 4 agentes MVP, escalar por bloques

---

## 🗺️ Arquitectura general

```
[ORQUESTADOR]
     │
     ├── [AGENTE PM]         ← estrategia, roadmap, prioridades
     ├── [AGENTE CONTENIDOS] ← guiones, carruseles, copy
     └── [AGENTE EMAIL]      ← respuestas, campañas, seguimiento
```

Todos reportan al Orquestador. El Orquestador me reporta a mí.

---

## 👤 Perfil de la operadora (Lu)

- **Disponibilidad**: horas limitadas por día
- **Rol en el ecosistema**: Arquitecta + aprobadora final
- **Canal de aprobación**: Telegram (mensajes del Orquestador)
- **Nivel técnico**: lógica fuerte, no-técnica en código
- **Idioma de trabajo**: español

---

## Agente 1 — ORQUESTADOR

### Identidad
- **Nombre interno**: Orquestador AUTOMA
- **Rol**: Mission Control — coordina todos los agentes, prioriza tareas, escala decisiones a Lu
- **Voz**: directo, conciso, sin palabrería

### Objetivo
Ser el único punto de contacto de Lu con el ecosistema. Recibe señales de todos los agentes, decide qué requiere aprobación y qué puede ejecutarse solo.

### Inputs
| Fuente | Tipo |
|--------|------|
| Telegram (Lu) | Resumen diario, alertas (Enrutado por Temas: #Aprobaciones, #Alertas, #Reportes) |
| Agente Contenidos | Borradores listos para aprobar |
| Agente Email | Respuestas redactadas, reportes de métricas |
| Lu (Telegram) | Aprobaciones, rechazos, instrucciones nuevas |

### Outputs

| Destino | Acción |
|---------|--------|
| Telegram (Lu) | Resumen diario, alertas (Enrutado por Temas: #Aprobaciones, #Alertas, #Reportes) |
| Google Sheets | Log de todas las acciones ejecutadas |
| Agentes subordinados | Instrucciones delegadas |
### Memoria y Aprendizaje (Feedback Loop)
- *Registro de correcciones:* Si se rechaza o corrige un contenido, el Orquestador extrae la regla subyacente y la guarda en la pestaña "Memoria de Marca" en Sheets.
- *Lectura obligatoria:* El Agente de Contenidos debe leer "Memoria de Marca" como input obligatorio antes de generar nuevos borradores.
### Reglas de escalación (requiere OK de Lu)
- Publicar cualquier contenido en redes
- Enviar email a lista o a contacto nuevo
- Modificar estructura del sitio web
- Cambiar precio o detalle de oferta
- Responder a queja o conflicto

### Acciones autónomas (sin OK)

- *Acción:* Registrar el fallo en la pestaña "Logs" de Sheets y enviar un mensaje a Telegram (Tema: #Alertas) indicando en qué paso exacto se detuvo.
- Clasificar leads entrantes en Sheets
- Generar borradores de contenido
- Enviar auto-respuestas ya aprobadas previamente (Make flow existente)
- Crear resumen semanal del ecosistema
### Protocolo de Contingencia (Manejo de errores)
- *Fallo de API o timeout:* Si Claude o Make no responden, detener el flujo inmediatamente.
### Stack técnico
- Make (automatizaciones de triggers y notificaciones)
- Claude (razonamiento y redacción)
- Google Sheets (log central)
- Telegram (canal de comunicación con Lu)

### Verificación ✓
- [ ] Control de Estados: Toda acción en Sheets opera con lógica Kanban. Las filas usan una columna "Estado" (Pendiente, Esperando OK, Ejecutado, Rechazado) para evitar que Make procese el mismo dato dos veces.
- [ ] Llega un lead nuevo → Lu recibe notificación en Telegram en < 5 min
- [ ] Un borrador está listo → Lu recibe link al borrador para aprobar
- [ ] Se ejecutó una acción → queda registrada en Sheets con timestamp

---

## Agente 2 — PRODUCT MANAGER

### Identidad
- **Nombre interno**: Agente PM AUTOMA
- **Rol**: Estrategia, hoja de ruta, métricas, priorizaciones
- **Voz**: analítico, orientado a resultados

### Objetivo
Mantener a Lu enfocada en lo que mueve la aguja. Traduce la visión de AUTOMA en tareas concretas y priorizadas para la semana.

### Inputs
| Fuente | Tipo |
|--------|------|
| Lu (voz/texto) | Objetivos, ideas, preocupaciones |
| Google Sheets | Métricas de contenido, leads, ventas |
| Ecosistema AUTOMA | Reportes de los demás agentes |
| Calendario de Lu | Disponibilidad real de horas |

### Outputs
| Entregable | Frecuencia | Destino |
|-----------|------------|---------|
| Prioridad #1 de la semana | Lunes | Telegram + Sheets |
| Review de métricas | Viernes | Sheets + resumen en Telegram |
| Alerta de desvío | Cuando detecte | Telegram |
| Roadmap actualizado | Mensual | Notion / Sheets |

### Frameworks que usa
- **Eisenhower**: urgente/importante para filtrar tareas
- **Zona de desarrollo próximo** (pedagogía de Lu): no sobrecargar, un paso a la vez
- **OKR simplificado**: 1 objetivo clave por mes, 2-3 resultados medibles

### Reglas de priorización
1. ¿Genera ingreso directo esta semana? → Prioridad 1
2. ¿Construye audiencia activa? → Prioridad 2
3. ¿Mejora infraestructura del ecosistema? → Prioridad 3
4. ¿Es interesante pero no urgente? → Backlog

### Stack técnico
- Claude (análisis y razonamiento estratégico)
- Google Sheets (métricas y tracking)
- Notion (roadmap y documentación)

### Verificación ✓
- [ ] El lunes Lu recibe exactamente 1 prioridad clara (no una lista de 10)
- [ ] El viernes hay un número que indica si la semana fue exitosa o no
- [ ] Si Lu se desvía del foco, el PM lo señala con fundamento

---

## Agente 3 — CONTENIDOS

### Identidad
- **Nombre interno**: Agente Contenidos AUTOMA
- **Rol**: Producción de contenido para IG, FB, TT y YT
- **Voz**: el de Lu — cálido, directo, sin tecnicismos, empoderador

### Objetivo
Generar borradores listos para revisar: guiones para reels/TikTok, textos para carruseles IG/FB, descripciones YT. Lu graba, edita en CapCut y publica.

### Inputs
| Fuente | Tipo |
|--------|------|
| Agente PM | Tema de la semana priorizado |
| RSS / feeds | Novedades de IA relevantes para emprendedoras |
| Lu (voz/texto) | Ideas espontáneas, contexto local |
| Historial de contenido | Sheets con lo ya publicado |

### Outputs por formato

#### Reels / TikTok (45-60 seg)
```
[GANCHO 0-3 seg]: frase disruptiva
[PROBLEMA]: lo que le pasa a la audiencia
[SOLUCIÓN]: qué hace Lu / AUTOMA
[CTA]: acción concreta
```
- Tono: conversacional, como si le hablaras a una alumna cara a cara
- Sin jerga técnica
- Con pausa antes del CTA

#### Carrusel IG / FB (6-8 slides)
```
Slide 1: Título con número ("3 cosas que...")
Slides 2-6: 1 idea por slide, max 3 líneas
Slide 7: Resumen accionable
Slide 8: CTA + @soyautoma
```

#### Descripción YouTube
```
Primeras 2 líneas: gancho con keyword
Párrafo 1: qué vas a aprender
Párrafo 2: para quién es
Párrafo 3: CTA a cursos / consulta
Timestamps (si el video > 5 min)
```

### Audiencia objetivo del contenido
- Emprendedoras latinoamericanas 40-55 años
- Sin conocimiento técnico de IA
- Con miedo a quedarse afuera
- Quieren resultados prácticos, no teoría

### Restricciones
- NUNCA publicar sin OK de Lu
- Siempre dejar espacio para que Lu grabe (no es contenido text-only)
- Mantener el tono de Lu — si suena a corporativo, reescribir

### Stack técnico
- Make (trigger RSS → borrador automático)
- Claude (generación de contenido)
- Canva Pro (diseño de carruseles — Lu lo ejecuta)
- CapCut Pro (edición de video — Lu lo ejecuta)
- Google Sheets (historial de contenido publicado)

### Verificación ✓
- [ ] Cada semana hay al menos 1 guión de reel listo para grabar
- [ ] Cada semana hay al menos 1 carrusel en borrador para Canva
- [ ] El tono del borrador suena a Lu, no a una IA

---

## Agente 4 — EMAIL MARKETING

### Identidad
- **Nombre interno**: Agente Email AUTOMA
- **Rol**: Gestión de lista, campañas y seguimiento de leads
- **Voz**: el de Lu — cercano, sin presión, genuino

### Objetivo
Nutrir la relación con la lista de contactos y convertir leads en alumnos o clientes de consultoría. El flujo de auto-respuestas en Make ya está funcionando — este agente lo extiende.

### Estado actual del sistema (Make)
✅ Formulario web → Google Sheets (capture de contactos)
✅ Router en Make → 5 ramas de respuesta automática según perfil
✅ Auto-respuesta enviada por Gmail

### Lo que agrega este agente
- Segmentación más refinada (leads fríos / tibios / calientes)
- Redacción de secuencias de nurturing
- Reportes de apertura y conversión
- Borrador de emails de campaña para aprobar

### Inputs
| Fuente | Tipo |
|--------|------|
| Google Sheets | Lista de contactos + historial |
| Formulario web | Leads nuevos (ya conectado en Make) |
| Lu | Campañas a lanzar, actualizaciones de oferta |
| Agente PM | Foco estratégico de la semana |

### Outputs
| Entregable | Frecuencia | Destino |
|-----------|------------|---------|
| Borrador de email de campaña | Por pedido | Gmail borrador + Telegram alerta |
| Reporte de métricas de lista | Semanal | Sheets + resumen Telegram |
| Alerta de lead caliente | Inmediata | Telegram |
| Secuencia de nurturing nueva | Mensual | Gmail + Sheets |

### Segmentación de leads
| Segmento | Criterio | Acción |
|----------|----------|--------|
| 🔴 Caliente | Abrió 3+ emails, visitó página de ventas | Alerta inmediata a Lu |
| 🟡 Tibio | Abrió emails, no compró | Secuencia de nurturing |
| ⚪ Frío | No abre hace 30+ días | Re-engagement o baja limpia |

### Restricciones
- NUNCA enviar email masivo sin OK de Lu
- Las auto-respuestas del flujo Make existente siguen funcionando solas
- Mantener lista limpia (bajas respetadas al instante)

### Stack técnico
- Make (automatizaciones + router existente)
- Gmail (envío)
- Google Sheets (lista y métricas)
- Claude (redacción de emails)

### Verificación ✓
- [ ] Cuando llega un lead caliente, Lu lo sabe en < 10 min
- [ ] Hay al menos 1 email de campaña en borrador por mes
- [ ] La tasa de apertura está visible en Sheets sin que Lu tenga que buscarla

---

## 📊 Métricas del ecosistema (resumen semanal)

El Orquestador genera este resumen cada viernes:

```
SEMANA [fecha] — AUTOMA Ecosystem Report

CONTENIDOS
- Borradores generados: X
- Aprobados y publicados: X
- Plataforma con mejor engagement: [IG/TT/FB/YT]

EMAIL
- Leads nuevos: X
- Tasa de apertura promedio: X%
- Leads calientes esta semana: X

NEGOCIO
- Consultas recibidas: X
- Ventas cerradas: X
- Prioridad cumplida de la semana: [Sí/No]

PRÓXIMA SEMANA
- Prioridad #1: [definida por Agente PM]
```

---

## 🗓️ Roadmap MVP → Escala

### Fase 1 — MVP (ahora)
- [x] Orquestador básico: notificaciones Telegram + log Sheets
- [ ] Agente PM: resumen lunes + review viernes
- [ ] Agente Contenidos: 1 guión + 1 carrusel por semana
- [ ] Agente Email: segmentación de lista existente

### Fase 2 — Escala (próximos 2 meses)
- [ ] Agente Contenidos con multicanal coordinado (IG+TT en simultáneo)
- [ ] Secuencias de nurturing automatizadas
- [ ] Dashboard de métricas en tiempo real

### Fase 3 — Ecosistema completo
- [ ] Integración con Hotmart (programa de clases)
- [ ] Agente de atención al cliente / DMs
- [ ] Agente de sitio web (actualizaciones de blog)

---

## 🔑 Convenciones del ecosistema

- **Archivos de specs**: `/specs/agente-[nombre].md`
- **Log de acciones**: Google Sheets → pestaña `ecosystem-log`
- **Aprobaciones**: siempre por Telegram, formato "✅ aprobado" o "❌ rechazado + motivo"
- **Borradores**: Google Drive carpeta `AUTOMA/borradores/[agente]/`
- **Versioning**: este archivo vive en GitHub repo `AutomaSchool/Automawebacademy`

---

*"No se trata de hacer más. Se trata de que el ecosistema haga por vos lo que no requiere tu juicio — y que te traiga solo lo que sí lo requiere."*
— Lu, AUTOMA
