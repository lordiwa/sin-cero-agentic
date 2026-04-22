# Procesos de servicio

> Cómo Sin Cero toma pedidos, entrega y atiende al cliente, desde la toma del pedido hasta la postventa.

## Agentes que lo consumen
- `servicio-cliente` (para ejecutar el flujo en cada interacción)
- `dev-sin-cero` (para automatizar el lado digital)

## Vocabulario clave
- **Pedido**: una enjarrada solicitada (individual, desde pack, o sugerencia).
- **Pack**: crédito prepagado de 20 almuerzos, con código tipo `nombre-1234`.
- **Slot de entrega**: ventana horaria acordada con el cliente (TODO formalizar zonas y horarios).
- **Canal**: WhatsApp (principal), app web (sin-cero.com), Instagram DM (secundario).

## Canales y su rol (verificado en app)

1. **Web app (sin-cero.com)** — formulario interactivo para construir la enjarrada. Al finalizar, genera un mensaje de WhatsApp formateado al +593 98 464 5737.
2. **WhatsApp directo** — cliente recurrente que ya conoce el menú o tiene un pack; pide sin pasar por la web.
3. **Instagram @sincero / Facebook Sin-Cero** — awareness y contacto inicial, deriva a WhatsApp para cerrar el pedido.
4. **Email (info@sin-cero.com)** — notificaciones automáticas via EmailJS, B2B, consultas formales.

## Flujo de toma de pedido (verificado en app — flujo web)

El usuario recorre estos pasos (web mobile):

1. **Verduras** — elegir 6 incluidas, extras +$0.30 c/u (máximo 8 total).
2. **Proteínas** — 1 incluida, extras +$1.80 c/u (máximo 2).
3. **Carbohidratos** — 1 incluido, extras +$0.90 (máximo 4).
4. **Carbohidratos especiales** — opcionales, +$0.90 c/u (máximo 4).
5. **Fibras** — opcionales, precios individuales ($0.40-$0.60, máximo 2).
6. **Aderezos** — 1 incluido, extras +$0.90 c/u (máximo 4).
7. **Fruta** — opcional, 1 sin costo.

Luego: datos del cliente (nombre, teléfono, dirección) → click "Enviar por WhatsApp" → se abre `wa.me/593984645737` con el mensaje formateado (ver `templates-mensajes.md` para el template exacto).

## Horarios canónicos (verificado en copy de la app)

- **Entregas:** Lunes a Viernes, **12:00 PM - 15:00 PM**.
- **Cutoff de pedidos:** hasta las **21:30** del día anterior a la entrega.
- **Delivery:** costo variable según zona, se confirma por WhatsApp (no incluido en el subtotal de la web).

## Tres modos de pedido

- **Sugerencia del chef** — enjarrada pre-diseñada; el cliente puede excluir ingredientes.
- **Haz tu mix** — flujo completo de 6 pasos.
- **Pack code** — cliente ingresa su código, descuenta 1 crédito, pedido sin costo adicional (salvo extras fuera de la base).

## Datos clave a formalizar

- TODO: zonas de cobertura de delivery (y si delivery es propio, tercerizado o mixto).
- TODO: ventana de atención (horas de toma de pedidos vs. horario de entrega).
- TODO: SLA por canal (tiempo de respuesta objetivo en WhatsApp).
- TODO: política de devoluciones / quejas.
- TODO: qué hacer si un insumo se agota durante el día (se notifica al cliente? se sustituye?).

## Procedimientos

### Confirmación y cierre del pedido (WhatsApp)
1. Al recibir el mensaje auto-generado, el operador humano (o eventual bot) confirma: dirección, horario, monto total.
2. Se coordina el pago: efectivo contra entrega o transferencia previa.
3. Si usa pack, se descuenta 1 crédito en Firestore (via admin panel o service).
4. Se registra el pedido en la colección `orders` de Firestore.

### Postventa
- TODO: encuesta de satisfacción (canal y frecuencia).
- TODO: política de recompra / recordatorio amable.

### Escalamiento de queja a humano
- Si el cliente muestra molestia clara, `servicio-cliente` (agente) redacta propuesta y pide al dueño confirmar antes de enviar.
- Casos complejos (intoxicación alimentaria, errores graves): escalar inmediatamente al dueño.

## Notificaciones automáticas (verificado en app)

- **WhatsApp**: mensaje formateado al número del negocio al confirmar pedido desde la web.
- **Email (opcional)**: notificación al admin via EmailJS; template configurable.
- **Firestore**: cada pedido se escribe como documento en la colección `orders`.

## Referencias a `data/`
- `data/clientes/` — historial e interacciones por cliente.
- Fuente primaria: Firestore collections `orders`, `clients`, `settings` del app.

## TODO
- Diagramar el customer journey completo con tiempos objetivo por etapa.
- Diseñar scripts base de respuesta para los casos más comunes (consulta de menú, cambio de pedido, alergia, queja).
- Definir política y SOP para delivery.
- Documentar cómo se sincronizan los pedidos de Firestore con el agente `servicio-cliente` (via MCP de Firebase, lectura directa, export periódico?).
