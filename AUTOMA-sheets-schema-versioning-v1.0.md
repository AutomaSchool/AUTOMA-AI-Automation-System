# AUTOMA — Google Sheets Schema + Versioning v1.0
> Estructura exacta de cada pestaña de Sheets y sistema de control de versiones de documentos.
> Make necesita estos nombres de columnas exactos para funcionar sin errores.
> Última actualización: junio 2026

---

## Google Sheets — Estructura completa

### Archivo principal: `AUTOMA-Ecosystem-DB`

Todas las pestañas del ecosistema viven en UN solo archivo de Google Sheets.
URL: *(completar con el link real del archivo)*

---

### Pestaña 1: `ecosystem-log`
> Log de todas las acciones ejecutadas por el ecosistema. Make escribe aquí después de cada acción.

| Columna | Tipo | Descripción | Ejemplo |
|---------|------|-------------|---------|
| `timestamp` | Fecha/hora | Cuándo ocurrió | 16/06/2026 09:03 |
| `agente` | Texto | Qué agente ejecutó | Orquestador / Contenidos / Email / PM |
| `accion` | Texto | Qué hizo | borrador_generado / lead_clasificado / email_enviado |
| `detalle` | Texto | Descripción breve | "Borrador reel pilar 3 generado para semana 25" |
| `estado` | Texto | Resultado | EJECUTADO / FALLO / PENDIENTE / CANCELADO |
| `requiere_ok` | Texto | Si necesita aprobación de Lu | SÍ / NO |
| `link_recurso` | URL | Link al borrador o documento | https://drive.google.com/... |
| `resuelto` | Texto | Si fue atendido por Lu | SÍ / NO / N/A |

---

### Pestaña 2: `control-leads`
> Base de datos de todos los contactos capturados. Make y Lu escriben aquí.

| Columna | Tipo | Descripción | Ejemplo |
|---------|------|-------------|---------|
| `id_lead` | Número | ID único autogenerado | 001 |
| `fecha_entrada` | Fecha | Cuándo llegó | 16/06/2026 |
| `nombre` | Texto | Nombre del contacto | María González |
| `email` | Email | Email del contacto | maria@gmail.com |
| `negocio` | Texto | Tipo de negocio | Peluquería / Docente / Sin negocio |
| `perfil` | Texto | Categoría de audiencia | Emprendedora / Docente / Dueña negocio / Profesional |
| `fuente` | Texto | De dónde vino | Instagram / TikTok / Web / Referido |
| `descargó_pdf` | Texto | Descargó el lead magnet | SÍ / NO |
| `respondió_email` | Texto | Respondió algún email | SÍ / NO |
| `consultó_precio` | Texto | Preguntó por precio | SÍ / NO |
| `agendó_consulta` | Texto | Agendó consulta gratuita | SÍ / NO |
| `escribió_dm` | Texto | Escribió por DM espontáneamente | SÍ / NO |
| `score` | Número | Puntaje total | 0 a 100 |
| `categoria` | Texto | Clasificación actual | Caliente / Tibio / Frío / Inactivo |
| `días_sin_contacto` | Número | Calculado por Make | 5 |
| `última_interacción` | Fecha | Fecha de última acción | 10/06/2026 |
| `estado_actual` | Texto | Estado en el funnel | NUEVO / EN_PROCESO / RESPONDIDO / CONVERTIDO / INACTIVO |
| `rama_autorespuesta` | Texto | Qué respuesta automática recibió | Emprendedora / Docente / Negocio / Profesional / General |
| `notas_lu` | Texto | Observaciones manuales de Lu | "Muy interesada, espera cobro" |
| `convertido` | Texto | Si compró | SÍ / NO |
| `fecha_conversión` | Fecha | Cuándo compró | — |

---

### Pestaña 3: `memoria-de-marca`
> Registro vivo de correcciones y reglas de tono que aprende el ecosistema.

| Columna | Tipo | Descripción | Ejemplo |
|---------|------|-------------|---------|
| `fecha` | Fecha | Cuándo se registró | 16/06/2026 |
| `tipo_contenido` | Texto | Qué tipo de pieza fue | Reel / Carrusel / Email / Caption |
| `que_se_hizo_mal` | Texto | Error detectado | "Usó 'potenciar' en el caption" |
| `regla_nueva` | Texto | Regla que se agrega | "Nunca usar 'potenciar' — reemplazar por 'mejorar', 'hacer crecer', 'hacer más'" |
| `impacto` | Texto | Aplica a qué agentes | Todos / Solo Contenidos / Solo Email |
| `activa` | Texto | Si sigue vigente | SÍ / NO |

---

### Pestaña 4: `historial-contenido`
> Todo el contenido generado, aprobado y publicado.

