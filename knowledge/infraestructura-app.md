# Infraestructura de la app

> Stack técnico, repositorios, deploys, integraciones y monitoreo de Sin Cero.

## Agentes que lo consumen
- `dev-sin-cero` (único consumidor activo)

## Repositorios

| Repo | Propósito | Ruta local |
|---|---|---|
| **`lordiwa/sincero`** | App web Vue 3 del negocio (formulario de pedidos + panel admin) | `C:\Users\srpar\WebstormProjects\sincero` |
| **`lordiwa/sin-cero-agentic`** | Plugin agentic (este repo) — asistente operativo | `C:\Users\srpar\OneDrive\Documents\sin-cero-agentic` |

## Stack de la app web (verificado)

- **Framework:** Vue 3 (Composition API)
- **Build / dev server:** Vite 7
- **Estilos:** Tailwind CSS 3
- **Routing:** Vue Router 4
- **Backend-as-a-service:** Firebase
  - **Firestore** (base de datos principal — colecciones `orders`, `clients`, `settings`, `packs`, según servicios en `src/services/`)
  - **Authentication** (email/password + Google OAuth; usado en admin panel y en programa de fidelidad)
  - **Hosting** (preparado; alternativa Netlify / Vercel)
- **Notificaciones:**
  - **WhatsApp:** integración directa vía `wa.me` API (genera deep-link con mensaje formateado).
  - **Email:** **EmailJS** desde frontend (opcional).
  - **reCAPTCHA v3** para protección antispam.
- **Utilidades:**
  - `@vueuse/core` — composables útiles.
  - `date-fns` — manejo de fechas (expiración de packs, etc.).
- **PWA:** service worker (`public/sw.js`) con cache de estáticos y soporte offline básico.
- **Testing:** Vitest + @vue/test-utils + happy-dom.

## Estructura del código (de `src/`)

- `views/` — HomePage (pedido) + AdminView.
- `components/order/` — selectores paso-a-paso (VegetableSelector, SingleChoiceSelector, CustomerInfo, OrderSummary).
- `components/admin/` — dashboard (AdminPanel, ClientesManager, PacksManager, PedidosManager, MenuItemManager).
- `services/` — capa de lógica:
  - `firebaseService.js` — inicialización y wrappers.
  - `orderService.js` — formatea pedido, genera mensaje de WhatsApp, escribe en Firestore.
  - `emailService.js` — envío vía EmailJS.
  - `packService.js` — transacciones atómicas sobre créditos de pack.
  - `fidelityService.js` — conteo 25 → 1 gratis, historial.
- `store/index.js` — estado global con Composition API (menú, precios, pedido actual).
- `router/index.js` — rutas + meta tags.

## Variables de entorno (según `.env.example` en la app)

Requeridas:
- `VITE_FIREBASE_API_KEY`, `VITE_FIREBASE_AUTH_DOMAIN`, `VITE_FIREBASE_PROJECT_ID`, `VITE_FIREBASE_STORAGE_BUCKET`, `VITE_FIREBASE_MESSAGING_SENDER_ID`, `VITE_FIREBASE_APP_ID`
- `VITE_BUSINESS_PHONE=593984645737`
- `VITE_BUSINESS_EMAIL=info@sin-cero.com`
- `VITE_APP_NAME="Sin-Cero"`
- `VITE_APP_DESCRIPTION="Comida 100% natural, sin ultra procesados"`
- `VITE_APP_URL=https://sin-cero.com`

Opcionales (EmailJS):
- `VITE_EMAILJS_SERVICE_ID`, `VITE_EMAILJS_TEMPLATE_ID`, `VITE_EMAILJS_PUBLIC_KEY`

**Nunca commitear** los valores reales de estas variables a ningún repo. Usar `.env.local` o gestor de secretos.

## Identidad visual (verificado en `tailwind.config.cjs` y `public/manifest.json`)

**Colores en Tailwind:**
- `brand-green`: `#54BF8B` (verde principal — OJO: el README dice `#55bf8c`; el código es `#54BF8B`)
- `brand-green-light`: `#6dd199`
- `brand-dark`: `#0b2718`

**Colores en PWA manifest:**
- `theme_color`: `#55bf8c`
- `background_color`: `#F9FAF4`

**Tipografía Tailwind:**
- `font-gordita`: `['Gordita', 'Inter', 'sans-serif']` (fallback a Inter)

**Border radius Tailwind:**
- DEFAULT: `2px`, sm: `1px`, md: `3px`, lg: `4px` (minimalista, no redondeado)

**Activos:**
- Logo: `public/logo.jpg`
- Imagen hero: `public/jarra.jpg` (el jarro es el ícono del producto)
- Dominio: `sin-cero.com`
- PWA name: "Sin-Cero", short_name: "Sin-Cero", display: `standalone`, start_url: `/`.

## Deploys (opciones mencionadas en README)

Tres caminos soportados:
- **Firebase Hosting** — natural dado que la BD es Firebase.
- **Netlify** — build dist/ y subir.
- **Vercel** — `vercel --prod`.

TODO: confirmar cuál está activo actualmente.

## Firestore: colecciones y schema (verificado)

### `packs`
```yaml
codigo: "juan-4527"
nombreComprador: ""
creditosIniciales: 20
creditosDisponibles: 17
creditosUsados: 3
fechaCompra: <Timestamp>
fechaExpiracion: <Timestamp>
tipoExpiracion: "creditos_compra" | "almuerzos"
activo: true
precioTotal: 87.00
historialUso: [{ fecha, tipo, creditosAfectados, pedidoId, extrasAdicionales }]
```

