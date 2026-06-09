# Design: Reposicionamiento de landing — arquitectura de información y plan de componentes

## Architecture approach
Reposición de **contenido + arquitectura de información** sobre componentes Astro existentes. NO es rediseño visual. Patrón: componentes presentacionales auto-contenidos (cada `.astro` declara su data en el frontmatter como arrays locales y la mapea en el markup). Se respeta ese patrón existente — la nueva sección lo replica al pie de la letra. CERO CSS nuevo: se reutilizan exclusivamente los tokens y utilidades ya en uso (`bark-900`, `cream-50/100/200`, `ember-500/600`, `gold-300/500`, `font-display`, `grain`, `glow`, `glass`, `reveal`, `text-molten`, `rounded-card`, `text-muted`, `text-bark-700`).

Narrativa nueva (orden de secciones): **Hero → Diferencial → Servicios → Proceso → Contacto**. La tecnología deja de ser titular y baja a PRUEBA del criterio (tags/subtítulos). El método (Proceso) es la garantía que reemplaza a la prueba social inexistente.

## Component-level plan (real paths)

| File | Action | Cambio concreto |
|------|--------|-----------------|
| `src/pages/index.astro` | Modified | Importar `Differentiator` y slotearlo entre `<Hero />` y `<Services />`. Orden final: Nav, main(Hero, Differentiator, Services, Process, Contact), Footer. |
| `src/components/Hero.astro` | Modified | Reescribir titular/subtítulo + **eliminar la stats strip por completo** (borrar el array `stats` del frontmatter y el bloque `<!-- stats strip -->` del markup). El Hero termina después de los botones de CTA. |
| `src/components/Differentiator.astro` | **New** | 4 pilares. Único componente nuevo. |
| `src/components/Services.astro` | Modified | 6 cards técnicas → 3-4 cards de resultado; tech sobrevive en `tags`. |
| `src/components/Process.astro` | Modified | Promover como protagonista (eyebrow/intro que lo enmarca como garantía). |
| `src/components/Nav.astro` | Modified | Agregar link `#diferencial` en el array `links`, primero en el orden. |
| `src/components/Contact.astro` | Modified (mínimo/opcional) | Reforzar "diagnóstico gratis". Form `action="#"` queda OUT OF SCOPE. |
| `src/components/Footer.astro` | Modified (menor) | Reflejar nueva estructura si lista secciones. |

## Decision 1 — Hero rewrite + eliminar la stats strip
- **Titular**: liderar con el problema/resultado de negocio + el doble diferencial "IA + seguridad a medida", SIN jerga técnica. Mantener el patrón `<span class="text-molten">…</span>` para resaltar el diferencial.
- **Eyebrow** (línea 23): cambiar "Inteligencia artificial aplicada" por algo que combine IA + seguridad.
- **Subtítulo**: del listado de features (agentes, fine-tuning, RAG) → al criterio de negocio.
- **Stats strip — DECISIÓN DEL USUARIO: ELIMINAR POR COMPLETO.** El cliente está arrancando y prefiere un Hero más limpio que vaya directo al problema de negocio + CTA, sin métricas inventadas ni real estate de relleno. Acción concreta: borrar el array `stats` (frontmatter, líneas 2-6) Y el bloque `<!-- stats strip -->` renderizado (líneas 62-75) de `src/components/Hero.astro`. El Hero queda: glow blobs → eyebrow → h1 → subtítulo → botones de CTA, y CIERRA ahí (la `</div>` del contenedor y `</section>` quedan justo después del bloque de botones).
- Rechazado (explícitamente): repurposear la grilla con mini-señales de criterio — el usuario lo descartó por preferir un Hero más sobrio sin invención de datos. Rechazado: dejar los valores tech actuales (`A medida`, `LangGraph`, `End-to-end`) porque contradicen el reposicionamiento.

