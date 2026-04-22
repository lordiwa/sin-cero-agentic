# Clientes

> Modelo de datos del cliente, lifecycle, programa de fidelidad, packs y comunicación.

## Agentes que lo consumen
- `servicio-cliente` (único consumidor activo)

## Vocabulario clave
- **Lead**: contacto inicial sin compra.
- **Cliente activo**: con compras recientes (TODO definir umbral).
- **Cliente dormido**: sin compras en X tiempo (TODO definir).
- **Pack**: crédito prepagado de 20 almuerzos ($87), asociado a un código `nombre-1234`.
- **Fidelidad**: registro de compras para el beneficio de "25 compras = 1 almuerzo gratis".

## Modelo de datos del cliente (alineado con Firestore de la app)

Colección Firestore: `clientes`. Campos reales (de `fidelityService.js`):

```yaml
telefono: "593987654321"      # PK de búsqueda (siempre con código país)
nombre: ""
direccion: ""                  # dirección completa
almuerzosTotales: 0            # lifetime counter
almuerzosParaSiguienteGratis: 0  # 0-24 (reset al llegar a 25)
almuerzosGratisDisponibles: 0  # almuerzos gratis acumulados sin usar
frascosParaSiguienteGratis: 0  # 0-14 (reset al llegar a 15)
frascosTotalesDevueltos: 0     # lifetime counter, nunca decrementa
totalGastado: 0.00
historialPedidos:              # array
  - fecha: <Timestamp>
    pedidoId: "<order-id>"
    total: 4.20
    esGratis: false            # true si fue almuerzo de fidelidad
    usoPack: false             # true si usó pack code
  - fecha: <Timestamp>
    tipo: "bonus"
    cantidad: 1
    motivo: "Promoción lanzamiento"
  - fecha: <Timestamp>
    tipo: "frasco_gratis"
    motivo: "Almuerzo gratis por 15 frascos devueltos"
createdAt: <Timestamp>
```

Campos adicionales sugeridos (aún no en Firestore, TODO alinear):

```yaml
preferencias:
  - vegetariano | vegano | keto | omnívoro | etc.
restricciones:
  - sin-gluten | sin-lactosa | alergia-X | etc.
aversiones:
  - ""
ultimoContacto: <Timestamp>
notas: ""
```

## Programa de fidelidad (verificado en `fidelityService.js`)

- **Umbral almuerzos:** constante `ALMUERZOS_PARA_GRATIS = 25`. Cada compra cuenta +1 (campos `almuerzosTotales` y `almuerzosParaSiguienteGratis`). Al llegar a 25 → reset a 0, +1 a `almuerzosGratisDisponibles`, emit `ganoGratis: true`.
- **Umbral frascos:** 15 frascos devueltos = 1 almuerzo gratis (admin action `grantJarFreeMeal`). Campos `frascosParaSiguienteGratis` (cycle) y `frascosTotalesDevueltos` (lifetime).
- **Historial:** cada pedido, bono o frasco gratis se registra en `historialPedidos` con timestamp y tipo.
- **Transacciones:** todas las sumas son atómicas usando `runTransaction` de Firestore.

## Packs (verificado en `packService.js`)

Colección Firestore: `packs`. Estructura de documento:

```yaml
codigo: "juan-4527"            # nombre (max 10 chars, lowercase, sin acentos) + "-" + 4 dígitos (1000-9999)
nombreComprador: ""
creditosIniciales: 20
creditosDisponibles: 17
creditosUsados: 3
fechaCompra: <Timestamp>
fechaExpiracion: <Timestamp>
tipoExpiracion: "creditos_compra"  # o "almuerzos"
activo: true
precioTotal: 87.00
historialUso:                  # array de entries
  - fecha: <Timestamp>
    tipo: "uso"                # o "bonus", "frasco_gratis"
    creditosAfectados: 1
    pedidoId: "<order-id>"
    extrasAdicionales: 0       # cargos fuera de la base si los hubo
```

**Reglas:**
- **Tipos de expiración:**
  - `almuerzos` → 3 meses desde `fechaCompra`.
  - `creditos_compra` (default) → 1 año desde `fechaCompra`.
- **Búsqueda de código:** case-insensitive.
- **Validación para usar:** `activo == true`, `creditosDisponibles > 0`, `fechaExpiracion > ahora`.
- **Transacción `useCredit`:** atómica (runTransaction) — lee pack, valida, decrementa créditos, agrega al historial.
- **Renovación:** admin agrega +20 créditos y extiende fechaExpiracion.
- **Devolución (refund):** admin puede devolver créditos al pack (caso de pedido cancelado).

## Lifecycle del cliente (propuesta)

1. **Lead** → contacto por WhatsApp o Instagram sin compra.
2. **Primer pedido** → pasa a "cliente nuevo".
3. **Recurrente** → 3+ compras en 30 días (umbral a validar).
4. **Con pack activo** → tiene créditos prepagados.
5. **Pack agotado** → oportunidad de renovación.
6. **Dormido** → >60 días sin compra (umbral a validar).
7. **Embajador** → múltiples referidos + historial consistente.

Triggers de comunicación por etapa: TODO diseñar.

## Política de cobranzas

- Modelo actual: pago al recibir (efectivo/transferencia via WhatsApp) o prepago (pack).
- No hay crédito abierto a clientes individuales por defecto.
- TODO: política para clientes B2B (corporativo) que pagan contra factura.

## Comunicación

- **Canal principal:** WhatsApp.
- **Tono:** cálido, claro, resolutivo. Tuteo informal. En cobranzas: firme pero cordial.
- **Plantillas:** todas vivien en [`knowledge/templates-mensajes.md`](templates-mensajes.md) — bienvenida, recordatorio de entrega, almuerzo gratis ganado, pack próximo a expirar, cobranza, alergias, quejas.
- **Formato auto-generado por la app:** `orderService.js` genera el mensaje inicial del cliente. `servicio-cliente` debe responder con el mismo registro y tono.

## Referencias a `data/`
- `data/clientes/<id>.md` o `<id>.json` — un archivo por cliente.
- Fuente primaria: Firestore collection `clients` (o equivalente) en el proyecto Firebase del app.

## Restricciones
- **Datos sensibles**: las escrituras a `data/clientes/` requieren confirmación del usuario.
- Nunca compartir datos de un cliente con otro.
- Si el cliente describe condición de salud, derivar a profesional (no diagnosticar).

## TODO
- Conciliar schema con el modelo real en Firestore (que usa la app).
- Diseñar las plantillas de mensaje faltantes.
- Definir umbrales de "cliente dormido" y ritmo de reactivación.
- Definir política B2B corporativa si se activa ese segmento.
