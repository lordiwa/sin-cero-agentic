# Recetas

> Catálogo, estructura y lógica de las recetas que produce Sin Cero.

## Agentes que lo consumen
- `inventario` (para mapear receta → insumos → costo)
- `nutricionista` (para evaluar perfil nutricional y proponer nuevas)
- `servicio-cliente` (para responder qué hay disponible y describirlo al cliente)
- `marketing` (consulta de lectura para generar contenido)

## Vocabulario clave
- **Receta**: TODO definir formalmente para Sin Cero.
- **Plato**: TODO.
- **Insumo**: TODO.
- **Rendimiento**: TODO (porciones por preparación).

## Datos clave
- TODO: catálogo activo de recetas (cuántas, qué categorías).
- TODO: criterios de "receta Sin Cero" (qué la hace consistente con la marca).
- TODO: política de cambios estacionales.

## Procedimientos típicos
- TODO: cómo se da de alta una receta nueva.
- TODO: cómo se valida nutricionalmente.
- TODO: cómo se calcula el costo unitario.
- TODO: cómo se discontinúa una receta.

## Referencias a `data/`
- `data/recetas/` — un archivo por receta (formato a definir: markdown con frontmatter, JSON, YAML).

## TODO
- Definir el formato canónico de un archivo de receta (campos mínimos: nombre, ingredientes con cantidades, pasos, rendimiento, perfil nutricional, etiquetas/preferencias).
- Importar el catálogo actual a `data/recetas/`.
