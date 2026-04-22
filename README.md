# Sin Cero — Sistema Agentic

Sistema multi-especialista para asistir en la gestion y administracion del negocio **Sin-Cero**: enjarradas (ensaladas gourmet en jarro, "comida 100% natural, sin ultra procesados") en Quito, Ecuador. Pedidos por WhatsApp +593 98 464 5737 y en [sin-cero.com](https://sin-cero.com).

Este repositorio es un **plugin instalable de Cowork** (primario) y tambien funciona como proyecto de Claude Code (secundario). Complementa al repo de la app del negocio en [`lordiwa/sincero`](https://github.com/lordiwa/sincero).

> **Para empezar rapido:** [`QUICKSTART.md`](QUICKSTART.md) trae un prompt listo para copiar en cualquier sesion de Cowork que instala y configura todo automaticamente.

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

El plugin tambien es compatible con Claude Code: las skills se descubren como cualquier otro plugin. Instalacion identica:

```bash
claude plugin add github:lordiwa/sin-cero-agentic
```

## Estructura del repo

```
sin-cero-agentic/
|-- .claude-plugin/
|   |-- plugin.json             <- manifest del plugin
|   `-- marketplace.json        <- marketplace para instalar desde GitHub
|-- .claude/
|   `-- settings.json           <- MCPs (WhatsApp placeholder), permisos
|-- skills/                     <- 5 especialistas auto-activables
|-- knowledge/                  <- modulos de dominio compartidos
|-- data/                       <- datos reales del negocio
|-- docs/                       <- arquitectura, fichas, roadmap
|-- CLAUDE.md                   <- contexto siempre cargado
|-- README.md
`-- QUICKSTART.md               <- guia de arranque con prompts copy-paste
```

## Como invocar especialistas

Las skills se activan **solas** por el contenido de la pregunta. No hay que invocar a nadie por nombre:

> "investiga articulos de salud y alimentacion" -> activa `nutricionista`
> "cuanto nos cuesta el bowl mediterraneo" -> activa `inventario`
> "escribi un caption de Instagram sobre la receta X" -> activa `marketing`

Si querés forzar una skill concreta, mencionala explicitamente en el prompt: "usa la skill nutricionista para...".

## Estado actual

- Plugin Cowork configurado y 5 skills creadas.
- Subagentes Claude Code preservados para compatibilidad.
- Pendiente: dueno completa `knowledge/