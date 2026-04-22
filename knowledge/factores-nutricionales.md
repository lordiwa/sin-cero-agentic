# Factores nutricionales

> Criterios y datos nutricionales que aplica Sin Cero a sus enjarradas.

## Agentes que lo consumen
- `nutricionista` (criterio de diseño y validación)
- `servicio-cliente` (responder consultas básicas del cliente)

## Principio guía del negocio

**Tagline:** "Comida 100% natural, sin ultra procesados".

Esto se traduce en criterios duros:
- Sin edulcorantes artificiales (stevia natural sí; sucralosa, aspartame no).
- Sin conservantes añadidos (vinagre y cítricos como preservantes naturales sí).
- Sin saborizantes artificiales ni colorantes.
- Priorizar ingredientes frescos sobre enlatados/ultraprocesados.
- Aceites: preferir oliva virgen extra, aguacate; evitar aceites vegetales refinados cuando sea posible.

## Vocabulario clave

- **Macros**: proteínas, carbohidratos, grasas.
- **Micros**: vitaminas, minerales, fibra.
- **Densidad calórica**: kcal por gramo o por porción.
- **Etiquetas Sin Cero**: clasificaciones que aparecen al cliente (alta en proteína, baja en sodio, alta en fibra, antiinflamatoria, etc.).

## Etiquetas oficiales Sin Cero (propuesta inicial, ajustar con dueño)

| Etiqueta | Umbral por enjarrada |
|---|---|
| **Alta en proteína** | ≥ 25 g proteína |
| **Alta en fibra** | ≥ 8 g fibra |
| **Baja en sodio** | ≤ 600 mg sodio |
| **Baja densidad calórica** | ≤ 350 kcal |
| **Antiinflamatoria** | Contiene 3+ ingredientes antiinflamatorios (verdes oscuros, frutos rojos, omega-3, cúrcuma, jengibre, AOVE) |
| **Alta en omega-3** | ≥ 1 g ALA o EPA+DHA |
| **Sin ultra procesados** | Aplica por default a todo el menú (es parte del tagline) |

Los umbrales son orientativos; ajustar con dueño y con base en `data/investigaciones/` cuando haya guías locales (Ecuador) o evidencia nueva.

## Tablas de referencia nutricional

Fuentes a consultar para el perfil de cada ingrediente:

- **USDA FoodData Central** — referencia internacional, inglés.
- **BEDCA** — Base de Datos Española de Composición de Alimentos.
- **Tablas ecuatorianas** — si existen oficiales de MSP Ecuador (TODO verificar), preferir para alimentos locales.
- Para ingredientes andinos (quinua, chocho, amaranto, mashua): priorizar fuentes regionales.

## Procedimientos

### Cálculo del perfil nutricional de una enjarrada nueva
1. Para cada ingrediente con su cantidad, buscar perfil en tabla de referencia.
2. Sumar macros y micros.
3. Aplicar factores de cocción cuando aplique (ej: cocción reduce ~10% de vitamina C en hojas).
4. Redondear a enteros para presentación al cliente.
5. Asignar etiquetas según umbrales.

### Comunicación al cliente
Formato sugerido para ficha de enjarrada:

```
Enjarrada Mediterránea
~450 kcal · 30g proteína · 40g carbos · 18g grasa · 8g fibra
Etiquetas: alta-en-proteína · antiinflamatoria · sin-ultraprocesados
```

Publicar macros en Instagram es una oportunidad competitiva fuerte (ningún competidor ecuatoriano lo hace — ver `knowledge/marketing-estrategia.md`).

### Validación de claim clínico
Cualquier afirmación del tipo "reduce X" o "previene Y" debe marcarse con **[Verificar con dietista certificado]** antes de publicarse.

## Ingredientes recurrentes en el menú (a perfilar)

TODO completar con perfiles de los ingredientes más frecuentes:
- Quinua (blanca, roja, negra)
- Chocho
- Aguacate
- Pollo a la plancha
- Atún
- Lentejas / garbanzos
- Hojas verdes (espinaca, arúgula, kale, lechuga)
- Tomate cherry
- Aderezos (vinagreta de limón, tahini, aceite + vinagre balsámico)

## Referencias a `data/`
- `data/recetas/<slug>.md` — cada receta debe tener su perfil nutricional embebido.
- `data/investigaciones/` — estudios y evidencia usada (ej: `tendencias-nutricion-2026-04-21.md`).

## TODO
- Validar umbrales de etiquetas con dueño y/o con dietista certificado.
- Cargar tabla de composición para los 20-30 ingredientes más usados en una hoja consultable.
- Decidir si las fichas nutricionales se publican en el admin panel como campo editable (alineación con app).
