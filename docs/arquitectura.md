# Arquitectura del sistema agentic Sin Cero

## Vista general

```
              +------------------------------+
              |  Cowork / Claude Code        |
              |  (orquestador principal)     |
              |  Lee CLAUDE.md al conectar   |
              |  la carpeta del proyecto     |
              +--------------+---------------+
                             |
              auto-trigger por description
                             |
        +----------+---------+---------+----------+------------+
        v          v                   v          v            v
  +---------+ +--------------+ +----------+ +--------------+ +-------------+
  |inventa- | |nutricionista | |marketing | |servicio-     | |dev-sin-cero |
  |rio      | |              | |          | |cliente       | |             |
  +----+----+ +------+-------+ +----+-----+ +------+-------+ +------+------+
       |             |              |              |                |
       |    skills/<nombre>/SKILL.md (Cowork - primario)             |
       |    .claude/agents/<nombre>.md (Claude Code - secundario)   |
       v             v              v              v                v
  +------------------------------------------------------------------+
  |                 knowledge/ (modulos compartidos)                 |
  |   recetas . procesos-produccion . procesos-servicio .            |
  |   factores-nutricionales . preferencias-alimentarias .           |
  |   costos-operativos . clientes . marketing-estrategia .          |
  |   infraestructura-app                                            |
  +------------------------------------------------------------------+
                             |
                             v
  +------------------------------------------------------------------+
  |                       data/ (datos reales)                       |
  |   recetas/   inventario/   clientes/   investigaciones/          |
  +------------------------------------------------------------------+
```

## Decisiones de diseno

### 1. Plugin Cowork-first

El repo **es** un plugin de Cowork: hay un `.claude-plugin/plugin.json` en la raiz y los especialistas viven en `skills/<nombre>/SKILL.md`. Cowork autodescubre las skills por la description del frontmatter y las activa sin que el usuario tenga que invocarlas con `@`.

Por que: el objetivo del sistema es asistir gestion y administracion del negocio — no programacion. Cowork esta disenado para ese uso y tiene el modelo de skills que encaja mejor que el de subagentes de Claude Code para invocacion implicita.

### 2. Agents de Claude Code preservados como respaldo

Se mantienen tambien los 5 especialistas en `.claude/agents/<nombre>.md` para que el proyecto siga funcionando si alguien lo abre directamente en Claude Code. No se duplica contenido crucial: ambos lados leen los mismos modulos de `knowledge/` y describen las mismas responsabilidades.

Regla: si editas una SKILL, actualiza el agent correspondiente (o viceversa) para evitar drift. `dev-sin-cero` es responsable de mantener la sincronia.

### 3. Knowledge como modulos markdown, no como skills ejecutables

Los "skills compartidos" que comparten varios especialistas (Recetas, Factores nutricionales, Procesos) son areas de conocimiento, no acciones. Viven como archivos `.md` en `knowledge/` — unica fuente de verdad por dominio. Cualquier skill o agente los lee con `Read` cuando los necesita.

Si en el futuro un modulo evoluciona a una accion ejecutable repetitiva (ej: "calcular costo de receta"), se promueve a skill propia dentro de `skills/` o a herramienta de un agente.

### 4. Datos reales separados del conocimiento

- `knowledge/` = **como pensar** sobre el dominio (procedimientos, vocabulario, criterios).
- `data/` = **que pasa** (recetas concretas, clientes concretos, costos concretos).

Esto permite versionar el conocimiento independiente de los datos y evita que los especialistas inventen datos cuando solo tienen criterios.

### 5. Permisos restrictivos sobre datos sensibles

`.claude/settings.json` exige confirmacion para escrituras a `data/clientes/` (datos personales). Lecturas se permiten libremente.

### 6. WhatsApp MCP como placeholder

El MCP de WhatsApp esta configurado como bloque deshabilitado en `settings.json` con comentarios explicativos. El dueno debe elegir implementacion (recomendada: `lharries/whatsapp-mcp`), hacer auth, y activar el bloque.

## Flujo de activacion tipico (Cowork)

**Ejemplo**: scheduled task corre con el prompt "investiga articulos de salud y alimentacion".

1. Cowork arranca la sesion, carga el plugin Sin Cero.
2. El prompt matches la `description` de la skill `nutricionista` (incluye el trigger "articulos de salud y alimentacion").
3. Cowork carga `skills/nutricionista/SKILL.md` como instrucciones activas.
4. La skill instruye leer `knowledge/factores-nutricionales.md`, `knowledge/recetas.md`, y buscar con WebSearch.
5. La skill genera el informe y lo guarda en `data/investigaciones/tendencias-nutricion-<fecha>.md`.
6. Sesion termina con el archivo persistido en el repo.

## Flujo alternativo (Claude Code)

1. Usuario abre la carpeta en Claude Code.
2. Claude Code lee `CLAUDE.md` y autodescubre `.claude/agents/`.
3. Usuario escribe `@nutricionista investiga tendencias de dieta antiinflamatoria` (o descripcion libre y Claude Code elige el agente).
4. Claude Code invoca el subagente, que opera en contexto aislado.
5. El agente guarda el informe en `data/investigaciones/`.

## Ciclo de vida del plugin

- Cambios menores (contenido de una skill/agent, modulo de knowledge): no requieren bump de version.
- Cambios en el contrato (agregar/quitar una skill, cambiar permisos): bump minor en `.claude-plugin/plugin.json`.
- Cambios breaking (renombrar plugin, cambiar estructura de carpetas): bump major.

`dev-sin-cero` es responsable de mantener el manifest y documentar cambios en el roadmap.
