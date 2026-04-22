# Sin Cero — Sistema Agentic

Sistema multi-especialista para asistir en la gestion y administracion del negocio **Sin Cero** (comida/nutricion). Este repositorio es un **plugin instalable de Cowork** (primario) y tambien funciona como proyecto de Claude Code (secundario).

## Que incluye

Cinco especialistas que se activan automaticamente segun el tema de la pregunta:

| Especialista | Se activa cuando preguntas por |
|---|---|
| **nutricionista** | Composicion nutricional, recetas saludables, research de tendencias y evidencia |
| **inventario** | Stock, costos, proveedores, produccion |
| **marketing** | Sondeos de mercado, contenido, estrategia, benchmarks |
| **servicio-cliente** | Atencion, cobranzas, WhatsApp, historial de clientes |
| **dev-sin-cero** | Infraestructura tecnica, MCPs, automatizacion, mantenimiento del plugin |

Detalle en [`docs/agentes.md`](docs/agentes.md).

## Instalacion en Cowork (recomendado)

Ver [`docs/instalacion-plugin.md`](docs/instalacion-plugin.md) para el paso-a-paso completo con todas las opciones.

**TL;DR - desde GitHub (UI de Cowork):**

1. Abrir Cowork -> **Customize** -> **+** -> **Add marketplace from GitHub**.
2. Pegar: `https://github.com/lordiwa/sin-cero-agentic`
3. Confirmar la instalacion del plugin `sin-cero`.
4. Reiniciar la sesion. Las 5 skills quedan disponibles y se activan solas.

**TL;DR - via CLI de Claude Code:**

```bash
claude plugin add github:lordiwa/sin-cero-agentic
```

## Uso en Claude Code (alternativo)

1. Instalar [Claude Code](https://claude.com/claude-code).
2. Abrir esta carpeta en terminal: `cd sin-cero-agentic && claude`.
3. Claude Code autodescubre los subagentes en `.claude/agents/`. Verificar con `/agents`.

## Estructura del repo

```
sin-cero-agentic/
|-- .claude-plugin/plugin.json  <- manifest del plugin Cowork
|-- .claude/
|   |-- agents/                 <- subagentes (Claude Code)
|   `-- settings.json           <- MCPs, permisos
|-- skills/                     <- skills auto-activables (Cowork)
|-- knowledge/                  <- modulos de dominio compartidos
|-- data/                       <- datos reales del negocio
|-- docs/                       <- arquitectura, fichas, roadmap
|-- CLAUDE.md                   <- contexto siempre cargado
`-- README.md
```

## Como invocar especialistas

**Automatico (Cowork)** — la skill se activa sola por el contenido de la pregunta:
> "investiga articulos de salud y alimentacion" -> activa `nutricionista`
> "cuanto nos cuesta el bowl mediterraneo" -> activa `inventario`
> "escribi un caption de Instagram sobre la receta X" -> activa `marketing`

**Explicito (Claude Code)**:
> "@nutricionista investiga tendencias de dieta antiinflamatoria"
> "@inventario calcula el costo unitario de la receta X"

## Estado actual

- Plugin Cowork configurado y 5 skills creadas.
- Subagentes Claude Code preservados para compatibilidad.
- Pendiente: dueno completa `knowledge/*.md` con conocimiento real y `data/` con datos reales.
- Pendiente: configurar credenciales del MCP de WhatsApp (ver `.claude/settings.json`).

## Roadmap y proximos pasos

Ver [`docs/roadmap.md`](docs/roadmap.md).

Proximos pasos sugeridos:
1. Editar [`CLAUDE.md`](CLAUDE.md) y completar descripcion del producto Sin Cero.
2. Llenar [`knowledge/marketing-estrategia.md`](knowledge/marketing-estrategia.md) con voz de marca.
3. Definir el formato canonico de receta y cliente en `knowledge/recetas.md` y `knowledge/clientes.md`.
4. Configurar el MCP de WhatsApp (ver instrucciones en `.claude/settings.json`).
