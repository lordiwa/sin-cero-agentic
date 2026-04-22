# Preferencias alimentarias

> Cómo Sin Cero modela las preferencias y restricciones alimentarias de sus clientes.

## Agentes que lo consumen
- `nutricionista` (para diseñar opciones que cubran segmentos)
- `servicio-cliente` (para personalizar recomendaciones)
- `marketing` (consulta de lectura, para segmentar mensajes)

## Vocabulario clave
- **Preferencia**: elección voluntaria (vegano, vegetariano, keto, low-carb, omnívoro).
- **Restricción**: limitación obligatoria por salud (alergia, intolerancia, condición médica).
- **Aversión**: rechazo por gusto (no es alergia, no es preferencia ideológica).
- **Exclusión**: ingrediente específico que el cliente no quiere en su enjarrada (puede venir de cualquiera de las tres).

## Ventaja estructural del producto

El modelo de enjarrada por componentes separados **ya resuelve la mayor parte de la customización**:
- El cliente elige qué vegetales, qué proteína, qué carbohidrato y qué aderezo.
- Para un vegano, basta elegir proteína vegetal (lentejas, garbanzos, tofu si se ofrece) + aderezo sin lácteos.
- Para un celíaco, evitar carbohidratos con gluten (pan, pasta) y confirmar aderezos sin salsa de soya común.
- Para una alergia, basta excluir el ingrediente de cada categoría.

Implicación: **la app ya habilita esto** via los selectores por paso. El especialista `servicio-cliente` debe saber guiar al cliente a través de los selectores para cumplir su perfil.

## Lista de preferencias soportadas (propuesta inicial)

- Omnívoro (default)
- Vegetariano (sin carne, incluye huevo/lácteos si se ofrecen)
- Vegano (sin productos de origen animal)
- Keto / low-carb
- Alta en proteína
- Ecuatoriano tradicional (con quinua, chocho, tubérculos andinos)

## Lista de restricciones soportadas (propuesta inicial)

Alérgenos comunes a etiquetar en cada receta/ingrediente:
- Gluten (trigo, cebada, centeno, avena si no es certificada)
- Lactosa / lácteos
- Frutos secos (almendras, nueces, maní — especial: en Ecuador el maní es frecuente)
- Soja (salsa soya, tofu)
- Huevo
- Mariscos / pescado
- Semillas específicas (chía, linaza, ajonjolí)

## Almacenamiento en la ficha del cliente

Enum lista + texto libre para matices:

```yaml
preferencias:
  - vegano
restricciones:
  - sin-gluten
  - sin-lactosa
aversiones:
  - cebolla
  - cilantro
notas_alimentarias: "Prefiere aderezos cítricos antes que cremosos."
```

## Procedimientos

### Onboarding (primera compra)
- Preguntar por: preferencia principal, alergias (crítico), aversiones fuertes.
- Registrar en `data/clientes/<id>.md`.

### Actualización
- Si el cliente menciona cambio ("ahora soy vegano"), actualizar con confirmación del usuario dueño (escrituras a `data/clientes/` son sensibles).

### Filtrado de menú para un cliente
1. Leer perfil del cliente.
2. Filtrar recetas/componentes en `data/recetas/` que **cumplan** preferencia (hard filter).
3. Excluir recetas/componentes con ingredientes en `restricciones` (hard filter — crítico).
4. Ordenar descendente por compatibilidad con `aversiones` (soft preference).

### Si el cliente tiene alergia severa
- Confirmar doblemente al tomar el pedido.
- Flagear el pedido para producción con alerta visual (TODO formalizar en app).
- Prevenir contaminación cruzada en mise en place (protocolo de cocina).

## Referencias a `data/`
- `data/clientes/<id>.md` — perfil individual.
- `data/recetas/<slug>.md` — cada receta debe incluir `preferencias_compatibles` y `excluye` para filtrar correctamente.

## TODO
- Definir el enum canónico de preferencias y restricciones en la app (alinear con Firestore).
- Etiquetar cada ingrediente del menú con sus alérgenos (revisar si el admin panel lo permite o hay que extender).
- Diseñar alertas visuales para pedidos con alergias severas en el flujo de producción.
- Estudiar si conviene ofrecer explícitamente una "línea vegana" como filtro rápido en la app.
