# Knowledge — Módulos de conocimiento compartidos

Cada archivo `.md` en este directorio es un **módulo de conocimiento** sobre un dominio del negocio Sin Cero. Las skills los leen on-demand cuando necesitan contexto sobre ese dominio.

> **Por qué markdown y no skills ejecutables:** estos son áreas de conocimiento (no acciones). Un solo archivo por dominio = una única fuente de verdad, sin duplicación entre agentes.

## Mapeo módulo ↔ agentes

| Módulo | Agentes que lo leen |
|---|---|
| [`recetas.md`](recetas.md) | Inventario · Nutricionista · Servicio al Cliente · Marketing (consulta) |
| [`procesos-produccion.md`](procesos-produccion.md) | Inventario · DEV-Sin Cero |
| [`procesos-servicio.md`](procesos-servicio.md) | Servicio al Cliente · DEV-Sin Cero |
| [`factores-nutricionales.md`](factores-nutricionales.md) | Nutricionista · Servicio al Cliente |
| [`preferencias-alimentarias.md`](preferencias-alimentarias.md) | Nutricionista · Servicio al Cliente · Marketing (consulta) |
| [`infraestructura-app.md`](infraestructura-app.md) | DEV-Sin Cero |
| [`costos-operativos.md`](costos-operativos.md) | Inventario |
| [`clientes.md`](clientes.md) | Servicio al Cliente |
| [`marketing-estrategia.md`](marketing-estrategia.md) | Marketing |
| [`templates-mensajes.md`](templates-mensajes.md) | Servicio al Cliente · Marketing · DEV-Sin Cero |

## Cómo agregar un módulo nuevo

1. Crea `knowledge/<nombre>.md` siguiendo la plantilla estándar (ver cualquier módulo existente).
2. Si lo van a usar varios especialistas, **edita esta tabla** y **edita los archivos `skills/<nombre>/SKILL.md`** de los especialistas consumidores para que sepan que existe.
3. No dupliques contenido entre módulos. Si dos módulos comparten algo, refactoriza a un tercer módulo o referencia con un link.

## Plantilla de un módulo

```markdown
# <Nombre del módulo>

> Para qué sirve este módulo en una línea.

## Agentes que lo consumen
- Lista

## Vocabulario clave
- Términos del dominio con definición corta

## Datos clave
- Hechos centrales que cualquier agente debe saber

## Procedimientos típicos
- Pasos / heurísticas / decisiones recurrentes

## Referencias a `data/`
- Qué archivos en data/ contienen los datos crudos del dominio

## TODO
- Qué falta capturar
```

## Estado actual

Todos los módulos están como **plantillas esqueleto**. El dueño de Sin Cero los llena progresivamente con conocimiento real del negocio.
