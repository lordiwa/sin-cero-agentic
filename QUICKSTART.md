# QUICKSTART — Sin Cero en Cowork

Guia rapida: instalar el plugin Sin Cero desde cero en cualquier computadora con Cowork, y empezar a usar los 5 especialistas en minutos.

No necesitas saber programar. Cowork funciona con **prompts en lenguaje natural** — le pides cosas y el las hace.

---

## Paso 1: Instalar Cowork

Descargar e instalar Claude Desktop con Cowork activado desde [claude.com/cowork](https://claude.com/cowork). Iniciar sesion con tu cuenta.

---

## Paso 2: Instalar el plugin Sin Cero (3 opciones)

### Opcion A (la mas simple): Copiar este prompt en Cowork

Pega el siguiente texto en una sesion nueva de Cowork y envia. Cowork se encargara del resto, preguntando lo que necesite:

```
Quiero instalar el plugin "Sin Cero" desde el repositorio
https://github.com/lordiwa/sin-cero-agentic. Es un plugin que contiene
5 especialistas (nutricionista, inventario, marketing, servicio-cliente,
dev-sin-cero) para la gestion de un negocio de comida en Quito.

Por favor guiame para:
1. Registrar el marketplace de GitHub en Cowork (UI Customize + Add marketplace).
2. Instalar el plugin sin-cero.
3. Conectar mi carpeta del proyecto si ya la tengo en
   "C:/Users/srpar/OneDrive/Documents/sin-cero-agentic", o ayudarme a
   clonarla si aun no la tengo.
4. Verificar que las 5 skills esten activas.
5. Darme un ejemplo rapido con cada especialista.

Cuando termines el setup, muestrame un menu de lo que puedo hacer con
cada especialista.
```

Cowork interpretara este prompt y te guiara por cada paso.

### Opcion B: UI de Cowork (clicks)

1. Abrir Cowork.
2. Barra lateral izquierda -> **Customize**.
3. Click en **+** y elegir **Add marketplace from GitHub**.
4. Pegar: `https://github.com/lordiwa/sin-cero-agentic`
5. Confirmar la instalacion del plugin `sin-cero`.
6. Reiniciar la sesion.

### Opcion C: CLI de Claude Code

Si tenes Claude Code instalado en terminal:

```bash
claude plugin add github:lordiwa/sin-cero-agentic
```

---

## Paso 3: Conectar la carpeta del proyecto

Las skills necesitan leer archivos del proyecto (recetas, clientes, investigaciones). En una sesion de Cowork, pedi:

```
Conecta la carpeta C:/Users/srpar/OneDrive/Documents/sin-cero-agentic
```

Cowork abrira un dialogo de aprobacion. Click en "Permitir" una sola vez.

Si aun no tenes el repo clonado localmente:

```
Clona https://github.com/lordiwa/sin-cero-agentic a mi carpeta de Documentos
y conecta la carpeta cuando termine.
```

---

## Paso 4: Verificar que funciona

En una sesion nueva pedile a Cowork:

```
Lista las skills disponibles del plugin Sin Cero y describeme que hace cada una.
```

Deberias ver: `nutricionista`, `inventario`, `marketing`, `servicio-cliente`, `dev-sin-cero`.

Prueba con un trigger simple:

```
Investiga articulos de salud y alimentacion
```

Si el plugin esta bien instalado, Cowork activa la skill `nutricionista` automaticamente, carga el contexto del negocio y empieza a investigar.

---

## Como funciona Cowork (5 cosas clave)

1. **Pides en lenguaje natural.** No hay comandos crípticos. Decile a Cowork que hacer como le dirias a un asistente humano.

2. **Las skills se activan solas.** No tenes que decir "usa la skill nutricionista". Si preguntas "que tendencias hay en dietas mediterraneas", Cowork detecta que es nutricion y carga esa skill automaticamente.

3. **Scheduled tasks corren en segundo plano.** Podes decirle "investiga tendencias de nutricion todos los dias a las 19:00" y Cowork lo agenda. Los resultados quedan guardados en `data/investigaciones/`.

4. **La carpeta conectada es memoria persistente.** Todo lo que las skills escriben en `data/` queda en tu disco, accesible sesion tras sesion.

5. **Slash commands** (opcional, para usuarios avanzados). Empezar una linea con `/` abre un menu de comandos del sistema (ver `/help`).

---

## Prompts tipicos por especialista

Copia cualquiera de estos en Cowork y enviaselo. La skill correcta se activa sola.

### Nutricionista
```
Investiga que dice la evidencia cientifica sobre la dieta antiinflamatoria en 2026.
Guarda el informe en data/investigaciones/.
```
```
Proponeme 3 enjarradas altas en proteina y bajas en sodio con ingredientes
del catalogo actual de Sin Cero.
```
```
Cuantas calorias y proteinas tiene una enjarrada con atun fresco, quinoa,
6 verduras y aderezo de tahini?
```
```
Adapta la enjarrada mediterranea para un cliente celiaco. Marca las
afirmaciones clinicas que requieran validacion de dietista.
```

### Inventario
```
Calcula el costo de producir una enjarrada mediterranea con
pollo desmechado, quinoa, 6 verduras y aderezo de tahini.
```
```
Revisa el catalogo de ingredientes en data/inventario/ y dime cuales
tienen mayor margen sobre el precio extra.
```
```
Si tenemos un pedido B2B de 50 enjarradas para el viernes,
que insumos deberia revisar el stock?
```

### Marketing
```
Haz un sondeo de competencia de comida saludable en Quito, enfocado
en delivery por WhatsApp. Guarda los hallazgos en data/investigaciones/.
```
```
Escribi 3 captions de Instagram para la nueva enjarrada con yuca cocida
y tahini. Respeta la voz de marca en knowledge/marketing-estrategia.md.
```
```
Analisa competidores directos en Ecuador que no publican sus macros
y proponeme una estrategia de diferenciacion.
```

### Servicio al Cliente
```
Un cliente pregunta: "Tienen algo sin gluten?". Respondemelo con el
tono de marca y opciones concretas del catalogo actual.
```
```
Redacta el recordatorio de almuerzo gratis para el cliente Ana
(telefono 593987654321) que acaba de llegar a 25 compras.
```
```
Un cliente nuevo quiere saber como funcionan los packs de 20 almuerzos.
Responde en formato WhatsApp listo para enviar.
```

### DEV-Sin Cero
```
Configura el MCP de WhatsApp siguiendo la implementacion lharries/whatsapp-mcp.
Mostrame los pasos y pidete las credenciales que necesites.
```
```
Programa un scheduled task que sincronice los datos de Firestore a
data/clientes/ cada lunes a las 08:00.
```
```
Agrega una nueva skill llamada "contable" al plugin para manejar
facturacion B2B. Bumpea la version del plugin.
```
```
Revisa las reglas de Firestore del proyecto sincero y sugerime mejoras
de seguridad para las colecciones clientes y packs.
```

---

## Flujos combinados (varios especialistas a la vez)

```
Disena una nueva enjarrada estacional de invierno:
- Que el nutricionista proponga perfil nutricional e ingredientes.
- Que inventario calcule el costo.
- Que marketing sugiera nombre y copy para redes.
Entregame el resultado consolidado.
```

Cowork activa las 3 skills, las ejecuta en orden (o en paralelo cuando son independientes) y te entrega una respuesta integrada.

---

## Scheduled tasks ya configurados

El plugin viene con dos tareas recurrentes que podes activar:

| Tarea | Cron | Que hace |
|---|---|---|
| `investigacion-de-articulos-de-salud-y-alimentacion` | Diario 19:00 | Research nutricional via skill nutricionista |
| `sondeo-mercado-comida-saludable-ecuador` | Diario 19:00 | Sondeo de mercado via skill marketing |

Para verlos:
```
Lista los scheduled tasks activos
```

Para pausarlos o cambiarlos:
```
Pausa el scheduled task de investigacion de nutricion
```

---

## Comandos utiles de Cowork (para copiar)

```
Lista las skills disponibles
```
```
Lista los scheduled tasks
```
```
Lista los artifacts
```
```
Muestrame la estructura del plugin sin-cero
```
```
Que archivos hay en data/investigaciones/
```
```
Ejecuta una revision del plugin: validacion de SKILL.md, consistencia
entre manifest y marketplace
```

---

## Actualizar el plugin

Cuando haya una nueva version en GitHub:

```
Actualiza el plugin sin-cero a la ultima version
```

O via CLI:
```bash
claude plugin update sin-cero
```

---

## Troubleshooting

**"No pasa nada cuando pregunto sobre nutricion"**
La skill no se activo. Verifica que el plugin este instalado:
```
Muestrame los plugins instalados y confirma que sin-cero este activo
```

**"La skill se activa pero no encuentra los archivos"**
No conectaste la carpeta del proyecto:
```
Conecta la carpeta C:/Users/srpar/OneDrive/Documents/sin-cero-agentic
```

**"El scheduled task falla"**
Los scheduled tasks corren sin supervision. Si requieren conectar una carpeta
o aprobar un MCP, fallan en silencio. Revisa la ultima corrida con:
```
Mostrame el resultado del ultimo run del scheduled task
investigacion-de-articulos-de-salud-y-alimentacion
```

**"Quiero desinstalar el plugin"**
```
Desinstala el plugin sin-cero
```

---

## Para aprender mas

- Arquitectura del sistema: [`docs/arquitectura.md`](docs/arquitectura.md)
- Ficha detallada de cada especialista: [`docs/agentes.md`](docs/agentes.md)
- Roadmap y fases: [`docs/roadmap.md`](docs/roadmap.md)
- Instalacion avanzada: [`docs/instalacion-plugin.md`](docs/instalacion-plugin.md)
- Contexto del negocio (lo que saben los especialistas): [`CLAUDE.md`](CLAUDE.md)
- Catalogo de ingredientes: [`data/inventario/catalogo-ingredientes.md`](data/inventario/catalogo-ingredientes.md)
- Templates de mensajes: [`knowledge/templates-mensajes.md`](knowledge/templates-mensajes.md)

---

## Resumen

1. Instalar Cowork.
2. Pegar el prompt de Opcion A arriba -> Cowork hace todo lo demas.
3. Preguntar lo que necesitas en lenguaje natural. La skill correcta se activa sola.
4. Los outputs quedan guardados en `data/` dentro de la carpeta del proyecto.
