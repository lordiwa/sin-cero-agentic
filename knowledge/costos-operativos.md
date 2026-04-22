# Costos operativos

> Estructura de costos del negocio Sin Cero y cómo se calculan márgenes.

## Agentes que lo consumen
- `inventario` (único consumidor activo)

## Vocabulario clave
- **Costo unitario**: costo de producir una enjarrada.
- **Costo variable**: depende del volumen (insumos, empaque, delivery si aplica).
- **Costo fijo**: no depende del volumen (alquiler de cocina, salarios base, mantenimiento app, etc.).
- **Margen bruto**: precio de venta − costo variable unitario.
- **Margen de contribución**: ingresos − costos variables totales.
- **Punto de equilibrio**: nivel de ventas que cubre costos fijos.

## Precios de venta actuales (verificado en `src/store/index.js`)

**Objeto `prices` del store:**

```javascript
{
  base: 4.20,          // Enjarrada base: 6 vegetales + 1 proteina + 1 carb + 1 aderezo
  extraVeg: 0.30,      // Cada vegetal adicional despues del 6to
  extraProtein: 1.80,  // Cada proteina adicional
  extraCarb: 0.90,     // Cada carbohidrato adicional (normal o especial)
  extraFiber: 0.30,    // Fallback; fibras tienen precios individuales
  extraDressing: 0.90  // Cada aderezo adicional
}
```

**Otros precios / conceptos:**

| Concepto | Precio (USD) | Notas |
|---|---|---|
| Enjarrada base | **$4.20** | 6 vegetales + 1 proteína + 1 carb + 1 aderezo + 1 fruta |
| Vegetal extra (>6) | +$0.30 | c/u, maximo total 8 |
| Proteína extra | +$1.80 | c/u, maximo total 2 |
| Carbohidrato (normal o especial) | +$0.90 | c/u, maximo 4 |
| Fibra (chia/lino) | +$0.40 | c/u |
| Fibra (avena integral) | +$0.50 | c/u |
| Fibra (quinoa perlada) | +$0.60 | c/u |
| Aderezo extra (>1) | +$0.90 | c/u, maximo total 4 |
| Fruta | $0.00 | incluida, maximo 1 |
| **Pack 20 almuerzos** | **$87** | $4.35/u (0.15 premium por prepago) |
| **Descuento por retorno de frascos** | (15 frascos = 1 almuerzo gratis) | logica en `fidelityService.js` (`grantJarFreeMeal`) |

Catálogo completo de ingredientes por categoría: ver `data/inventario/catalogo-ingredientes.md`.

## Datos clave a completar

- TODO: estructura de costos fijos mensuales (alquiler cocina, salarios, servicios, licencias ARCSA, hosting Firebase).
- TODO: tabla de costos unitarios por insumo con proveedor y unidad de compra.
- TODO: merma promedio por categoría de insumo (hojas verdes vs proteínas vs granos).
- TODO: costo unitario de empaque (jarro + tapa + etiqueta) y su impacto en el retorno circular.
- TODO: costo de delivery (propio o tercerizado) y política de quién lo absorbe.

## Procedimientos

### Cálculo de costo de una enjarrada
1. Sumar costo de cada componente según cantidad estándar en la receta.
2. Aplicar factor de merma por categoría (ej: hojas verdes ~15%, granos cocidos ~5%).
3. Sumar costo de empaque (con política de recuperación por retorno).
4. Prorratear mano de obra de preparación (lote promedio).
5. Comparar con precio de venta → margen bruto unitario.

### Revisión de costos cuando cambia un proveedor
1. Actualizar precio del insumo en `data/inventario/<insumo>.md`.
2. Recalcular costos de todas las recetas que lo usan (filtrar por insumo).
3. Flagear recetas cuyo margen caiga bajo el mínimo objetivo (TODO definir umbral).

### Rentabilidad de una receta nueva
- Costo objetivo ≤ 40% del precio de venta al cliente (cifra a validar con el dueño).
- Si no se cumple, proponer ajustar ingredientes antes de dar de alta.

## Referencias a `data/`
- `data/inventario/` — costos de insumos y proveedores.
- `data/recetas/` — cada receta debe tener o permitir calcular su costo unitario.

## TODO
- Validar con el dueño: umbral de margen objetivo por enjarrada.
- Confirmar monto y lógica del descuento por retorno de envase.
- Importar estructura de costos fijos actual.
- Definir si se calcula costo estándar (precio fijo pactado con proveedor) o costo real (FIFO/promedio ponderado).