## Decision 2 — Differentiator.astro (sección nueva)
- **Posición**: entre Hero y Services en `index.astro`. Ancla `id="diferencial"`.
- **Layout**: replicar el patrón de card de Services (`article.rounded-card.border.bg-cream-50`) en grilla `sm:grid-cols-2 lg:grid-cols-4` para 4 pilares: escalable, best practices de IA, mínimo consumo, seguridad. Cada pilar = ícono SVG inline (mismo estilo `stroke-width="1.6"` que Services) + título + desc corta. Reusar el tag/badge pill (`rounded-full bg-cream-100 … text-bark-700`) si hace falta una etiqueta de prueba técnica.
- **Fondo**: claro (`cream`/blanco) para alternar con el `bark-900` del Hero y crear ritmo. Header de sección con eyebrow `text-gold-500 uppercase` igual que Services.
- **Sin logos/casos inventados** (constraint duro de la proposal).

## Decision 3 — Services: 6 → 3-4 cards de resultado
- Colapsar el array `services` de 6 a 3-4 entradas. Cada `title` pasa de feature técnica a RESULTADO de negocio (p.ej. "Atendé y vendé 24/7" en vez de "Chatbots & WhatsApp").
- **Tech survives en `tags`**: las keywords técnicas (LangGraph, Hugging Face, RAG, WhatsApp API, Workflows) bajan al array `tags` de cada card → preserva señales SEO y funciona como prueba del criterio. El `desc` traduce el beneficio.
- Markup de la card NO cambia (íconos, hover, grilla `lg:grid-cols-3` sigue sirviendo para 3 cards; con 4 cae a 2x2 sin tocar clases).
- Eyebrow/titular de sección se ajustan al lenguaje de resultados.

## Decision 4 — Process como protagonista (garantía)
- El markup ya es fuerte (fondo `bark-900`, glow, numeración `01-04`). Promoción = reforzar el **encuadre de copy**: eyebrow/intro que lo posicione explícitamente como la GARANTÍA/método que reemplaza al portfolio ("sin casos que mostrar todavía, pero sí un método probado"). Sin cambios estructurales de layout.
- Mantener posición después de Services (el orden narrativo lo necesita: primero qué lográs, después cómo lo garantizamos).

## Decision 5 — Nav update
- En `Nav.astro`, array `links` (líneas 2-6): agregar `{ href: '#diferencial', label: 'Diferencial' }` como **primer** ítem. Orden final: Diferencial, Servicios, Proceso, Contacto. CTA "Hablemos" y logo intactos.
- El ancla `#diferencial` debe coincidir con el `id` de `Differentiator.astro`.

## Decision 6 — Copy voice
- Español rioplatense (voseo: "contanos", "escribinos"), audiencia dueño de negocio, CERO jerga técnica en titulares/eyebrows. La jerga vive solo en `tags`. Tono coherente con el existente (Contact ya usa voseo).

## Out of scope (flagged)
- **Form wiring**: `Contact.astro` mantiene `action="#"` (placeholder, ver NOTE líneas 1-4). Conectar endpoint (Formspree/serverless/CRM) NO es parte de este cambio.
- Logos, clientes, casos de estudio inventados: prohibido por decisión explícita.
- Rediseño visual / nueva paleta / i18n.

## Data flow / integration points
- Sin backend, sin estado, sin fetch. Todo es data estática en frontmatter de cada componente → SSG de Astro. Integración = composición de componentes en `index.astro`. El único "contrato" entre componentes son los anchors (`#diferencial`, `#servicios`, `#proceso`, `#contacto`, `#top`) que Nav referencia.

## Risks / assumptions
- Sin prueba social, la credibilidad recae 100% en copy de método + transparencia técnica (mitigado por Process protagonista + tags reales).
- Riesgo de copy genérico en Services: cada card DEBE anclar a un resultado concreto, no "automatizamos".
- Asunción: Footer lista secciones; verificar en apply si requiere el nuevo anchor.
- La grilla `lg:grid-cols-4` de Differentiator asume 4 pilares fijos (alineado con proposal).
- Con la stats strip eliminada, el Hero pierde altura visual: asegurar que el subtítulo + CTA tengan suficiente peso para llenar el viewport del fold (no se agrega CSS; el `pt-36 pb-24` existente debería bastar, validar en apply).
