# Instalacion del plugin Sin Cero

Esta guia explica como instalar el plugin Sin Cero para que los cinco especialistas (nutricionista, inventario, marketing, servicio-cliente, dev-sin-cero) se activen automaticamente en cualquier sesion — incluso en scheduled tasks.

Hay **tres formas** de instalarlo, ordenadas de mas simple a mas tecnica:

1. Desde GitHub en la UI de Cowork (recomendado para el dueno).
2. Desde GitHub via Claude Code CLI.
3. Desde una carpeta local (util para desarrollo del plugin mismo).

---

## Opcion 1: Instalar desde GitHub en Cowork (recomendado)

Este repo esta publicado en: **https://github.com/lordiwa/sin-cero-agentic**

Y configurado como marketplace (contiene `.claude-plugin/marketplace.json`), asi que Cowork lo instala con un click desde su UI:

1. Abrir la app de Cowork.
2. En la barra lateral izquierda, click en **Customize**.
3. Click en **+** y elegir **Add marketplace from GitHub**.
4. Pegar la URL: `https://github.com/lordiwa/sin-cero-agentic`
5. Cowork detecta automaticamente el plugin `sin-cero` y lo ofrece para instalar. Confirmar.
6. Reiniciar la sesion de Cowork. Las 5 skills (`nutricionista`, `inventario`, `marketing`, `servicio-cliente`, `dev-sin-cero`) quedan disponibles y se activan solas.

---

## Opcion 2: Instalar desde GitHub via Claude Code CLI

Si tenes Claude Code instalado:

```bash
# Instalar directamente (un solo paso)
claude plugin add github:lordiwa/sin-cero-agentic
```

O usando el flujo de marketplace (equivalente, mas explicito):

```bash
claude plugin marketplace add github:lordiwa/sin-cero-agentic
claude plugin install sin-cero@sin-cero
```

(El `@sin-cero` al final es el nombre del marketplace definido en `marketplace.json`.)

Verificar:

```bash
claude plugin list
```

---

## Opcion 3: Instalar desde carpeta local (desarrollo)

Si estas editando el plugin mismo y queres probar sin pushear a git:

```bash
claude plugin add "C:/Users/srpar/OneDrive/Documents/sin-cero-agentic"
```

O para una sesion aislada sin instalar permanentemente:

```bash
claude --plugin-dir "C:/Users/srpar/OneDrive/Documents/sin-cero-agentic"
```

---

## Conectar la carpeta del proyecto

Independiente de como instales el plugin, las skills necesitan acceso a la carpeta del proyecto para leer `knowledge/`, `data/` y escribir en `data/investigaciones/`. La primera vez:

1. En una sesion de Cowork, pedir: "Conecta la carpeta `C:\Users\srpar\OneDrive\Documents\sin-cero-agentic`".
2. Cowork pedira aprobar acceso una sola vez.
3. A partir de ahi, las skills usan rutas relativas a esa carpeta.

Para scheduled tasks, asegurate de que el prompt incluya `request_cowork_directory` con la ruta del proyecto si la carpeta podria no estar conectada al momento de correr la tarea.

---

## Verificar la instalacion

Abrir una sesion limpia de Cowork y pedir:

> "Lista las skills disponibles"

Deberias ver las 5 skills de Sin Cero: `nutricionista`, `inventario`, `marketing`, `servicio-cliente`, `dev-sin-cero`.

Prueba con un trigger simple:

> "investiga articulos de salud y alimentacion"

Si el plugin esta bien instalado, Cowork activa `nutricionista` automaticamente y sigue sus instrucciones (cargar contexto, investigar, guardar hallazgos en `data/investigaciones/`).

---

## Actualizar el plugin

Cada vez que pushees un nuevo commit al repo en GitHub, los usuarios pueden traer los cambios con:

```bash
claude plugin update sin-cero
```

O actualizar todo:

```bash
claude plugin update --all
```

**No hay auto-update**: los usuarios deben correr el comando explicitamente. Si uno usa la UI de Cowork, hay un boton equivalente en la seccion Customize.

---

## Publicar o actualizar el repo (flujo del dueno)

Para pushear cambios al repo `lordiwa/sin-cero-agentic`:

```bash
cd "C:/Users/srpar/OneDrive/Documents/sin-cero-agentic"

# primera vez
git init
git remote add origin https://github.com/lordiwa/sin-cero-agentic.git
git add .
git commit -m "v0.1.0 - plugin Cowork con 5 especialistas"
git branch -M main
git push -u origin main

# commits subsiguientes
git add .
git commit -m "mensaje descriptivo"
git push
```

**Antes de cada push**: revisa que no estes subiendo datos sensibles. El `.gitignore` ya excluye `data/clientes/**` y `settings.local.json`, pero confirma siempre con `git status`.

**Bumpear version**: si el cambio agrega/modifica skills o contrato del plugin, actualiza `version` en `.claude-plugin/plugin.json` Y en `.claude-plugin/marketplace.json` (ambos deben coincidir). Semver: `0.1.0` -> `0.2.0` para cambios menores, `1.0.0` para breaking.

---

## Desinstalar

```bash
claude plugin remove sin-cero
claude plugin marketplace remove sin-cero
```

Los archivos del repo local no se borran; solo se desregistra el plugin de Cowork/Claude Code.

---

## Troubleshooting

**"No veo las skills despues de instalar"** — reinicia Cowork por completo. Algunas versiones no recargan plugins en caliente.

**"La skill se activa pero no encuentra los archivos"** — conecta la carpeta del proyecto (ver seccion "Conectar la carpeta" arriba).

**"La skill no se activa automaticamente"** — revisa el campo `description` en `skills/<nombre>/SKILL.md`. Debe contener palabras clave cercanas a como el usuario formula la pregunta. La skill `dev-sin-cero` puede ayudar a optimizarla.

**"El repo es privado y Cowork no puede acceder"** — Cowork necesita credenciales de GitHub para repos privados. Ver la documentacion de Cowork sobre autenticacion de GitHub en Customize -> Settings.
