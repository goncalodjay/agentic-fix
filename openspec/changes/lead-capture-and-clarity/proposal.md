# Proposal: lead-capture-and-clarity

## Why

The landing page (`agencia-fix`) is a lead-generation site for an agency that builds custom AI/automation for small and mid-size businesses. Today it fails its one job in two ways:

1. **It cannot capture a lead.** The contact form posts to `action="#"`, which does a full page reload and sends nothing anywhere. A business owner who fills it out gets no confirmation and no message reaches the owner. There is also no way to book a call.
2. **It speaks the wrong language.** The page is built for the audience that builds the software, not the audience that buys it. Service cards advertise `n8n`, `RAG`, `LangGraph`, `Astro`, `Python`, `APIs`, `Data lake` — terms a non-technical business owner does not recognize and that create distance instead of trust. The "Differentiator" and "Process" sections also leak jargon ("tokens", "arquitectura", "modelos").

**Audience**: the non-technical business owner who recognizes their PROBLEM ("recibo mensajes fuera de horario y se me escapan ventas") but does not know or care about the tooling. Success means this owner can (a) instantly see their problem named and a plain-language solution offered, and (b) act in one click — either send a message that actually arrives or book a 30-minute call.

**Why now**: every visitor today is a lost lead. Both gaps are cheap to close on this static site using hosted third-party services (no backend, no SSR migration).

## What Changes

Four requirements, mapped to the locked decisions:

### Req 1 — Wire the contact form to email (Web3Forms)
Replace the dead `action="#"` form with a working Web3Forms submission:
- Add an inline `<script>` in `Contact.astro` that `preventDefault`s the submit, builds `FormData`, and `fetch` POSTs to `https://api.web3forms.com/submit`.
- Add a hidden `access_key` input. The key is a **user-provided placeholder** — shipped as a clearly-marked placeholder constant the owner replaces with their real key.
- Destination email: `gonzzamc@gmail.com` (configured in the Web3Forms dashboard tied to the access_key).
- Drive UI states `idle → sending → success | error` without page reload, via a `data-state` attribute on the form wrapper and CSS toggling. Disable the button + show "Enviando…" while sending; show a success confirmation or an error message inline.

### Req 2 — Add a booking calendar (Cal.com)
Embed a Cal.com 30-minute call booking widget. The Cal.com username/event link is a **user-provided placeholder** — clearly marked and documented for the owner to fill in.

### Req 3 — Contact section: toggle/tabs layout (LOCKED)
The `#contacto` section gets a TWO-CHOICE toggle (tabs), not side-by-side widgets:
- Tab A: **"Escribinos un mensaje"** → the Web3Forms form (Req 1).
- Tab B: **"Agendá una charla de 30 min"** → the Cal.com embed (Req 2).
- Only ONE widget is visible at a time; the user picks. Must work on mobile.
- **Implementation approach**: a small inline `<script>` toggling a `data-active` attribute on the section and using CSS (`[hidden]` / class toggle) to show/hide panels. Rationale: the page already needs an inline script for Req 1, so reusing the same script for tab switching keeps it to one JS block, is fully accessible (real buttons with `aria-selected`), and is more robust than a pure-CSS `:checked` hack when combined with the form's dynamic states. Default active tab: the message form.

### Req 4 — Reframe the 4 Services cards (problem → plain solution) — covers Req 3 + Req 4 together
**REUSE** the existing 4 `Services.astro` cards. For each card:
- Keep the outcome-oriented `<h3>` title but ensure the `desc` names the PROBLEM the owner recognizes and states the solution in plain language.
- **REPLACE** the jargon tag arrays with one (or a few) plain-language **solution chips** the owner understands. Remove every framework/language/tool name.

Problem→solution mapping (all 4 cards):

