# Templates de mensajes

> Mensajes estandarizados que usa el negocio Sin Cero (cliente, admin, automaticos). Fuente principal: `src/services/orderService.js` y `src/services/emailService.js` del repo de la app.

## Agentes que lo consumen
- `servicio-cliente` (para responder con el mismo tono y formato)
- `marketing` (para alinear copy en redes con el del pedido)
- `dev-sin-cero` (para automatizar envios futuros)

---

## 1. Template WhatsApp: nuevo pedido (auto-generado por la app)

La web app genera este mensaje y abre `wa.me/593984645737?text=<mensaje>` cuando el cliente confirma el pedido.

```
Hola Sin Cero! 👋 Quisiera hacer el siguiente pedido:

*Cliente:* ${clientName}
*Telefono:* ${clientPhone}
*Direccion:* ${clientAddress}

--- MI ENJARRADA ---
*Verduras* (6 base incluidas):
[lista numerada; marcadas con ⭐ EXTRA si >6, con precio unitario]

*Proteinas* (1 base incluida):
[lista numerada; marcadas con ⭐ EXTRA si >1, con precio unitario]

*Carbohidratos* (1 base incluido, van por separado):
[lista numerada, con precios]

*Carbohidratos Especiales* (opcionales, van por separado):
[lista numerada, con precios]

*Fibras* (opcionales):
[lista numerada, con precios]

*Aderezos* (1 base incluido):
[lista numerada; marcados con ⭐ EXTRA si >1, con precio]

*Fruta:* ${fruit || 'Ninguna'}

*SUBTOTAL COMIDA: $${total}*
_(No incluye costo de delivery)_

⭐ = Ingrediente EXTRA con costo adicional

*Por favor confirmen:*
- Disponibilidad de ingredientes
- Costo de entrega a mi direccion

📅 *Entregas:* Lunes a Viernes, 12:00 PM - 15:00 PM
⏰ *Pedidos hasta las 21:30* del dia anterior
📋 *Ver pedido:* ${adminUrl}/admin/pedido/${orderId}
Gracias!
```

### Reglas de respuesta del operador a este mensaje
- Saludo breve + confirmar recepcion: *"Hola ${nombre}! Recibi tu pedido."*
- Confirmar disponibilidad de ingredientes (revisar stock).
- Cotizar delivery segun zona.
- Confirmar total final (enjarrada + delivery).
- Confirmar horario de entrega dentro de la ventana L-V 12-3pm.
- Pedir metodo de pago (efectivo contra entrega / transferencia previa).

---

## 2. Template email (EmailJS) al admin

Payload enviado a EmailJS para notificar al admin de un pedido nuevo:

```javascript
{
  to_email: 'info@sin-cero.com',
  customer_name: orderData.cliente,
  customer_phone: orderData.telefono,
  customer_address: orderData.direccion,
  vegetables: orderData.items.verduras.join(', '),
  protein: orderData.items.proteina,
  carb: orderData.items.carbohidrato,
  dressing: orderData.items.aderezo,
  fruit: orderData.items.fruta,
  extra_vegetables: orderData.extras.verdurasExtra || 0,
  total: orderData.total,
  order_date: orderData.fecha,
  order_time: orderData.hora,
  order_summary: <cuerpo HTML completo>
}
```

---

## 3. Plantillas propuestas para servicio-cliente

### Bienvenida (primera compra)
```
Hola ${nombre}! 👋 Gracias por tu primera enjarrada en Sin Cero.
Comida 100% natural, sin ultra procesados.
Si tenes alguna preferencia o alergia, contanos por aqui y lo tenemos siempre en cuenta.
```

### Recordatorio de entrega (dia del pedido, en la mañana)
```
Buenos dias ${nombre}! Te confirmamos tu pedido de hoy:
${resumen corto}
Entregamos entre ${hora_estimada}. Cualquier ajuste, avisanos.
```

### Almuerzo gratis ganado (al llegar a la 25ta compra)
```
${nombre}! 🎉 Acabas de llegar a 25 almuerzos con nosotros.
Tu proximo almuerzo va por la casa. Cuando quieras pedirlo, avisanos.
```

### Frascos devueltos: bono (al llegar a 15 frascos devueltos)
```
${nombre}! Tus 15 frascos devueltos nos ayudaron a reducir impacto ambiental.
Te sumamos un almuerzo gratis por eso. Gracias por cuidar el planeta con nosotros.
```

### Pack proximo a expirar (14 dias antes)
```
Hola ${nombre}! Tu pack ${codigo} vence el ${fecha_expiracion} y te quedan ${creditos_restantes} almuerzos.
Podes consumirlos antes o renovar con un nuevo pack de 20 a $87.
```

### Recordatorio de cobranza (B2B, si aplica)
```
Hola ${nombre}! Te escribimos para recordarte que tenes pendiente ${monto}
correspondiente a ${concepto}. Podemos coordinarlo cuando te acomode.
```

### Queja: acuse de recibo
```
${nombre}, sentimos mucho lo que paso. Tu feedback nos importa.
Ya estamos revisando el caso. Te confirmamos solucion en menos de 24 horas.
```

### Respuesta a alergia
```
${nombre}, gracias por avisarnos de tu alergia a ${alergeno}.
Lo registramos en tu ficha y lo tenemos en cuenta en cada pedido.
Te recomendamos evitar: ${ingredientes_con_alergeno}.
```

---

## 4. Reglas de tono

- Tuteo informal ("te", "contanos", "avisanos"). Sin "usted".
- Sin emojis agresivos, si emojis humanos puntuales (👋 🎉 ❤️ ✅ 📅 ⏰).
- Mensajes cortos. Si algo necesita mas de 4 parrafos, proponer llamada o videollamada.
- Nunca dar consejo medico. Derivar a profesional.
- En cobranzas: cordial, nunca amenazante.
- Cerrar con CTA claro (que accion pedimos al cliente).

---

## 5. Horarios operativos (canonicos)

- **Recepcion de pedidos**: hasta las **21:30** del dia anterior a la entrega.
- **Entregas**: **Lunes a Viernes, 12:00 PM a 15:00 PM**.
- **Atencion WhatsApp**: TODO formalizar ventana (sugerido: L-V 9:00-18:00, sabados 10:00-14:00).

## TODO

- Validar plantillas con el dueno (tono correcto, nada que suene forzado).
- Migrar las plantillas a un CMS o al admin panel para que sean editables sin tocar codigo.
- Configurar MCP de WhatsApp para que servicio-cliente pueda enviar (hoy es solo generacion de texto para copy/paste).
- Definir plantilla de factura si se activa B2B.