| Columna | Tipo | Descripción | Ejemplo |
|---------|------|-------------|---------|
| `id_contenido` | Texto | ID único | C001 |
| `semana` | Número | Semana del año | 25 |
| `fecha_generado` | Fecha | Cuándo lo generó el agente | 16/06/2026 |
| `plataforma` | Texto | Para dónde es | IG / TT / FB / YT / Email |
| `formato` | Texto | Tipo de pieza | Reel / Carrusel / Video / Caption |
| `pilar` | Número | Pilar de contenido | 1 / 2 / 3 / 4 / 5 |
| `tema` | Texto | Tema principal | "3 herramientas gratuitas de IA" |
| `estado` | Texto | En qué paso está | Borrador / Revisión / Aprobado / Publicado / Rechazado |
| `link_borrador` | URL | Link al borrador en Drive | https://drive.google.com/... |
| `link_publicado` | URL | Link al post publicado | https://www.instagram.com/... |
| `fecha_publicado` | Fecha | Cuándo se publicó | — |
| `motivo_rechazo` | Texto | Si fue rechazado | — |

---

### Pestaña 5: `ideas-contenido`
> Banco de ideas para el Agente de Contenidos.

| Columna | Tipo | Descripción | Ejemplo |
|---------|------|-------------|---------|
| `fecha` | Fecha | Cuándo surgió | 16/06/2026 |
| `idea` | Texto | La idea en una línea | "Video sobre cómo usar Claude para responder emails" |
| `pilar` | Número | Pilar correspondiente | 3 |
| `formato` | Texto | Formato sugerido | Reel |
| `fuente` | Texto | De dónde salió la idea | DM / RSS / Lu / Tendencia |
| `estado` | Texto | Estado | Pendiente / En producción / Publicado / Descartado |
| `semana_asignada` | Número | Semana en que se usará | — |

---

### Pestaña 6: `metricas-redes`
> Métricas de redes sociales — carga manual cada 15 días.

| Columna | Tipo | Descripción |
|---------|------|-------------|
| `fecha_carga` | Fecha | Cuándo se cargaron los datos |
| `plataforma` | Texto | IG / TT / FB / YT |
| `período` | Texto | "01/06 al 15/06" |
| `seguidores_total` | Número | Total acumulado |
| `seguidores_nuevos` | Número | Ganados en el período |
| `alcance` | Número | Personas únicas alcanzadas |
| `impresiones` | Número | Total de impresiones |
| `interacciones` | Número | Likes + comentarios + guardados |
| `pieza_top` | Texto | La pieza con más alcance |
| `notas` | Texto | Observaciones de Lu |

---

## Sistema de Versioning de Documentos

### Problema que resuelve
El archivo `AUTOMA-system-prompt-base (3).md` tiene un `(3)` porque hay 3 versiones. Sin sistema de control, no se sabe cuál es la oficial.

### Convención de nombres

**Formato:** `AUTOMA-[nombre]-v[número].[número].md`

| Documento | Nombre oficial |
|-----------|---------------|
| Ecosystem Spec | `AUTOMA-ecosystem-spec-v1.0.md` |
| Brand Bible | `AUTOMA-brand-bible-v1.0.md` |
| System Prompt Base | `AUTOMA-system-prompt-base-v1.0.md` |
| Base de Conocimiento | `AUTOMA-base-conocimiento-v1.0.md` |
| Protocolo de Contingencia | `AUTOMA-protocolo-contingencia-v1.0.md` |
| Lead Scoring | `AUTOMA-lead-scoring-v1.0.md` |
| Calendario Editorial | `AUTOMA-calendario-editorial-v1.0.md` |
| Sheets Schema | `AUTOMA-sheets-schema-v1.0.md` |

### Reglas de versioning

- **v1.0 → v1.1**: cambio menor (corrección de un dato, agregado de una regla)
- **v1.0 → v2.0**: cambio estructural (rediseño de un agente, nueva oferta)
- Solo existe UNA versión activa de cada documento
- Las versiones anteriores se archivan en GitHub carpeta `/archivo/`
- Nunca borrar versiones anteriores — solo archivar

### Versión oficial actual de cada documento

| Documento | Versión activa | Última actualización |
|-----------|---------------|----------------------|
| ecosystem-spec | v1.0 | junio 2026 |
| brand-bible | v1.0 | junio 2026 |
| system-prompt-base | v1.0 | junio 2026 |
| base-conocimiento | v1.0 | junio 2026 — INCOMPLETA (pendientes confirmación de Lu) |
| protocolo-contingencia | v1.0 | junio 2026 |
| lead-scoring | v1.0 | junio 2026 |
| calendario-editorial | v1.0 | junio 2026 |
| sheets-schema | v1.0 | junio 2026 |

---

## Estructura de carpetas en GitHub

```
AutomaSchool/Automawebacademy/
│
├── /specs/                    ← todos los documentos de este sistema
│   ├── AUTOMA-ecosystem-spec-v1.0.md
│   ├── AUTOMA-brand-bible-v1.0.md
│   ├── AUTOMA-system-prompt-base-v1.0.md
│   ├── AUTOMA-base-conocimiento-v1.0.md
│   ├── AUTOMA-protocolo-contingencia-v1.0.md
│   ├── AUTOMA-lead-scoring-v1.0.md
│   ├── AUTOMA-calendario-editorial-v1.0.md
│   └── AUTOMA-sheets-schema-v1.0.md
│
├── /archivo/                  ← versiones anteriores archivadas
│
├── /borradores/               ← contenido generado por agentes pendiente de aprobación
│   ├── /reels/
│   ├── /carruseles/
│   ├── /emails/
│   └── /youtube/
│
└── README.md                  ← índice del ecosistema
```

---

*Versión 1.0 — junio 2026.*
