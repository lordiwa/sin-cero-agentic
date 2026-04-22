# Procesos de producción

> Cómo se produce físicamente cada enjarrada de Sin Cero.

## Agentes que lo consumen
- `inventario` (para planificar compras y rendimientos)
- `dev-sin-cero` (para automatizar agendamiento, registro y seguimiento)

## Vocabulario clave
- **Enjarrada**: la unidad de producto (ensalada servida en jarro con componentes separados).
- **Jarro**: el envase físico — reutilizable, con política de descuento por retorno.
- **Lote / batch**: preparación simultánea de componentes para múltiples enjarradas.
- **Componente**: cada ingrediente preparado (vegetales picados, proteína cocida, carbo cocido, aderezo hecho).
- **Mise en place**: preparación previa de componentes para ensamble rápido.
- **Punto crítico**: etapa que requiere control (cadena de frío, higiene, separación de alérgenos).

## Concepto de producción (inferido del producto)

Sin Cero produce por **componentes separados** y **ensamble al momento del pedido** para mantener frescura:

- Los vegetales se pican y se reservan refrigerados (posiblemente por lote matutino).
- La proteína se cocina en batch (pollo a la plancha, atún, lentejas, etc.).
- Los carbohidratos se cocinan en batch (quinoa, arroz integral, papa, etc.).
- Los aderezos se preparan en batch (frascos pequeños o al momento).
- Al llegar un pedido, se monta el jarro con las capas pedidas y el aderezo aparte.

Esto permite:
- Minimizar merma (los componentes duran más separados que mezclados).
- Customización sin tiempo extra significativo.
- Escalar produciendo más lotes, no más recetas distintas.

## Datos clave a formalizar

- TODO: dónde se produce (cocina propia en Quito, tercerizada, cocina comercial compartida).
- TODO: capacidad diaria/semanal (enjarradas máximas producibles en un día).
- TODO: equipamiento crítico (refrigeración, superficies de corte, planchas, freidoras).
- TODO: staffing (cuántas personas, qué roles).
- TODO: horario de producción vs. horario de despacho.

## Procedimientos típicos

### Recepción de insumos
- Verificar cadena de frío (temperaturas de llegada).
- Registrar lote y fecha en `data/inventario/recepciones/<fecha>.md` (TODO definir formato).
- Separar por categoría para mise en place.

### Mise en place diaria
1. Limpiar superficies y utensilios (protocolo a documentar).
2. Lavar y desinfectar vegetales.
3. Picar vegetales según especificación de receta.
4. Cocinar proteínas y carbohidratos en lotes del día.
5. Preparar aderezos frescos.
6. Guardar componentes etiquetados con fecha/hora en refrigeración.

### Ensamble de enjarrada
1. Tomar jarro limpio de stock.
2. Montar por capas (pesados/firmes abajo, frágiles arriba).
3. Aderezo en envase separado.
4. Etiquetar con pedido y cliente.
5. Entregar al mensajero o al cliente.

### Control de calidad
- Inspección visual antes del despacho.
- TODO: checklist formal.
- TODO: política de muestras retenidas para trazabilidad sanitaria.

### Manejo de mermas
- Registrar mermas al final del día en `data/inventario/mermas/<fecha>.md`.
- Identificar patrones (siempre sobra X → ajustar compra).

### Cadena de frío
- Refrigeración ≤ 4°C para componentes preparados.
- Jarros montados que no se despachen en X horas: TODO política (recuperar componentes reutilizables, desechar los no).

## KPIs de producción (propuestos)

- Enjarradas producidas / día.
- Merma en % sobre compra (por categoría).
- Tiempo promedio de ensamble por pedido.
- Incidentes de calidad / semana.
- % de entregas on-time.

## Referencias a `data/`
- `data/inventario/` — stock y movimientos.
- `data/inventario/produccion/<fecha>.md` — registros diarios (TODO formalizar).

## TODO
- Documentar SOPs por etapa con fotos/videos.
- Definir SKUs internos de insumos y de componentes preparados.
- Diseñar formato de registro diario de producción (cuánto se preparó de cada componente).
- Definir plan de contingencia (quiebre de stock, falla de refrigerador, etc.).
- Documentar permisos y licencias sanitarias (ARCSA en Ecuador).