| Card | Problem the owner recognizes | Plain-language solution chip(s) |
|------|------------------------------|--------------------------------|
| 1. Atendé y vendé sin parar | Reciben muchos mensajes / mensajes fuera de horario | **Chatbots de WhatsApp** |
| 2. Eliminá el trabajo manual | Flujos desorganizados y cuellos de botella | **Agentes automatizados con las tareas de tu negocio** |
| 3. Decisiones más rápidas con datos reales | Información dispersa, reportes a mano, decisiones lentas | **Tu información ordenada y lista para consultar** |
| 4. Una plataforma a medida | Pagar licencias que no encajan, sin control de la herramienta | **Una aplicación hecha a medida de tu negocio** |

### Req 5 (sub of Req 3) — Jargon cleanup elsewhere
- `Differentiator.astro` pillar 3 desc: remove **"tokens"** → "…para usar solo las llamadas y recursos que hacen falta."
- `Process.astro` step 02 desc: rephrase **"arquitectura"** / **"modelos"** into plain business language → "Definimos el plan a medida: qué herramientas usamos, cómo se conectan y cuánto cuesta operarlo."

**Design reversal (surfaced explicitly):** the prior `landing-repositioning` design kept tech tags "as proof of technical criterion." This change DELIBERATELY REVERSES that decision. The tradeoff: we lose the visible "look how technical we are" signal in exchange for clarity and trust with non-technical buyers. The owner is the buyer's advocate here — clarity wins for a lead-gen landing. This reversal applies to the Services tags, the Process step 02 copy, and the Differentiator pillar 3 copy.

## Scope

**In scope**
- Functional contact form via Web3Forms (client-side, no backend).
- Cal.com 30-min booking embed.
- Toggle/tabs UI inside `#contacto` (message vs. booking).
- Reframing the 4 Services cards (problem framing + plain solution chips).
- Jargon removal in Services, Process step 02, Differentiator pillar 3.
- Rioplatense Spanish copy for all new/changed user-facing text.

**Out of scope**
- SSR / Astro adapter migration — site stays purely static.
- Any custom backend, serverless function, or self-hosted form endpoint.
- CRM / pipeline integration (HubSpot, Notion, etc.).
- Analytics / conversion tracking / A/B testing.
- New page sections or routes (we reuse existing components).
- Adding a JS framework (React/Svelte islands) — plain inline `<script>` only.
- Spam protection beyond Web3Forms' built-in (no custom captcha).
- Email deliverability/DNS configuration (handled in Web3Forms dashboard by owner).

## Affected Files

| File | Change |
|------|--------|
| `src/components/Contact.astro` | Major: restructure `#contacto` into a tabs layout (two panels). Tab A = the existing form, now wired with hidden `access_key` input + `data-state` wrapper + inline `<script>` doing fetch POST to Web3Forms and driving idle/sending/success/error. Tab B = Cal.com embed panel. Inline `<script>` also handles tab switching. Update the stale comment at top (lines 2–3) that references Formspree. Update success/error copy. |
| `src/components/Services.astro` | Edit frontmatter `services` array: reframe `desc` text to name the problem in plain language, and replace all 4 `tags` arrays with plain-language solution chip(s). No structural/template change required (the `s.tags.map` chip rendering stays). |
| `src/components/Process.astro` | Minor copy edit: step 02 desc — remove "arquitectura" and "modelos". |
| `src/components/Differentiator.astro` | Minor copy edit: pillar 3 desc — remove "tokens". |

`src/pages/index.astro` is NOT changed — no new component is added (Req 4 reuses Services).

## Risks & Mitigations

