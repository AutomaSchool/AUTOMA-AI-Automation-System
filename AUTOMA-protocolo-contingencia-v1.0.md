# AUTOMA — Protocolo de Contingencia v1.0
> Qué hace el ecosistema cuando algo falla. Cada escenario tiene un dueño, un tiempo máximo y una acción concreta.
> Última actualización: junio 2026

---

## Principio base

**El ecosistema nunca falla en silencio.** Todo fallo genera una alerta. Si no puede alertar, registra. Si no puede registrar, para.

---

## Nivel de severidad

| Nivel | Qué significa | Tiempo máximo de respuesta |
|-------|---------------|---------------------------|
| 🔴 CRÍTICO | El ecosistema está detenido o hay riesgo de envío incorrecto | Alerta inmediata a Lu |
| 🟡 MODERADO | Un agente falló pero el resto sigue funcionando | Alerta en el resumen de las 9am |
| 🟢 LEVE | Algo no se completó, sin impacto visible | Log en Sheets, sin alerta |

---

## Escenarios de fallo y protocolo exacto

### Escenario 1 — Make no responde / timeout

**Síntoma:** El escenario de Make se activa pero no completa la ejecución.

**Protocolo:**
1. Make reintenta automáticamente 3 veces con 5 minutos entre intentos
2. Si los 3 reintentos fallan → registrar en `ecosystem-log` con estado `FALLO` + timestamp + nombre del escenario
3. Enviar mensaje a Telegram (Tema: #Alertas):
```
🔴 ALERTA CRÍTICA — Make
Escenario: [nombre del escenario]
Hora del fallo: [timestamp]
Intentos realizados: 3
Estado: detenido
Acción requerida: revisar Make manualmente
```
4. **NO reintentar más.** Esperar acción de Lu.
5. Si el fallo afecta a un lead nuevo: ese lead queda en estado `Pendiente` en Sheets hasta que Lu resuelva.

**Impacto en leads:** Ningún lead queda sin respuesta por más de 24 horas. Si Make no se recuperó en 24hs, Lu responde manualmente desde Gmail.

---

### Escenario 2 — Claude no responde / API error

**Síntoma:** Make llama a Claude para generar contenido o redactar y no recibe respuesta.

**Protocolo:**
1. Esperar 10 minutos y reintentar una vez
2. Si sigue sin responder → registrar en `ecosystem-log` como `CLAUDE_FALLO`
3. La tarea queda en estado `Pendiente` en la pestaña correspondiente de Sheets
4. Enviar alerta a Telegram (Tema: #Alertas):
```
🟡 ALERTA MODERADA — Claude
Tarea pendiente: [tipo de tarea]
Estado: esperando intervención
Acción: podés generar el contenido manualmente o reintentar más tarde
```
5. **El resto del ecosistema sigue funcionando.** Este fallo no detiene los otros agentes.

---

### Escenario 3 — Google Sheets no responde

**Síntoma:** Make intenta leer o escribir en Sheets y recibe error de conexión.

**Protocolo:**
1. Reintentar 2 veces con 3 minutos entre intentos
2. Si falla: **detener el flujo completo** (no ejecutar ninguna acción sin poder registrarla)
3. Registrar el intento fallido en un Google Form de backup (configurar un Form simple como respaldo de logs)
4. Enviar alerta a Telegram:
```
🔴 ALERTA CRÍTICA — Google Sheets
El log central no está disponible
Estado: ecosistema en pausa
Acción: verificar Google Drive y reiniciar Make manualmente
```

---

### Escenario 4 — Telegram bot no envía mensajes

**Síntoma:** Make intenta notificar a Lu pero el bot no responde.

**Protocolo:**
1. Reintentar 2 veces
2. Si falla: registrar en Sheets el mensaje no entregado con estado `SIN_NOTIFICAR`
3. El flujo continúa pero NO ejecuta acciones que requieran OK de Lu
4. Lu ve el log pendiente la próxima vez que abre Sheets manualmente

**Regla crítica:** Si el canal de comunicación con Lu está caído, el ecosistema entra en **modo conservador** — solo ejecuta acciones autónomas de bajo riesgo (clasificar, redactar borradores). Nada que requiera aprobación.

---

### Escenario 5 — Contenido generado con tono incorrecto

**Síntoma:** Lu rechaza un borrador porque no suena a ella.

**Protocolo:**
1. Lu envía rechazo por Telegram con motivo: `❌ rechazado: [motivo específico]`
2. El Orquestador extrae la regla subyacente del rechazo
3. La guarda en Sheets → pestaña `memoria-de-marca` con formato:
```
Fecha | Tipo de contenido | Qué se hizo mal | Regla nueva
```
4. El Agente Contenidos lee `memoria-de-marca` como input obligatorio en su próximo ciclo
5. Genera nueva versión del borrador aplicando la corrección
6. Si el mismo error se repite 3 veces → alerta a Lu de que el prompt base necesita actualización manual

---

### Escenario 6 — Lead nuevo sin respuesta automática

**Síntoma:** Llega un lead al formulario pero el flujo de Make no lo procesa.

**Protocolo:**
1. Sheets registra el lead con estado `NUEVO` automáticamente (el formulario escribe directo en Sheets)
2. Si en 15 minutos el estado sigue en `NUEVO` sin cambiar a `EN_PROCESO` → alerta a Telegram
3. Lu revisa manualmente y responde desde Gmail usando las plantillas de respuesta aprobadas
4. Registra el estado como `RESPONDIDO_MANUAL` en Sheets
5. **Máximo tiempo sin respuesta a un lead nuevo: 24 horas.**

---

### Escenario 7 — Lu no responde una aprobación en 48 horas

**Síntoma:** Un borrador está esperando OK de Lu y no hay respuesta.

**Protocolo:**
1. A las 24 horas: recordatorio en Telegram (Tema: #Aprobaciones)
```
⏰ RECORDATORIO — Borrador esperando tu OK
Borrador: [tipo] generado el [fecha]
Link: [link al borrador]
¿Aprobás o necesitás cambios?
```
2. A las 48 horas: segundo recordatorio
3. A las 72 horas: el borrador pasa a estado `VENCIDO` en Sheets y se archiva
4. El agente genera un nuevo borrador la próxima semana con el mismo tema

---

## Estados del ecosistema

| Estado | Significado | Qué puede hacer el ecosistema |
|--------|-------------|-------------------------------|
| ✅ OPERATIVO | Todo funciona | Todo |
| 🟡 DEGRADADO | Un componente falló | Solo acciones autónomas de bajo riesgo |
| 🔴 PAUSA | Sheets o Telegram caídos | Solo registrar, no ejecutar |
| ⛔ DETENIDO | Fallo crítico múltiple | Nada — esperar intervención de Lu |

---

## Checklist de salud semanal (cada lunes 9am)

El Orquestador verifica y reporta:
- [ ] Make: todos los escenarios activos sin errores en los últimos 7 días
- [ ] Sheets: todas las pestañas accesibles y con datos recientes
- [ ] Telegram: bot activo y mensajes entregados
- [ ] Claude: último uso exitoso
- [ ] Leads: ninguno con estado `NUEVO` por más de 24 horas

Si algún ítem falla → incluir en el reporte del lunes con estado y acción recomendada.

---

*Este protocolo se revisa cada vez que aparece un escenario de fallo nuevo. Versión 1.0 — junio 2026.*
