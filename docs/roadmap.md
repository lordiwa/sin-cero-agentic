# Roadmap del sistema agentic Sin Cero

## Fase 1 — Bootstrap & migracion a plugin Cowork (actual)

**Objetivo**: dejar el andamiaje listo para que los 5 especialistas asistan al dueno en research, analisis y operacion cotidiana, con Cowork como interfaz primaria.

**Estado**: Estructura creada y migrada a formato plugin. Pendiente que el usuario llene `knowledge/` y `data/` con contenido real del negocio.

**Hitos**:
- [x] Estructura de carpetas
- [x] CLAUDE.md raiz con vision global
- [x] 5 skills en `skills/<nombre>/SKILL.md` (Cowork, auto-activables)
- [x] `.claude-plugin/plugin.json` manifest del plugin
- [x] `.claude-plugin/marketplace.json` para instalacion desde GitHub
- [x] 10 modulos de conocimiento en `knowledge/` (incluye templates-mensajes)
- [x] Catalogo completo de ingredientes en `data/inventario/catalogo-ingredientes.md`
- [x] `settings.json` con permisos y placeholder de WhatsApp MCP
- [x] Documentacion de arquitectura e instalacion del plugin
- [x] QUICKSTART.md con prompts listos para copiar
- [ ] Instalar el plugin en Cowork del usuario y verificar triggers
- [ ] Importar primeras recetas reales a `data/recetas/`
- [ ] Importar primeros clientes a `data/clientes/`

---

## Fase 2 — Integracion con WhatsApp y automatizacion tentativa

**Objetivo**: conectar el canal real con clientes y empezar a automatizar tareas fisicas (inventariado, manejo de clientes).

**Hitos propuestos** (a confirmar/priorizar):
- [ ] Activar MCP de WhatsApp (`lharries/whatsapp-mcp` u otro)
  - `dev-sin-cero` ejecuta el setup, registra credenciales en lugar seguro
  - `servicio-cliente` empieza a redactar respuestas (envio manual al inicio)
- [ ] Scheduled task de inventario
  - Verificacion semanal del stock vs produccion planificada
  - Alerta al dueno si hay riesgo de quiebre
- [ ] Scheduled task de clientes
  - Recordatorios de cobranza programados
  - Deteccion de clientes "dormidos" y propuesta de reactivacion
- [ ] Hook `PreToolUse` que registre toda escritura a `data/clientes/` para auditoria
- [ ] Metricas y reporte semanal automatico (que se vendio, que quedo, quien compro)

**Notas importantes**:
- La automatizacion es **tentativa** — el dueno confirmo que aun no esta seguro del alcance.
- Antes de automatizar **respuestas al cliente**, validar manualmente durante un periodo (humano-en-el-loop).
- Los scheduled tasks se crean via la skill `schedule` de Cowork o via CLI de Claude Code. `dev-sin-cero` decide.

---

## Fase 3 — Maduracion (a definir)

Posibles direcciones segun evolucione el negocio:

- **Skill/agente contable** dedicado si la complejidad financiera lo justifica.
- **Aplicacion standalone con Claude Agent SDK** si se necesita correr logica fuera de Cowork/Claude Code (ej: bot 24/7 sin sesion interactiva).
- **MCPs adicionales** (sistema contable, pasarela de pago, logistica).
- **Dashboard** que consuma `data/` para vista humana.
- **Publicar el plugin** en una marketplace privada para replicarlo en otros negocios similares.

---

## Decisiones diferidas (parking lot)

- **Esquema canonico de receta**: a definir cuando el usuario llene `knowledge/recetas.md`.
- **Implementacion especifica del MCP WhatsApp**: a confirmar.
- **APIs externas**: pendiente identificar cuales necesitamos.
- **Segmentacion oficial de clientes**: a definir con marketing.
- **Consolidar skill y agent por especialista en una sola fuente**: evaluar si vale la pena generar uno a partir del otro automaticamente para evitar drift.
