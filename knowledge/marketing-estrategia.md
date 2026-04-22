# Estrategia de marketing

> Voz de marca, segmentos, canales y posicionamiento de Sin Cero.

## Agentes que lo consumen
- `marketing` (único consumidor activo)

## Identidad de marca (verificado en app)

### Nombre y claim
- **Marca:** Sin-Cero (con guión en uso visual; el dominio es `sin-cero.com`).
- **Tagline oficial:** "Comida 100% natural, sin ultra procesados".
- **Producto insignia:** Enjarrada — ensalada gourmet servida en jarro con componentes separados para frescura.

### Paleta (verificado en código)
- **Verde principal:** `#54BF8B` (Tailwind `brand-green`)
- **Verde claro:** `#6dd199` (Tailwind `brand-green-light`)
- **Verde oscuro:** `#0b2718` (Tailwind `brand-dark`)
- **Fondo PWA:** `#F9FAF4` (crema muy claro, no blanco puro)
- **Theme color PWA:** `#55bf8c`

Nota: hay leve inconsistencia entre el valor del Tailwind (#54BF8B) y el del manifest PWA (#55bf8c). Pedir al dueño que defina el canónico y alinear en un solo cambio.

### Tipografías
- **Principal:** Inter (Google Fonts).
- **Futura:** Gordita (cuando esté licenciada/disponible).

### Activos visuales
- Logo: `public/logo.jpg` del repo de la app.
- Hero: `public/jarra.jpg` — el jarro es el ícono del producto.

## Voz de marca

- **Adjetivos:** cercana, fresca, directa, optimista, sin moralismo.
- **Lo que NO es:** condescendiente, alarmista, "fitness hardcore", culpógena, elitista.
- **Registro:** tuteo informal en redes y WhatsApp 1-a-1.
- **Emojis aceptados:** 👋 🎉 ❤️ ✅ 📅 ⏰ ⭐ (puntuales, no decorativos).

### Copy real del producto (verificado en `views/HomePage.vue` y `components/order/`)

Estos son los textos que el cliente ve en la app. Son el anclaje del tono oficial:

**Headings:**
- "Sugerencia del Chef"
- "Elige tu forma favorita de armar tu enjarrada"
- "Elige cada ingrediente a tu gusto"
- "Usa tu pack de 20 almuerzos"
- "¿Cómo funciona?"

**Instrucciones:**
- "Marca los ingredientes que NO te gustan y nosotros armamos tu enjarrada perfecta"
- "Solo dinos qué ingredientes prefieres evitar. Nuestro chef seleccionará la mejor combinación con todo lo demás."

**Selectores:**
- "1. Elige 6 verduras" (Extras: $0.30 c/u - Máximo 8 total)
- "2. Elige proteínas" (Máximo 2)
- "3. Elige carbohidratos" (1° gratis, extras: +$0.90)
- "4. Elige carbohidratos especiales (opcional)"
- "5. Elige aderezos"
- "6. Elige 1 fruta"

**Delivery:**
- "Información de Entrega"
- "Horario: Lunes a Viernes, 12:00 PM - 15:00 PM"
- "Pedidos hasta las 21:30 del día anterior"
- "Ⓘ El costo de delivery se calculará según tu ubicación y será confirmado vía WhatsApp"

**Fidelidad:**
- "¡Tienes almuerzo(s) gratis!"
- "${n} más para tu próximo gratis"
- "${n} frascos más para un almuerzo gratis"
- "¡Listo para tu almuerzo gratis! Habla con nosotros."

**Pack:**
- "Pack activo: ${codigo}"
- "${n} créditos disponibles"
- "Este pedido usará 1 crédito de tu pack."
- "Ingresa tu código de pack"

**Validación:**
- "El nombre es requerido"
- "El teléfono es requerido"
- "La dirección es requerida"

### Reglas para copy nuevo
- Imperativo suave ("Elige", "Marca"), no "Debes".
- Cifras duras cuando existen ("Máximo 8", "$0.30 c/u"), sin redondear.
- Emojis puntuales, nunca decorativos.
- Mayúsculas solo al inicio o en *inline bold* para resaltar ("MI ENJARRADA" solo en el template de WhatsApp).

## Segmentos objetivo

### Primario (según sondeo 2026-04-21 y app)
Profesionales urbanos 25-45 años en **Quito, Ecuador**. Tienen ingresos medios-altos, valoran conveniencia (delivery), rechazan dietas restrictivas, quieren "comer sano sin sacrificar sabor". Sensibles a precio en el rango $3-$5/almuerzo individual y $25-$50 por planes de 5-10 días.

### Secundarios
- **B2B corporativo** — almuerzos ejecutivos, contratos recurrentes en oficinas de Quito. Segmento poco explotado por la competencia.
- **Clientes con packs** — prepagan 20 almuerzos, buscan previsibilidad y ahorro marginal.
- **Fit / deportistas** — interesados en la etiqueta "alta en proteína".

### Anti-segmento
- Clientes con presupuesto sub-$3/almuerzo (buffet, fast-casual) — Sin Cero no compite en ese tier.
- Clientes que quieren "premium extremo" (>$30/almuerzo) — el mercado ecuatoriano rechaza ese rango (lección Munch).
- Dietas restrictivas muy nicho sin disposición a customizar (nuestro flujo requiere elegir ingredientes).

## Canales (verificado en app + sondeo)

| Canal | Rol | Estado |
|---|---|---|
| **Web app (sin-cero.com)** | Conversión — formulario de pedido que termina en WhatsApp | Activo |
| **WhatsApp (+593 98 464 5737)** | Cierre de pedido + atención | Activo (canal principal) |
| **Instagram @sincero** | Awareness + captación | Activo |
| **Facebook Sin-Cero** | Awareness + alcance 35+ | Activo |
| **Email `info@sin-cero.com`** | B2B + notificaciones | Activo |
| **TikTok** | Diferenciación futura (competencia casi nula en Ecuador) | TODO evaluar |
| **Rappi / Uber Eats** | Alcance incremental, margen comprimido | TODO evaluar |

## Posicionamiento

### Propuesta de valor en una frase
> "Enjarradas customizables, 100% naturales y sin ultraprocesados — con ingredientes frescos que tú eliges y entrega por WhatsApp."

### Diferenciadores vs. competencia (del sondeo 2026-04-21)
1. **Customización radical** — 6 categorías elegibles por pedido; ninguna otra marca ecuatoriana lo ofrece con este nivel de granularidad.
2. **Producto físico distintivo** — el jarro con componentes separados es visualmente icónico.
3. **Programa de fidelidad transparente** — 25 compras = 1 gratis, sin letra chica.
4. **Packs prepagados** — incentivo a compra recurrente con ahorro modesto.
5. **Gap oportunidad: macros públicos** — ningún competidor publica macros/calorías. Publicar en Instagram sería diferenciador inmediato.
6. **Gap oportunidad: sostenibilidad** — el retorno de envase con descuento es economía circular; amplificar en comunicación.

### Pricing tier
- **Premium-accesible** ($4.20 - $6 por enjarrada totalmente customizada) — rango poco ocupado en Quito según el sondeo.
- Competencia directa en tier buffet ($3 y menos) → no competimos ahí.
- Competencia tier premium (>$15) → tampoco es donde jugamos.

## Competencia
- TODO: 3-5 competidores directos con qué hacen bien y qué hacen mal.
- TODO: link a `data/investigaciones/competencia-*.md` cuando existan análisis.

## Calendario y campañas
- TODO: campañas recurrentes (lanzamientos estacionales, promos).
- TODO: ciclo creativo (briefing → contenido → publicación → medición).

## Métricas
- TODO: KPIs de marketing (alcance, CTR, CAC, LTV, etc.).

## Referencias a `data/`
- `data/investigaciones/` — sondeos, análisis competitivos, benchmarks.

## Notas de sondeos

### Sondeo: Comida saludable en Ecuador — 2026-04-21
**Fuente:** `data/investigaciones/sondeo-comida-saludable-ecuador-2026-04-21.md`

#### Hallazgos para el posicionamiento de Sin Cero

**Competidores principales identificados (12 empresas):**
- Healthy Life Dietas (Quito, 12+ años, especializada en dietas Scarsdale/Fitness)
- Simply Healthy (Quito, énfasis en ecuatoriana saludable)
- Comesano (Quito, 3.6K followers IG, planes variados: keto, detox, vegano)
- Eat Well / Dietas Express (Guayaquil, 45K followers IG, planes 5–20 días USD 25–195)
- Lunchero (Guayaquil, coaching nutricional integrado, profesionales certificados)
- Dietas a Domicilio (Guayaquil, 21 años trayectoria, reputación establecida)
- Nutri Delivery, ServiFood, DELIDIET, Olive Comida Saludable (secundarias)
- Fitness Power Food (Cuenca, casi única opción especializada)

**Segmento de consumidor ecuatoriano de comida saludable:**
- Profesionales urbanos (25–45 años) en Quito y Guayaquil
- Predisposición media-alta a pagar por conveniencia (delivery) + nutrición
- Sensibles a precio: competencia observable en rango USD 25–50 por 5–10 días; USD 3–5 por almuerzo individual
- Buscan "sin sacrificar sabor", aunque mercado ecuatoriano rechaza premium extremo (caso Munch)

**Canales dominantes:**
1. Instagram (principal para captación; 3K–45K followers en competidores activos)
2. WhatsApp (pedidos, soporte directo)
3. Agregadores: Rappi, Uber Eats (menos usado por meal-prep puro, más por multiservicios)
4. Facebook y TikTok (secundarios, en crecimiento)
5. Ningún competidor tiene app propia; operaciones mayormente manuales

**Gaps de mercado observados:**
1. **Comida saludable ecuatoriana explícita:** Escaso énfasis en raíces locales + nutrición moderna
2. **Datos nutricionales públicos:** Ningún competidor publica macros/calorías; oportunidad de diferenciación
3. **Segmento "lifestyle" (no solo dieta/pérdida peso):** Mayoría posicionada en pérdida de peso; poco en "vivir bien saludablemente"
4. **Coaching ligero integrado:** Lunchero lo hace caro; oportunidad de oferta ligera (WhatsApp mensual)
5. **Sostenibilidad ambiental:** Ningún competidor enfatiza; tendencia 2026 global
6. **Cuenca y ciudades secundarias:** Pocas opciones; Fitness Power Food casi única

**Tendencias 2026 observadas:**
- Proteína como pilar central (high-protein diets, snacks proteicos)
- "Smart foods" y conveniencia (bebidas funcionales, probióticos)
- Salud mental y emocional (alimentos anti-estrés, ánimo)
- Nutrición personalizada con IA/apps (incipiente en Ecuador)
- Salud digestiva (fermentados, kéfir)
- Transparencia y clean label

#### Implicaciones estratégicas

**Ventajas de Sin Cero:**
1. Posicionamiento diferenciador posible: "sabor ecuatoriano + nutrición moderna"
2. Oportunidad en transparencia (publicar datos nutricionales en redes)
3. Segmento B2B corporativo (almuerzo ejecutivo) es escalable
4. Rango de precio "premium-accesible" (USD 15–25/almuerzo) está poco ocupado
5. Cuenca es mercado abierto

**Riesgos:**
1. Competencia establecida con reputación (Healthy Life 12 años, Dietas a Domicilio 21 años)
2. Agregadores (Rappi, Uber Eats) creciente oferta de opciones saludables
3. Rendimientos ajustados si compite en precio con buffet ($3) o Eat Well (USD 5–10)
4. Validación crítica: mercado ecuatoriano NO paga premium extremo (lección Munch)
5. Saturación en Instagram; diferenciación visual/narrativa debe ser excepcional

#### Próximas fases recomendadas
1. **Validación (2–3 sem):** Entrevistas a 3–5 clientes por ciudad, preguntas sobre gaps e intención de compra
2. **Modelo (4–6 sem):** Definir segmento inicial, estructura de costos, precio final, city de lanzamiento
3. **Escalado (2–3 m):** Expansión geográfica, diversificación de planes, integración de datos nutricionales en redes, considerar coaching ligero

---

## Aprendizajes de sondeo 2026-04-21

### Perfil del consumidor ecuatoriano de comida saludable

**Demográfico:**
- Profesionales urbanos, 25-45 años, principalmente en Quito y Guayaquil
- Predisposición media-alta a pagar por conveniencia (delivery) + nutrición
- Sensibles a precio: validan competencia en rango USD 25-50 por 5-10 días; USD 3-5 por almuerzo individual

**Psicográfico:**
- Buscan "sin sacrificar sabor": rechazo a dietas restrictivas o insulsas
- Valoran transparencia: ingredientes simples, datos nutricionales públicos, origen de productos
- Mercado ecuatoriano rechaza premium extremo (lección Munch: pivotó de "comida saludable premium" a hamburguesas)
- Emergente: interés en sostenibilidad ambiental (dispuestos a pagar 5-10% más por packaging ecológico)

**Comportamiento de compra:**
- Canal primario: Instagram (3K-45K followers en competidores activos)
- Soporte y pedidos: WhatsApp (cálido, directo)
- Agregadores (Rappi, Uber Eats): secundarios para meal-prep puro; usados cuando operan modelo multi-servicio

### Rangos de precio típicos

- **Almuerzo individual:** USD 3-5 (buffet/fast-casual) a USD 15-25 (premium meal-prep)
- **Planes 5-10 días:** USD 25-50 (accesible) a USD 100-195 (premium Lunchero/Healthy Life)
- **Coaching integrado:** +USD 30-50/mes (Lunchero modelo premium)
- **Packaging ecológico:** Consumidores pagan 5-10% de premium
- **Oportunidad:** rango USD 15-25/almuerzo está poco ocupado (gap entre ServiFood $8-15 y Lunchero $35+)

### Canales dominantes

1. **Instagram:** Principal para captación; 3K-45K followers en competidores. Engagement mediante recetas, transformaciones, testimonios
2. **WhatsApp:** Soporte, pedidos, relación 1-a-1; tono cercano, profesional
3. **Rappi/Uber Eats:** Creciente pero no dominante para meal-prep puro; margen comprimido por comisión
4. **Facebook:** Secundario (grupos comunitarios, audiencia 35+)
5. **TikTok:** Emergente, casi no explotado por competencia actual
6. **Apps propias:** Ningún competidor local las tiene; oportunidad futura

### Tendencias de mensaje 2026

**Dominantes en mercado actual:**
- "Comida saludable sin sacrificar sabor"
- Pérdida de peso / fitness / dieta
- Conveniencia (delivery, ahorro tiempo)
- Profesional / ejecutivo

**Emergentes (oportunidad de diferenciación):**
- Transparencia radical: datos nutricionales públicos (macros, calorías, ingredientes)
- Raíces locales: "sabor ecuatoriano genuino + nutrición moderna"
- Sostenibilidad ambiental: empaques ecológicos con storytelling
- Salud mental/emocional: "comida que mejora el ánimo", anti-estrés
- Salud digestiva: fermentados, probióticos, nutrición preventiva (vs. solo pérdida peso)
- Envejecimiento saludable y nutrición preventiva

### Tendencias globales 2026 aplicables a Ecuador

1. **Proteína como pilar:** high-protein diets, snacks proteicos; aumento proyectado 14% en proteína vegetal hasta 2035
2. **Clean label:** consumidores rechazan azúcar excesivo, sodio, aditivos artificiales
3. **Smart foods:** bebidas funcionales, ready-to-drink, probióticos
4. **Personalización con IA:** Tipti y Planeta Fit son pioneras en Ecuador; tendencia creciente
5. **Envejecimiento saludable:** nutrición como prevención, no solo dieta

### Gaps de mercado validados

1. **Datos nutricionales públicos:** NINGÚN competidor publica macros/calorías. Diferenciador inmediato
2. **Comida saludable ecuatoriana:** Escaso énfasis. Oportunidad: quinua, chocho, tubérculos andinos + nutrición
3. **Coaching accesible:** Lunchero es premium ($35-100+/mes). Gap: WhatsApp coaching $10-20/mes
4. **Segmento "lifestyle" no-dieta:** 90% posicionado en "pérdida peso". Gap: "vivir bien saludablemente"
5. **Sostenibilidad:** Ningún competidor enfatiza packaging ecológico. Consumidores pagan más por ello
6. **B2B corporativo:** Almuerzo ejecutivo saludable poco explotado
7. **IA/Personalización:** Incipiente; oportunidad de early adoption o asociación
8. **Cuenca y ciudades secundarias:** Fitness Power Food casi única; mercado abierto

### Ventajas competitivas potenciales para Sin Cero

1. **Diferenciador ecuatoriano:** "Sabor ecuatoriano + nutrición moderna" (vs. genérico)
2. **Transparencia radical:** Publicar macros en Instagram reels; oportunidad visual
3. **Segmento B2B corporativo:** Contratos recurrentes con empresas (menos saturado)
4. **Rango "premium-accesible":** USD 15-25/almuerzo (gap poco ocupado)
5. **Coaching ligero:** WhatsApp seguimiento mensual; modelo escalable
6. **Empaques ecológicos:** Diferenciador + willingness to pay +5-10%
7. **IA/Personalización:** Asociarse con Tipti o Planeta Fit; futuro-proof brand
8. **Mercados secundarios:** Cuenca sin competencia especializada

### Riesgos validados

1. **Competencia establecida:** Healthy Life (12 años), Dietas a Domicilio (21 años), Comesano (comunidad activa)
2. **Agregadores saturados:** Rappi y Uber Eats con creciente oferta saludable; margen comprimido
3. **Rechazo al premium extremo:** Lección Munch: mercado ecuatoriano no paga >USD 30/almuerzo en promedio
4. **Saturación en Instagram:** +10 cuentas similares; diferenciación visual/narrativa crítica
5. **Costo de operación:** Producción + delivery en Quito/Guayaquil = capital inicial significativo
6. **Regulación sanitaria:** ARCSA, licencias, permisos en aumento

### Recomendaciones de posicionamiento inicial

**Si apunta a B2C fitness:** Diferenciarse por transparencia (macros públicos) + sostenibilidad + coach accesible
**Si apunta a B2B corporativo:** Énfasis en conveniencia, consistencia, profesionalismo, recepción en oficina
**Si apunta a ciudades secundarias (Cuenca):** Énfasis local + presencia, servicio personalizado, diferenciador geográfico

## TODO
- Definir voz de marca y posicionamiento.
- Establecer KPIs y baselines.
- Validar con 5-10 clientes potenciales antes de MVP.
