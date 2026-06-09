# Landing Page Specification

## Overview
Multi-section single-page landing for agencia-fix, positioned toward business decision-makers (not engineers). Communicates IA + custom security as dual differentiator, supported by transparent method and concrete business outcomes.

## Page Sections (Capabilities)

### landing-hero
**Status**: Implemented (post-repositioning)
**Purpose**: Lead with business problem + IA+security positioning; no tech jargon in headline.

**Requirements**:
- Headline must not contain: LangGraph, LangChain, fine-tuning, RAG, Hugging Face, GPT, embeddings, tool use, pipeline, inference
- Headline communicates a real business problem (time, cost, error, process efficiency)
- Mentions "IA" and "seguridad" explicitly with "a medida" or equivalent positioning
- CTA button with text including "diagnóstico" or "proyecto"
- No client logos, testimonials, or case studies in this section

**Current Content**:
- h1: "Tu equipo no da abasto. La IA a medida cambia eso."
- Eyebrow: "IA con seguridad, hecha para tu negocio"
- Subtitle: "Soluciones de IA y seguridad a medida de tu operación"
- CTA: "Pedí tu diagnóstico gratis" → #contacto

---

### landing-differentiator
**Status**: Implemented (new section post-repositioning)
**Purpose**: Replace social proof with 4 pillar framework; position method as trust builder.

**Requirements**:
- Section must appear AFTER Hero, BEFORE Services
- Exactly 4 pillars covering: scalability, IA best practices, minimum cost/efficiency, security/data protection
- No fabricated client logos, testimonials, or social proof
- No testimonials section anywhere on page

**Current Pillars**:
1. "Crece con tu negocio" — scalability
2. "Criterio, no azar" — IA best practices and methodology
3. "Mínimo consumo posible" — cost efficiency and token optimization
4. "Tus datos, protegidos" — security and data protection

---

### landing-services
**Status**: Implemented (restructured post-repositioning)
**Purpose**: Shift from 6 technical features to 3–4 business outcomes; tech becomes proof of methodology.

**Requirements**:
- Between 3–4 service cards (was 6)
- Each title must describe a business OUTCOME, not a technology name
- Technical keywords (RAG, LangGraph, n8n, WhatsApp, Python, Astro) move to tags/secondary elements
- Each outcome must be specific and measurable
- No product/framework names in primary titles

**Current Services** (4 cards):
1. "Atendé y vendé 24/7" — RAG, LangGraph, WhatsApp integration tag
2. "Automatiza procesos que hoy son manuales" — n8n, AI workflow tag
3. "Toma decisiones basadas en datos" — Python, analytics, IA tag
4. "Una plataforma a medida para crecer sin límites" — Astro, custom solutions tag

---

### landing-process
**Status**: Implemented (repositioned as guarantor post-repositioning)
**Purpose**: Position methodology as trust substitute for portfolio; frame as 4-step guarantee.

**Requirements**:
- Position AFTER Services, BEFORE Contact
- Exactly 4 steps named: Entendemos → Diseñamos → Programamos → Crecemos
- Each step has concrete description (≥15 words) of what agency does
- Headline/intro frames process as guarantee or differentiating method
- Visual/semantic hierarchy equal to or greater than Services section

**Current Steps** (4):
1. "Entendemos" — your business, processes, and pain points
2. "Diseñamos" — custom architecture aligned to your needs
3. "Programamos" — build and test with transparent methodology
4. "Crecemos" — iterate and scale with your business

**Positioning Headline**: "Un proceso claro que garantiza resultados"

---

### landing-contact
**Status**: Implemented (reinforced post-repositioning)
**Purpose**: Finalize CTA with "free diagnosis + custom proposal" messaging.

**Requirements**:
- Form with name, email, message/project description fields
- Headline mentions "diagnóstico gratis" or "diagnóstico sin costo"
- Copy includes "propuesta a medida"
- Response time stated: "48h" / "48hs" / "48 horas"
- Submit button text actionable and specific (not "Enviar" / "Submit")
- No unsupported promises of ROI or results

**Current Copy**:
- h2: "Pedí tu diagnóstico gratis"
- Intro: mentions "propuesta a medida"
- Response: "48hs"
- Submit: "Pedir diagnóstico →"

---

## Page-Level Invariants

**INV-1**: All copy in Spanish (Rioplatense). English only in tech tags/badges where Spanish is forced.

**INV-2**: Section order in `src/pages/index.astro` is strictly: Hero → Differentiator → Services → Process → Contact.

**INV-3**: No "Clients", "Success Stories", "Testimonials", or "Logos" section with fabricated data anywhere on page.

**INV-4**: No IA/tech tool names (LangGraph, RAG, fine-tuning, etc.) appear in any h1, h2, or primary card title.

**INV-5**: Phrase "a medida" or equivalent personalization concept appears ≥2 times across page (minimum: Hero + Contact).

---

## Implementation Notes

- **Component files**: All changes isolated to `src/components/{Hero,Differentiator,Services,Process,Contact,Nav}.astro` and `src/pages/index.astro`
- **Design tokens**: Reuse only existing tokens (bark-900, cream-50/100/200, ember-500/600, gold-300/500, grain, glow, glass, reveal, text-molten, rounded-card, etc.). Zero new CSS.
- **No backend/data changes**: Pure content and markup repositioning.
- **No form wiring**: Contact form `action="#"` placeholder — backend integration out of scope.

---

## Verification Status

**Last Verified**: 2026-06-08
**Verdict**: PASS (0 CRITICAL, 0 WARNING, 1 SUGGESTION resolved)
- All 5 sections satisfy requirements and acceptance scenarios
- Build: `npm run build` SUCCESS (9.22s, zero errors)
- Suggestion resolved: Card 4 copy sharpened to clarify "sin pagar licencias de terceros y tenés control total sobre tu solución"
