# Recetas

> Catálogo, estructura y lógica de las recetas que produce Sin Cero.

## Agentes que lo consumen
- `inventario` (para mapear receta → insumos → costo)
- `nutricionista` (para evaluar perfil nutricional y proponer nuevas)
- `servicio-cliente` (para responder qué hay disponible y describirlo al cliente)
- `marketing` (consulta de lectura para generar contenido)

## Concepto del producto (verificado en app)

La "receta" de Sin Cero es una **enjarrada**: ensalada gourmet servida en jarro con componentes separados para mantener frescura. El cliente arma la enjarrada combinando ingredientes de 5-6 categorías.

**Hay 7 categorías** (verificado en código):

1. **Verduras** — 6 base incluidas, extras +$0.30 c/u, máximo 8 total. 15 opciones.
2. **Proteínas** — 1 base incluida, extras +$1.80 c/u, máximo 2. 9 opciones.
3. **Carbohidratos normales** — 1 base incluido, extras +$0.90 c/u, máximo 4. 3 opciones (papa, quinoa, arroz integral).
4. **Carbohidratos especiales** — opcionales, +$0.90 c/u, máximo 4. 5 opciones (papa puré, quinoa crocante, arroz integral crocante, camote asado, yuca).
5. **Fibras** — opcionales, precio individual por item ($0.40 a $0.60), máximo 2. 4 opciones.
6. **Aderezos** — 1 base incluido, extras +$0.90 c/u, máximo 4. 6 opciones (nombres descriptivos, no formulaciones).
7. **Frutas** — opcional, sin costo, máximo 1. 3 opciones (manzana, mandarina, pera).

Catálogo literal con todos los items y precios: ver [`data/inventario/catalogo-ingredientes.md`](../data/inventario/catalogo-ingredientes.md).

## Modos de pedido (verificado en app)

1. **Sugerencia del chef** — el cliente marca los ingredientes que **NO quiere** y el chef arma la enjarrada con el resto disponible. No es una receta pre-diseñada; es selección por exclusión (componente `SugerenciaChef.vue`).
2. **Haz tu mix** — customización total paso a paso (flujo mobile completo).
3. **Pack code** — el cliente tiene un código `nombre-1234` y redime un almuerzo contra sus créditos (formato: 10 chars nombre minúsculas + 4 dígitos).

## Formato canónico de una receta (propuesta)

Cada receta (sugerencia del chef o enjarrada nueva) vive en `data/recetas/<slug>.md`:

```yaml
---
nombre: "Enjarrada Mediterránea"
slug: "enjarrada-mediterranea"
tipo: sugerencia | custom | estacional
precio_base: 4.20  # USD
ingredientes:
  vegetales: [espinaca, tomate, pepino, zanahoria, cebolla morada, arúgula]
  proteina: "pollo a la plancha"
  carbohidrato: "quinoa blanca"
  carbohidrato_especial: null  # o "quinoa crocante" (+0.60)
  fibras: ["almendras", "semillas de chía"]
  aderezo: "vinagreta de limón"
etiquetas: [mediterránea, alta-en-proteína, sin-gluten]
excluye: []  # alérgenos/restricciones que NO incluye
preferencias_compatibles: [omnívoro, sin-gluten]
perfil_nutricional:
  kcal: ~450
  proteina_g: ~30
  carbos_g: ~40
  grasa_g: ~18
  fibra_g: ~8
  sodio_mg: ~600
estado: activa | pausada | discontinuada
fecha_creacion: 2026-04-21
---

# Enjarrada Mediterránea

Breve descripción comercial para el cliente (2-3 líneas).

## Notas de preparación
- Orden de montaje en el jarro (pesado abajo, frágiles arriba).
- Aderezo separado para servir al momento.
```

## Procedimientos

- **Alta de receta nueva:** `nutricionista` propone ingredientes y perfil → `inventario` valida costo y disponibilidad → se crea el archivo en `data/recetas/` → se actualiza el menú en Firebase (via admin panel).
- **Validación nutricional:** macros estimados a partir de cantidades estándar; etiquetas según criterios en `factores-nutricionales.md`.
- **Cálculo de costo:** ver `costos-operativos.md` (suma de insumos por cantidad + merma + mano de obra prorrateada).
- **Discontinuación:** marcar `estado: discontinuada` en el archivo; no borrar (para trazabilidad en pedidos antiguos).

## Referencias a `data/`
- `data/recetas/<slug>.md` — un archivo por receta activa o archivada.

## TODO
- Importar el catálogo actual desde Firebase (Firestore collection `settings` o equivalente) a `data/recetas/`.
- Confirmar si "fibras" y "fruta" son la misma categoría o distintas en la versión actual de la app.
- Definir umbrales exactos para etiquetas (alta-en-proteína, etc.) en `factores-nutricionales.md`.
