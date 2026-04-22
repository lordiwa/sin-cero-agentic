# TEMPLATE: Ficha de cliente

> Este es el formato canónico. Copiar a `<telefono>.md` (sin el prefijo `+` o espacios) y llenar. Alineado con Firestore `clientes`.

```yaml
telefono: "593987654321"
nombre: ""
direccion: ""
preferencias: []
restricciones: []
aversiones: []
almuerzosTotales: 0
almuerzosParaSiguienteGratis: 0
almuerzosGratisDisponibles: 0
frascosParaSiguienteGratis: 0
frascosTotalesDevueltos: 0
totalGastado: 0.00
createdAt: 2026-04-21
ultimoContacto: 2026-04-21
```

## Preferencias y restricciones conocidas

- Preferencia declarada: (ej: omnívoro | vegetariano | vegano | keto)
- Restricciones: (ej: sin-gluten, sin-lactosa, alergia a X)
- Aversiones: (ingredientes específicos que rechaza por gusto)

## Historial de pedidos

| Fecha | Pedido ID | Modo | Total | ¿Pack? | ¿Gratis? |
|---|---|---|---|---|---|
| 2026-04-21 | abc123 | mix | 4.80 | no | no |

## Historial de bonos / frascos gratis

| Fecha | Tipo | Motivo |
|---|---|---|
|  |  |  |

## Notas libres del operador

- Fechas/eventos importantes sobre el cliente que no caben en el modelo.

## Política de escritura

**IMPORTANTE:** las escrituras a este archivo requieren confirmación explícita del usuario (ver `.claude/settings.json`). `servicio-cliente` nunca escribe sin preguntar.
