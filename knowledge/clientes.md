# Clientes

> Modelo de datos del cliente, lifecycle, política de cobranzas y comunicación.

## Agentes que lo consumen
- `servicio-cliente` (único consumidor activo)

## Vocabulario clave
- **Lead**: contacto inicial sin compra.
- **Cliente activo**: con compras recientes (definir umbral).
- **Cliente dormido**: sin compras en X tiempo (definir).
- **Cobranza**: gestión del cobro pendiente.

## Modelo de datos del cliente

Esquema sugerido (ajustar):

```yaml
id: <slug o uuid>
nombre: ""
contacto:
  whatsapp: ""
  email: ""
  telefono: ""
preferencias:
  - vegetariano | vegano | keto | etc.
restricciones:
  - sin gluten | sin lactosa | alergia X | etc.
aversiones:
  - ""
direccion_entrega:
  - ""
historial_pedidos:
  - fecha: 2026-04-21
    items: []
    total: 0
    estado: entregado | pendiente | cancelado
saldo_pendiente: 0
notas:
  - ""
ultimo_contacto: 2026-04-21
```

## Lifecycle del cliente
- TODO: definir etapas (lead → primer pedido → recurrente → embajador / dormido).
- TODO: triggers de cada etapa.

## Política de cobranzas
- TODO: plazos máximos.
- TODO: tono de los recordatorios (suave → firme).
- TODO: cuándo escalar a humano.

## Comunicación
- Canal principal: WhatsApp.
- Tono: cálido, claro, resolutivo. En cobranzas, firme pero cordial.
- TODO: catálogo de mensajes plantilla (bienvenida, confirmación pedido, recordatorio entrega, cobro, postventa).

## Referencias a `data/`
- `data/clientes/<id>.md` o `<id>.json` — un archivo por cliente.

## Restricciones
- **Datos sensibles**: las escrituras a `data/clientes/` requieren confirmación del usuario.
- Nunca compartir datos de un cliente con otro.
- Si el cliente describe condición de salud, derivar a profesional.

## TODO
- Definir formato canónico de archivo de cliente.
- Importar clientes existentes (si los hay).
- Diseñar plantillas de mensajes.
