# Catalogo de ingredientes del menu Sin Cero

> Fuente: `src/store/index.js` del repo `lordiwa/sincero` (app Vue 3). Extraccion del 2026-04-21. Source-of-truth vigente: Firestore collection `settings` (la app lee defaults del store y los sincroniza a Firestore).

## Precios base y extras (objeto `prices`)

```javascript
{
  base: 4.20,          // Precio base de la enjarrada
  extraVeg: 0.30,      // Cada vegetal adicional despues del 6to
  extraProtein: 1.80,  // Cada proteina adicional despues de la 1ra
  extraCarb: 0.90,     // Cada carbohidrato adicional (normal o especial)
  extraFiber: 0.30,    // Fallback para fibras sin precio propio
  extraDressing: 0.90  // Cada aderezo adicional despues del 1ro
}
```

Nota: algunos ingredientes de carbohidratos/fibras tienen su propio precio individual (ver tabla). El `extraX` opera como fallback cuando no se especifica precio individual.

## Pack 20 almuerzos: $87.00

## Categorias y opciones

### 1. Verduras
- Incluidas en precio base: 6
- Maximo total: 8
- Precio de cada extra (>6): $0.30

| # | Nombre |
|---|---|
| 1 | Col Morada |
| 2 | Col China |
| 3 | Espinaca |
| 4 | Lechuga |
| 5 | Tomate Cherry |
| 6 | Pimiento |
| 7 | Calabacin |
| 8 | Pepino |
| 9 | Pepinillos encurtidos |
| 10 | Apio |
| 11 | Cebolla encurtida |
| 12 | Rabanitos encurtidos |
| 13 | Chayote |
| 14 | Aceitunas |
| 15 | Chochos |

### 2. Proteinas
- Incluidas en precio base: 1
- Maximo total: 2
- Precio de cada extra: $1.80

| # | Nombre |
|---|---|
| 1 | Pollo desmechado |
| 2 | Cerdo en julianas |
| 3 | Atun fresco en filetitos |
| 4 | Dorado en filetitos |
| 5 | Pechuga de pavo molida |
| 6 | Ternera molida |
| 7 | Res molida o desmechada |
| 8 | 3 Huevos cocidos |
| 9 | Tofu en cubos |

### 3. Carbohidratos normales
- Incluidos en precio base: 1 (sirve aparte para mantener textura)
- Maximo total: 4
- Precio unitario: $0.90 c/u

| # | Nombre | Precio |
|---|---|---|
| 1 | Papa cocida | $0.90 |
| 2 | Quinoa cocida | $0.90 |
| 3 | Arroz integral cocido | $0.90 |

### 4. Carbohidratos especiales
- No incluidos en el base (opcionales)
- Maximo total: 4
- Precio unitario: $0.90 c/u

| # | Nombre | Precio |
|---|---|---|
| 1 | Papa en pure | $0.90 |
| 2 | Quinoa crocante | $0.90 |
| 3 | Arroz integral crocante | $0.90 |
| 4 | Camote asado | $0.90 |
| 5 | Yuca cocida | $0.90 |

### 5. Fibras
- No incluidas en el base (opcionales, todas son extras)
- Maximo total: 2

| # | Nombre | Precio |
|---|---|---|
| 1 | Semillas de chia | $0.40 |
| 2 | Semillas de lino | $0.40 |
| 3 | Avena integral | $0.50 |
| 4 | Quinoa perlada | $0.60 |

### 6. Aderezos
- Incluidos en precio base: 1
- Maximo total: 4
- Precio de cada extra: $0.90

| # | Nombre |
|---|---|
| 1 | Manzana, oregano y mostaza |
| 2 | Aguacate, albahaca, limon y ajo |
| 3 | Tahini, limon y ajo |
| 4 | Jengibre, naranja, curry y chia |
| 5 | Yogurt deslactosado, aji y cebolla |
| 6 | Mayonesa de cilantro y perejil |

### 7. Frutas
- Opcional: maximo 1
- Sin costo extra (o incluida segun codigo)

| # | Nombre |
|---|---|
| 1 | Manzana |
| 2 | Mandarina |
| 3 | Pera |

## Alergenos comunes a marcar en cada ingrediente (TODO)

Para cada ingrediente deberia marcarse si contiene:
- Gluten (posible en avena integral si no es certificada libre de gluten)
- Lactosa (aderezo de yogurt deslactosado es bajo pero no cero lactosa)
- Huevo (aderezo mayonesa de cilantro y perejil, proteina "3 Huevos cocidos")
- Soja (tofu, posiblemente salsas)
- Frutos secos / semillas (chia, lino, ajonjoli en tahini)
- Mostaza (aderezo 1)
- Mariscos/pescado (atun, dorado)

## Ejemplo de calculo de precio

Enjarrada custom con:
- 7 verduras (6 base + 1 extra = $0.30)
- 2 proteinas (1 base + 1 extra = $1.80)
- 1 carbohidrato normal ($0.90)
- 1 carbohidrato especial ($0.90)
- 1 fibra (ej: quinoa perlada $0.60)
- 2 aderezos (1 base + 1 extra = $0.90)
- 1 fruta (sin costo)

**Total: $4.20 + $0.30 + $1.80 + $0.90 + $0.90 + $0.60 + $0.90 = $9.60** + delivery.

## TODO

- Completar columna de alergenos para cada ingrediente.
- Agregar perfil nutricional por ingrediente (100 g) cuando el nutricionista lo consolide.
- Mapear proveedor y costo de compra por ingrediente (responsabilidad de inventario).
- Confirmar que "Frutas" no tiene costo en la logica de la app (o reportar el valor real).
- Mantener sincronizado con Firestore `settings` — ejecutar export peridoico (dev-sin-cero).
