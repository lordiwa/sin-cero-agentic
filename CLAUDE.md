# Sin Cero — Sistema Agentic

> Contexto cargado automaticamente cuando Claude abre esta carpeta. Valido tanto en Cowork (primario) como en Claude Code (secundario).

## Que es Sin Cero

**Sin Cero** es un negocio de comida/nutricion. Este repositorio es un **plugin** instalable de Cowork/Claude Code que asiste en la operacion del negocio: research, analisis, atencion al cliente, marketing, inventariado y soporte tecnico.

> **Pendiente por el dueno**: descripcion exacta del producto/servicio, propuesta de valor, segmento objetivo. Actualizar esta seccion cuando este definido.

## Idioma

**Toda la comunicacion, contenido y respuestas son en espanol** salvo que el usuario pida lo contrario.

## Arquitectura: plugin Cowork-first

Este repo **es** un plugin de Cowork. Estructura:

```
sin-cero-agentic/
|-- .claude-plugin/
|   `-- plugin.json          <- manifest del plugin
|-- .claude/
|   |-- agents/              <- subagentes (compat. Claude Code)
|   |-- settings.json        <- MCPs, permisos
|   `-- settings.local.json
|-- skills/                  <- skills auto-activables (Cowork nativo)
|   |-- nutricionista/SKILL.md
|   |-- inventario/SKILL.md
|   |-- marketing/SKILL.md
|   |-- servicio-cliente/SKILL.md
|   `-- dev-sin-cero/SKILL.md
|-- knowledge/               <- modulos de dominio compartidos
|-- data/                    <- datos reales del negocio
|-- docs/                    <- arquitectura, fichas, roadmap
|-- CLAUDE.md                <- este archivo
`-- README.md
```

### Como se activan los especialistas

**En Cowork (primario):** cada skill en `skills/<nombre>/SKILL.md` tiene una descripcion que activa automaticamente al especialista cuando el usuario toca su dominio. No hace falta decir `@nutricionista` — basta con preguntar "investiga articulos de salud y alimentacion" y la skill `nutricionista` se carga.

**En Claude Code (secundario):** los mismos especialistas existen tambien como subagentes en `.claude/agents/<nombre>.md` y se invocan con `@nombre` o via la Task tool. Las skills en `skills/` y los agentes en `.claude/agents/` contienen las mismas responsabilidades, solo cambia el mecanismo de activacion.

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
- **No escribas en `data/clientes/`** sin confirmar con el usuario (datos sensibles).
- **Canal WhatsApp**: solo el especialista `servicio-cliente` lo usa. La integracion esta en `.claude/settings.json` como placeholder hasta conectar credenciales.

## Documentacion interna

- [`docs/arquitectura.md`](docs/arquitectura.md) — diagrama del sistema y decisiones de diseno
- [`docs/agentes.md`](docs/agentes.md) — ficha detallada por especialista
- [`docs/roadmap.md`](docs/roadmap.md) — fases del proyecto
- [`docs/instalacion-plugin.md`](docs/instalacion-plugin.md) — como instalar el plugin en Cowork
