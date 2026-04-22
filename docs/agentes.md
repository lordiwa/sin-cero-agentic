# Ficha detallada de cada especialista

Sin Cero tiene **5 especialistas**. Cada uno vive como una **skill** en `skills/<nombre>/SKILL.md`. Las skills se activan automaticamente en Cowork cuando la descripcion coincide con la pregunta del usuario.

---

## `nutricionista`

**Rol**: Criterio nutricional. Diseno y validacion de recetas, investigacion de tendencias y evidencia cientifica.

**Se activa cuando**:
- Se piden articulos/tendencias/evidencia sobre nutricion, salud, dietas, ingredientes.
- Se pide composicion nutricional de una receta.
- Se piden recetas con perfil especifico (alto en proteina, bajo en sodio, antiinflamatorio, alto en fibra).
- Se pide adaptar una receta a una restriccion (celiaco, vegano, keto, alergias).

**Knowledge que consume**: `recetas`, `factores-nutricionales`, `preferencias-alimentarias`.

**Datos que consulta**: `data/recetas/`, `data/investigaciones/`.

**Donde escribe**: `data/investigaciones/tendencias-<tema>-<fecha>.md` para research.

**Ejemplos de prompts**:
- "Investiga tendencias de dieta antiinflamatoria en 2026"
- "Es viable nutricionalmente una version sin gluten de este pastel?"
- "Cuantas calorias tiene el bowl mediterraneo?"

---

## `inventario`

**Rol**: Gestor de stock, costos y produccion operativa.

**Se activa cuando**:
- Se pide costo de producir un plato/receta.
- Se pregunta por disponibilidad de insumos.
- Se habla de mermas, rendimientos, planificacion de compras.
- Se evalua viabilidad operativa de una receta nueva.

**Knowledge que consume**: `recetas`, `procesos-produccion`, `costos-operativos`.

**Datos que consulta**: `data/inventario/`, `data/recetas/`.

**Ejemplos de prompts**:
- "Calcula el costo unitario del bowl mediterraneo"
- "Con el stock actual cuantas porciones podemos producir esta semana?"

---

## `marketing`

**Rol**: Estrategia, contenido, sondeos de mercado.

**Se activa cuando**:
- Se pide analisis de competencia.
- Se pide generar copy/posts/captions.
- Se pide estrategia de comunicacion o posicionamiento.
- Se pide benchmark de precio.

**Knowledge que consume**: `marketing-estrategia`, `recetas`, `preferencias-alimentarias`, `templates-mensajes`.

**Datos que consulta / escribe**: `data/investigaciones/`.

**Ejemplos de prompts**:
- "Analiza 3 competidores de comida saludable a domicilio en mi zona"
- "Escribi 3 captions para Instagram para la nueva receta X"

---

## `servicio-cliente`

**Rol**: Cara del negocio frente al cliente. Atencion, historial, cobranzas, WhatsApp.

**Se activa cuando**:
- Se pide responder mensajes de clientes.
- Se pide recomendar segun historial/preferencias.
- Se pide recordatorio de cobranza.
- Se analizan quejas o feedback.

**Knowledge que consume**: `clientes`, `recetas`, `factores-nutricionales`, `preferencias-alimentarias`, `procesos-servicio`, `templates-mensajes`.

**Datos que consulta**: `data/clientes/`, `data/recetas/`. **Escrituras a `data/clientes/` requieren confirmacion humana.**

**Ejemplos de prompts**:
- "Respondele este mensaje al cliente Ana: 'tienen algo sin lactosa?'"
- "Genera los recordatorios de cobro de los clientes con saldo pendiente"

---

## `dev-sin-cero`

**Rol**: Ingeniero del sistema. Infraestructura, integraciones, automatizacion, mantenimiento del plugin mismo.

**Se activa cuando**:
- Se pide configurar MCPs (WhatsApp, otros).
- Se pide crear/ajustar schedulers, hooks, automatizaciones.
- Hay un debug tecnico.
- Se pide modificar el plugin (skills, settings, manifest).

**Herramientas**: todas (Read, Write, Edit, Grep, Glob, Bash, WebSearch, WebFetch).

**Knowledge que consume**: `infraestructura-app`, `procesos-produccion`, `procesos-servicio`.

**Ejemplos de prompts**:
- "Configura el MCP de WhatsApp siguiendo lharries/whatsapp-mcp"
- "Programa un scheduled task que avise cada lunes el stock bajo"
- "Agrega una nueva skill `contable` al plugin"

---

## Cuando NO activar especialistas

Para preguntas conversacionales o triviales, responde sin activar ninguno. Activar un especialista tiene un costo (tokens, latencia) — usalo cuando realmente aporta criterio o conocimiento que vale el viaje.

## Mantenimiento de skills

- Cada SKILL.md tiene frontmatter YAML con `name`, `description`, `license` (minimo).
- La `description` es lo que determina si la skill se activa. Debe incluir triggers positivos ("Usar cuando...") y negativos ("NO usar para...").
- Si agregas o modificas una skill, bumpea la version en `.claude-plugin/plugin.json` y `.claude-plugin/marketplace.json`.
- Para validar que una skill esta bien formada, `dev-sin-cero` puede correr `quick_validate.py` del skill-creator.
