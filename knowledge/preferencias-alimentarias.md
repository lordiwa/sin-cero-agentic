# Preferencias alimentarias

> Cómo Sin Cero modela las preferencias y restricciones alimentarias de sus clientes.

## Agentes que lo consumen
- `nutricionista` (para diseñar opciones que cubran segmentos)
- `servicio-cliente` (para personalizar recomendaciones)
- `marketing` (consulta de lectura, para segmentar mensajes)

## Vocabulario clave
- **Preferencia**: elección voluntaria (vegano, vegetariano, keto, low-carb, etc.).
- **Restricción**: limitación obligatoria por salud (alergia, intolerancia, condición médica).
- **Aversión**: rechazo por gusto (no es alergia, no es preferencia ideológica).

## Datos clave
- TODO: lista canónica de preferencias soportadas.
- TODO: lista canónica de restricciones (alérgenos comunes: gluten, lactosa, frutos secos, soja, huevo, mariscos, etc.).
- TODO: cómo se almacena esto en la ficha del cliente (enum vs texto libre vs ambos).

## Procedimientos típicos
- TODO: cómo se levanta esta info al onboardear un cliente nuevo.
- TODO: cómo se actualiza si el cliente cambia.
- TODO: cómo se filtra el catálogo de recetas según el perfil del cliente.

## Referencias a `data/`
- `data/clientes/` — perfil de cada cliente.
- `data/recetas/` — etiquetas por receta deben permitir filtrar por preferencia/restricción.

## TODO
- Definir el esquema unificado preferencias↔restricciones↔aversiones.
- Garantizar que toda receta esté etiquetada con compatibilidades.
