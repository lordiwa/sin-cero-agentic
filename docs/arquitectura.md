# Arquitectura del sistema agentic Sin Cero

## Vista general

```
              +------------------------------+
              |  Cowork (orquestador)        |
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
       |    skills/<nombre>/SKILL.md (auto-activable)                |
       v             v              v              v                v
  +------------------------------------------------------------------+
  |                 knowledge/ (modulos compartidos)                 |
  |   recetas . procesos-produccion . procesos-servicio .            |
  |   factores-nutricionales . preferencias-alimentarias .           |
  |   costos-operativos . clientes . marketing-estrategia .          |
  |   infraestructura-app . templates-mensajes                       |
  +------------------------------------------------------------------+
                             |
                             v
  +------------------------------------------------------------------+
  |                       data/ (datos reales)                       |
  |   recetas/   inventario/   clientes/   investigaciones/          |
  +------------------------------------------------------------------+
```

## Decisiones de diseno

### 1. Plugin Cowork con skills auto-activables

El repo **es** un plugin de Cowork: hay un `.claude-plugin/plugin.json` en la raiz y los especialistas viven como skills en `skills/<nombre>/SKILL.md`. Cowork autodescubre las skills por la description del frontmatter y las activa sin que el usuario tenga que invocarlas por nombre.

Por que: el objetivo del sistema es asistir gestion y administracion del negocio — no programacion. Cowork esta disenado para ese uso y el modelo de skills encaja mejor que el de subagentes de Claude Code para invocacion implicita.

### 2. Repo instalable como marketplace

El archivo `.claude-plugin/marketplace.json` permite que el repo funcione simultaneamente como plugin y como marketplace de un solo plugin. Asi se puede instalar desde Cowork (UI `Customize` -> `Add marketplace from GitHub`) o desde Claude Code CLI (`claude plugin add github:lordiwa/sin-cero-agentic`).

### 3. Knowledge como modulos markdown, no como skills ejecutables

Los "skills compartidos" que comparten varios especialistas (Recetas, Factores nutricionales, Procesos) son areas de conocimiento, no acciones. Viven como archivos `.md` en `knowledge/` — unica fuente de verdad por dominio. Cualquier skill los lee con `Read` cuando los necesita.

Si en el futuro un modulo evoluciona a una accion ejecutable repetitiva (ej: "calcular costo de receta"), se promueve a skill propia dentro de `skills/`.

### 4. Datos reales separados del conocimiento

- `knowledge/` = **como pensar** sobre el dominio (procedimientos, vocabulario, criterios).
- `data/` = **que pasa** (recetas concretas, clientes concretos, costos concretos).

Esto permite versionar el conocimiento independiente de los datos y evita que los especialistas inventen datos cuando solo tienen criterios.

### 5. Permisos restrictivos sobre datos sensibles

`.claude/settings.json` exige confirmacion para escrituras a `data/clientes/` (datos personales). Lecturas se permiten libremente.

### 6. WhatsApp MCP como placeholder

El MCP de WhatsApp esta configurado como bloque deshabilitado en `settings.json` con comentarios explicativos. El dueno debe elegir implementacion (recomendada: `lharries/whatsapp-mcp`), hacer auth, y activar el bloque. Mientras tanto, la skill `servicio-cliente` genera los mensajes como texto listo para copy/paste.

### 7. Sincronizacion con la app Sin Cero

El plugin complementa al repo de la app `lordiwa/sincero` (Vue 3 + Firebase). Reglas de source-of-truth:

- **Firestore** manda para datos transaccionales (pedidos, clientes, packs, settings).
- **`knowledge/`** manda para procedimientos, criterios y templates.
- **`data/`** sirve como snapshot offline para consulta por las skills.

Pull periodico de Firestore a `data/` es una responsabilidad de `dev-sin-cero` (via scheduled task).

## Flujo de activacion tipico

**Ejemplo**: scheduled task corre con el prompt "investiga articulos de salud y alimentacion".

1. Cowork arranca la sesion, carga el plugin Sin Cero.
2. El prompt matches la `description` de la skill `nutricionista` (incluye el trigger "articulos de salud y alimentacion").
3. Cowork carga `skills/nutricionista/SKILL.md` como instrucciones activas.
4. La skill instruye leer `knowledge/factores-nutricionales.md`, `knowledge/recetas.md`, y buscar con WebSearch.
5. La skill genera el informe y lo guarda en `data/investigaciones/tendencias-nutricion-<fecha>.md`.
6. Sesion termina con el archivo persistido en el repo.

## Ciclo de vida del plugin

- Cambios menores (contenido de una skill, modulo de knowledge): no requieren bump de version.
- Cambios en el contrato (agregar/quitar una skill, cambiar permisos): bump minor en `.claude-plugin/plugin.json` Y en `.claude-plugin/marketplace.json` (deben coincidir).
- Cambios breaking (renombrar plugin, cambiar estructura de carpetas): bump major.

`dev-sin-cero` es responsable de mantener el manifest y documentar cambios en el roadmap.