| Risk | Mitigation |
|------|-----------|
| **Placeholders ship empty** — if the owner deploys before pasting the Web3Forms key / Cal.com link, the form and calendar silently fail. | Use clearly-marked placeholder constants (e.g. `YOUR_WEB3FORMS_ACCESS_KEY`, `YOUR_CALCOM_LINK`) with a code comment + a dedicated "Owner setup" note in the spec. Optionally render a visible inline warning when the placeholder is still present. |
| **Cal.com mobile layout** — third-party embeds can overflow or scroll awkwardly on small screens; two widgets in one section compounds this. | The tabs design already shows only ONE widget at a time, which is the main mitigation. Give the Cal.com panel a responsive container (full width, min-height, controlled height) and test the embed on a narrow viewport during apply. Flag a popup/button fallback if inline proves too bulky. |
| **External service availability** — Web3Forms or Cal.com downtime breaks capture. | Web3Forms failure is handled by the `error` UI state with a fallback instruction (show a mailto / direct email). Cal.com is hosted externally; acceptable risk for a lead-gen landing. |
| **Design reversal regret** — removing tech tags loses the "technical proof" signal the prior design valued. | Surfaced explicitly above; this is a deliberate, owner-aligned tradeoff. Technical credibility now lives in the Process section's "plan claro" language rather than tool-name tags. |
| **Inline script + Astro** — script must run client-side and bind after DOM ready. | Astro bundles `<script>` tags and runs them client-side by default; bind listeners on `DOMContentLoaded` or place the script after the markup. No framework needed. |
| **Accessibility of tabs** | Use real `<button>` elements with `role="tab"` / `aria-selected`, panels with `role="tabpanel"`, and keep keyboard focus working. |

## Open Config Items (owner must supply)

1. **Web3Forms access_key** — owner registers at web3forms.com with destination `gonzzamc@gmail.com`, then pastes the key into the placeholder constant in `Contact.astro`.
2. **Cal.com link** — owner's Cal.com username + 30-min event slug (e.g. `cal.com/USERNAME/30min`), pasted into the placeholder in `Contact.astro`.

Until both are supplied, the form and calendar are non-functional by design (placeholders are intentional).

## Proposed Copy Draft (Rioplatense Spanish)

### Services cards — reframed `desc` + solution chip(s)

**Card 1 — Atendé y vendé sin parar**
- desc: "¿Te llegan mensajes a toda hora y se te escapan ventas cuando no podés responder? Tu negocio contesta, califica y vende solo, las 24 horas, sin que nadie tenga que estar pendiente del teléfono."
- chip: `Chatbots de WhatsApp`

**Card 2 — Eliminá el trabajo manual que frena a tu equipo**
- desc: "¿Tu equipo pierde horas en tareas repetitivas y los procesos se traban? Automatizamos el trabajo manual y ordenamos los flujos para que las cosas pasen solas y sin cuellos de botella."
- chip: `Agentes automatizados con las tareas de tu negocio`

**Card 3 — Decisiones más rápidas con datos reales**
- desc: "¿Tenés la información dispersa y dependés de que alguien arme un reporte a mano? Centralizamos los datos de tu negocio y los hacemos fáciles de consultar, para que decidas en minutos y no en días."
- chip: `Tu información ordenada y lista para consultar`

**Card 4 — Una plataforma a medida para crecer sin límites**
- desc: "¿Pagás licencias de herramientas que no terminan de encajar con tu negocio? Te construimos tu propia aplicación, hecha a tu medida, para que tengas el control total y dejes de adaptarte a soluciones genéricas."
- chip: `Una aplicación hecha a medida de tu negocio`

### Contact toggle (tabs)

- Tab A label: **"Escribinos un mensaje"**
- Tab B label: **"Agendá una charla de 30 min"**

### Form state messages

- Submit button (idle): "Pedir diagnóstico →"
- Sending: "Enviando…"
- Success: "¡Listo! Recibimos tu mensaje. Te contactamos a la brevedad."
- Error: "Uy, algo falló al enviar. Probá de nuevo o escribinos directo a gonzzamc@gmail.com."

### Booking panel intro (optional helper line above the Cal.com embed)

- "Elegí el día y horario que te queden cómodos. Son 30 minutos, sin compromiso."

### Jargon cleanup copy

- Differentiator pillar 3 desc: "…para usar solo las llamadas y recursos que hacen falta."
- Process step 02 desc: "Definimos el plan a medida: qué herramientas usamos, cómo se conectan y cuánto cuesta operarlo."

## Next

Run `sdd-spec` and `sdd-design` (can run in parallel) against this proposal.
