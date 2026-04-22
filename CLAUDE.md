# Sin Cero — Sistema Agentic

> Contexto cargado automaticamente cuando Claude abre esta carpeta. Valido tanto en Cowork (primario) como en Claude Code (secundario).

## Que es Sin Cero

**Sin Cero** (marca: `Sin-Cero`, dominio `sin-cero.com`) es un negocio de comida saludable en **Quito, Ecuador**. Producto principal: **Enjarradas** — ensaladas gourmet servidas en jarro con componentes separados para mantener frescura.

**Tagline oficial:** "Comida 100% natural, sin ultra procesados".

**Modelo de ventas:**
- Pedido individual (enjarrada base $4.20 + extras configurables).
- Pack de 20 almuerzos ($87, ~$4.35/u) con codigo de uso `nombre-1234`.
- Programa de fidelidad: 25 compras = 1 almuerzo gratis.
- Incentivo de economia circular: descuento por retorno de envase.

**Canal principal:** WhatsApp **+593 98 464 5737**. Tambien `info@sin-cero.com`, Instagram `@sincero`, Facebook `Sin-Cero`.

**App de respaldo:** `lordiwa/sincero` (Vue 3 + Vite + Tailwind + Firebase) con formulario de pedidos, panel admin, PWA y notificaciones por WhatsApp/Email. Detalle en `knowledge/infraestructura-app.md`.

Este repositorio (sin-cero-agentic) es un **plugin** instalable de Cowork/Claude Code que asiste en la operacion del negocio: research, analisis, atencion al cliente, marketing, inventariado y soporte tecnico.

## Idioma

**Toda la comunicacion, contenido y respuestas son en espanol** salvo que el usuario pida lo contrario.

## Arquitectura: plugin Cowork

Este repo **es** un plugin de Cowork. Estructura:

```
sin-cero-agentic/
|-- .claude-plugin/
|   |-- plugin.json          <- manifest del plugin
|   `-- marketplace.json     <- marketplace para instalar desde GitHub
|-- .claude/
|   |-- settings.json        <- MCPs (WhatsApp placeholder), permisos
|   `-- settings.local.json  <- config local, gitignored
|-- skills/                  <- 5 especialistas auto-activables
|   |-- nutricionista/SKILL.md
|   |-- inventario/SKILL.md
|   |-- marketing/SKILL.md
|   |-- servicio-cliente/SKILL.md
|   `-- dev-sin-cero/SKILL.md
|-- knowledge/               <- modulos de dominio compartidos
|-- data/                    <- datos reales del negocio
|-- docs/                    <- arquitectura, fichas, roadmap
|-- CLAUDE.md                <- este archivo
|-- README.md
`-- QUICKSTART.md            <- guia de arranque con prompts listos para copiar
```

### Como se activan los especialistas

Cada skill en `skills/<nombre>/SKILL.md` tiene una descripcion que activa automaticamente al especialista cuando el usuario toca su dominio. No hace falta invocar a nadie por nombre — basta con preguntar "investiga articulos de salud y alimentacion" y la skill `nutricionista` se carga sola.

## Los cinco especialistas

| Especialista | Cuando activarlo |
|---|---|
| **`nutricionista`** | Composicion nutricional, propuestas de recetas saludables, investigacion de tendencias y evidencia cientifica, adaptacion a preferencias/restricciones alimentarias |
| **`inventario`** | Costos operativos, stock, proveedores, manejo de insumos, vinculo recetas<->insumos, procesos de produccion |
| **`marketing`** | Sondeo y estudio de mercado, generacion de contenido, estrategia de comunicacion, copywriting, benchmarks |
| **`servicio-cliente`** | Atencion al cliente, manejo de datos/historial, cobranzas, consultas sobre recetas, canal WhatsApp |
| **`dev-sin-cero`** | Infraestructura tecnica, integraciones, automatizacion, soporte del propio plugin y de la app |

## Regla de orquestacion

1. **Identifica el dominio** dominante de la pregunta.
2. **Deja que la skill correspondiente se active** (Cowork lo hace automaticamente segun la description). Si no se activo, lee el `SKILL.md` correspondiente y actua segun sus instrucciones.
3. **Si cruza varios dominios**, activa varias skills en secuencia o invoca agentes en paralelo (cuando aplique).
4. **Si es trivial o conversacional**, responde sin invocar ninguna.

## Knowledge compartido (modulos)

Varios especialistas comparten areas de conocimiento (ej: "Recetas" la usan Inventario, Nutricionista y Servicio al Cliente). Esos conocimientos viven como modulos markdown en `knowledge/` — **unica fuente de verdad** por dominio. Ver mapeo en [`knowledge/README.md`](knowledge/README.md).

## Datos del negocio

Los datos reales viven en `data/`:

- `data/recetas/` — una receta por archivo
- `data/inventario/` — stock, proveedores, costos
- `data/clientes/` — historial, preferencias, cobranzas (datos sensibles, escritura requiere confirmacion)
- `data/investigaciones/` — output de research (mercado, nutricion)

Las skills deben **leer `data/` antes de responder** cuando la pregunta dependa de informacion concreta del negocio.

## Convenciones

- **Nunca inventes** datos del negocio. Si falta informacion, pregunta al usuario o anotalo como TODO en el archivo correspondiente.
- **Actualiza `knowledge/<modulo>.md`** cuando aprendas algo nuevo del dominio que sea reutilizable por varios especialistas.
- **No escribas en `data/clientes/`** sin confirmar con el usuario (datos s