### `clientes`
```yaml
telefono: "593987654321"  # PK
nombre: ""
direccion: ""
almuerzosTotales: 0
almuerzosParaSiguienteGratis: 0
almuerzosGratisDisponibles: 0
frascosParaSiguienteGratis: 0
frascosTotalesDevueltos: 0
totalGastado: 0.00
historialPedidos: [...]
createdAt: <Timestamp>
```

### `orders`
```yaml
cliente: ""
telefono: ""
direccion: ""
items: { verduras: [], proteina: "", carbohidrato: "", aderezo: "", fruta: "" }
extras: { verdurasExtra: n, ... }
total: 0.00
fecha: <Timestamp>
hora: ""
status: "recibido" | "listo" | "entregado"
```

### `settings` (inferido)
Configuración del menú y precios que el admin panel edita. Espejo de lo hardcoded en `store/index.js` como defaults.

**Reglas Firestore (del README):**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /settings/{document} {
      allow read, write: if request.auth != null;
    }
    match /orders/{document} {
      allow read, write: if request.auth != null;
    }
  }
}
```
TODO: agregar reglas para `clientes` y `packs`.

## Rutas de la app (verificado en `router/index.js`)

| Path | Componente | Auth |
|---|---|---|
| `/` | HomePage | No |
| `/admin` | AdminView | Sí |
| `/admin/pedido/:id` | AdminOrderDetail | Sí (modal login) |
| `*` | → `/` | — |

## Capacidades del admin panel (verificado en `components/admin/`)

| Tab | Entidad | Operaciones |
|---|---|---|
| **Menú y Precios** | Verduras | agregar, listar, toggle disponible, eliminar |
| **Menú y Precios** | Proteínas | agregar, listar, toggle, eliminar |
| **Menú y Precios** | Carbohidratos | agregar, listar, editar nombre/precio/toggle, eliminar |
| **Menú y Precios** | Carbs especiales | agregar, listar, editar, eliminar |
| **Menú y Precios** | Fibras | agregar, listar, toggle, eliminar |
| **Menú y Precios** | Aderezos | agregar, listar, toggle, eliminar |
| **Menú y Precios** | Prices object | editar base, extras por categoría |
| **Pedidos (hoy)** | Orders | listar, cambiar status (recibido → listo → entregado), ver detalle |
| **Historial** | Orders históricos | listar, ver detalle |
| **Clientes** | Fidelity | crear, buscar, editar nombre/dirección, añadir bono manual, ver historial, eliminar |
| **Packs** | Packs | crear, listar, toggle activo, renovar (+20 créditos), refund, ver historial, eliminar |

## Integración con el sistema agentic

### WhatsApp MCP (pendiente)

El agente `servicio-cliente` necesita un MCP que lea/escriba mensajes de WhatsApp. Actualmente el bloque está como **placeholder** en `.claude/settings.json`. Implementación recomendada: `lharries/whatsapp-mcp` con auth por QR.

### Firebase MCP (potencial)

Idealmente, el agente `servicio-cliente` e `inventario` leen directamente de Firestore para:
- Responder "¿cuántos pedidos tengo hoy?" (agente inventario).
- Responder "¿qué ordena usualmente el cliente X?" (agente servicio-cliente).
- Sumarizar ventas de la semana (agente marketing con foco interno).

TODO: evaluar si hay un MCP de Firebase/Firestore o si conviene hacer un export periódico a `data/` vía un scheduled task.

### Sincronización de `knowledge/` ↔ admin panel

El admin panel de la app permite editar menú, precios, ingredientes, packs y clientes en Firestore. Esos son los mismos dominios que los módulos de `knowledge/`. Hay un riesgo de drift: los especialistas leen `knowledge/` y `data/` locales, mientras que la app escribe en Firestore.

Opciones a evaluar:
- **Pull periódico:** scheduled task diario que sincroniza Firestore → `data/` (agente `dev-sin-cero`).
- **Push mediante MCP:** los especialistas escriben ajustes acordados en Firestore (riesgo; requiere permisos).
- **Source-of-truth única:** definir explícitamente que Firestore manda para datos transaccionales y `knowledge/` manda para procedimientos/criterios.

Recomendación inicial: **Firestore = fuente de datos transaccionales**, `knowledge/` = **fuente de criterios y procedimientos**. `data/` sirve como snapshot de `data/clientes/` y `data/recetas/` para consulta offline por los especialistas.

## Secrets y credenciales

- Nunca commitear secrets a este repo (`.gitignore` excluye `settings.local.json`).
- Credenciales de Firebase, EmailJS, reCAPTCHA viven en `.env` del repo de la app (también fuera de git).
- Para operaciones que requieren credenciales, `dev-sin-cero` pide al usuario por canal seguro.

## Sistema agentic mismo

- Plugin manifest: `.claude-plugin/plugin.json`.
- Marketplace: `.claude-plugin/marketplace.json`.
- Skills (auto-activables): `skills/<nombre>/SKILL.md` (5 especialistas).
- Knowledge: `knowledge/`.
- Settings: `.claude/settings.json`.
- GSD (gestor de fases, instalado global): `C:\Users\srpar\.claude\get-shit-done\`.

## TODO

- Confirmar cuál target de deploy de la app está activo (Firebase Hosting, Netlify, Vercel).
- Decidir la estrategia de sincronización Firestore ↔ `data/`.
- Documentar plan de backup (exports de Firestore, frecuencia, retención).
- Documentar monitoreo (Firebase Analytics? Sentry? Logs de errores?).
- Evaluar MCP de Firebase o wrapper propio.
