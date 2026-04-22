# Infraestructura de la app

> Stack técnico, repositorios, deploys, integraciones y monitoreo de Sin Cero.

## Agentes que lo consumen
- `dev-sin-cero` (único consumidor activo)

## Vocabulario clave
- TODO: nombres internos de servicios/repos.

## Componentes
- **App principal**: TODO (web, móvil, ambos, stack).
- **Backend / API**: TODO (lenguaje, framework, hosting).
- **Base de datos**: TODO.
- **Integraciones externas**:
  - **WhatsApp**: vía MCP (placeholder en `.claude/settings.json`, implementación recomendada `lharries/whatsapp-mcp`).
  - TODO: pasarela de pago.
  - TODO: sistema contable.
  - TODO: logística/entregas.

## Deploys
- TODO: ambientes (dev, staging, prod).
- TODO: pipeline CI/CD.
- TODO: dónde corre (cloud provider, VPS, on-prem).

## Secrets y credenciales
- TODO: dónde se guardan (gestor de secretos, variables de entorno, etc.).
- **Nunca commitear secrets a este repo.** Si DEV necesita uno, pedir al usuario por canal seguro.

## Monitoreo y alertas
- TODO: qué se monitorea, dónde, quién recibe alertas.

## Sistema agentic mismo
- Subagentes en `.claude/agents/`.
- Knowledge en `knowledge/`.
- Settings en `.claude/settings.json`.
- GSD (gestor de fases) instalado global en `C:\Users\srpar\.claude\get-shit-done\` v1.34.2.

## Referencias a `data/`
- N/A (datos operativos del negocio, no técnicos).

## TODO
- Documentar el stack actual.
- Documentar el plan de backup y recovery.
