# AUTOMA — Lead Scoring y Definición de Lead Caliente v1.0
> Criterios exactos y medibles para que Make pueda clasificar leads automáticamente.
> Sin ambigüedad — cada criterio es un dato que existe en Sheets o que Make puede verificar.
> Última actualización: junio 2026

---

## El problema que resuelve este documento

La spec original decía "lead caliente = abrió 3+ emails y visitó página de ventas" — pero Gmail básico no trackea aperturas ni visitas. Este documento define criterios que SÍ son medibles con el stack actual (Make + Google Sheets + Gmail).

---

## Stack de tracking disponible

| Herramienta | Qué puede medir |
|-------------|-----------------|
| Google Sheets | Fecha de entrada, datos del formulario, historial de interacciones manuales |
| Make | Tiempo transcurrido, cambios de estado, triggers por evento |
| Gmail | Envíos (no aperturas — Gmail básico no trackea opens) |
| Formulario web | Qué campos completó, desde qué página llegó |
| Telegram | Respuestas de Lu, aprobaciones, marcas manuales |

**Realidad técnica:** Sin una herramienta de email marketing con tracking (Mailchimp, Brevo, etc.), las aperturas de email NO son medibles automáticamente. El scoring MVP usa criterios alternativos medibles.

---

## Sistema de puntos MVP (medible con stack actual)

Cada lead acumula puntos según sus acciones. Make actualiza la columna `score` en Sheets automáticamente.

| Acción | Puntos | Cómo Make lo detecta |
|--------|--------|----------------------|
| Completó el formulario con nombre + email + descripción de negocio | +10 | Formulario web → Sheets (ya conectado) |
| Descargó el PDF gratuito | +5 | Columna `descargó_pdf` = SÍ en Sheets |
| Respondió un email de Lu | +15 | Lu marca manualmente en Sheets (columna `respondió`) |
| Preguntó precio o más información | +20 | Lu marca manualmente (columna `consultó_precio`) |
| Agendó una consulta gratuita | +30 | Marca manual o webhook de plataforma de agenda |
| Escribió por Instagram/DM espontáneamente | +20 | Lu marca manualmente |
| Lleva más de 30 días sin interacción | -10 | Make calcula automáticamente por fecha |

---

## Categorías de lead

| Categoría | Puntaje | Color en Sheets | Acción del ecosistema |
|-----------|---------|-----------------|----------------------|
| 🔴 Caliente | 35+ puntos | Rojo | Alerta inmediata a Lu por Telegram |
| 🟡 Tibio | 15-34 puntos | Amarillo | Entra en secuencia de nurturing |
| ⚪ Frío | 5-14 puntos | Gris | Recibe emails de la secuencia base |
| 💀 Inactivo | 0-4 puntos o +45 días sin movimiento | Negro | Email de re-engagement o baja limpia |

---

## Schema exacto de la columna de score en Sheets

Pestaña: `control-leads`

| Columna | Tipo | Valores posibles |
|---------|------|-----------------|
| `score` | Número | 0 a 100 |
| `categoria` | Texto | Caliente / Tibio / Frío / Inactivo |
| `descargó_pdf` | Texto | SÍ / NO |
| `respondió_email` | Texto | SÍ / NO |
| `consultó_precio` | Texto | SÍ / NO |
| `agendó_consulta` | Texto | SÍ / NO |
| `escribió_dm` | Texto | SÍ / NO |
| `días_sin_contacto` | Número | Calculado automático por Make |
| `última_interacción` | Fecha | DD/MM/AAAA |
| `estado_actual` | Texto | NUEVO / EN_PROCESO / RESPONDIDO / CONVERTIDO / INACTIVO |

---

## Cómo Make actualiza el score

### Flujo automático (diario a las 9am junto con el Orquestador)

```
TRIGGER: Horario → 9am lunes a sábado
↓
Make lee todas las filas de control-leads
↓
Para cada lead:
  - Calcula días_sin_contacto = hoy - fecha última_interacción
  - Si días_sin_contacto > 30 → resta 10 puntos al score
  - Actualiza columna categoría según tabla de puntajes
  - Si categoría cambió a "Caliente" → envía alerta a Telegram
↓
Registra en ecosystem-log: "Score actualizado - [fecha] - [N leads procesados]"
```

### Flujo manual de Lu (cuando marca interacciones)

```
Lu actualiza SÍ en columna respondió_email / consultó_precio / etc.
↓
Make detecta el cambio (webhook en Sheets o revisión en el ciclo de 9am)
↓
Recalcula score sumando los puntos correspondientes
↓
Si el nuevo score cruza umbral de "Caliente" → alerta inmediata a Telegram
```

---

## Alerta de lead caliente (formato exacto)

Cuando un lead llega a 35+ puntos, el Orquestador envía esto a Telegram (Tema: #Alertas):

```
🔴 LEAD CALIENTE — acción recomendada

Nombre: [nombre del lead]
Email: [email]
Negocio: [descripción que completó en el formulario]
Score: [puntos] / 100
Por qué está caliente: [lista las acciones que sumó puntos]

Última interacción: [fecha]
Días en la lista: [número]

ACCIÓN SUGERIDA: Escribirle personalmente en las próximas 2 horas.
Borrador de mensaje: [link al borrador en Drive]
```

---

## Escalación futura (cuando tengás una herramienta de email con tracking)

Cuando migres a Brevo, Mailchimp u otra plataforma con tracking real, podés agregar:

| Acción | Puntos adicionales |
|--------|-------------------|
| Abrió email 1 vez | +3 |
| Abrió email 3+ veces | +8 |
| Hizo clic en link del email | +10 |
| Visitó página de ventas | +15 |
| Abandonó carrito / formulario a mitad | +25 |

Esto duplica la precisión del scoring sin cambiar la estructura de Sheets — solo se agregan columnas nuevas.

---

*Versión 1.0 — junio 2026. Revisar cuando se incorpore herramienta de email marketing con tracking.*
