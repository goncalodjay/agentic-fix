# Tasks: Reposicionamiento de landing — agencia-fix

## Review Workload Forecast

| Field | Value |
|-------|-------|
| Estimated changed lines | ~150–220 (additions + deletions) |
| 400-line budget risk | Low |
| Chained PRs recommended | No |
| Suggested split | Single PR |
| Delivery strategy | ask-on-risk |
| Chain strategy | N/A — single PR |

Decision needed before apply: No
Chained PRs recommended: No
Chain strategy: size-exception
400-line budget risk: Low

### Suggested Work Units

| Unit | Goal | Likely PR | Notes |
|------|------|-----------|-------|
| 1 | All landing copy + nav changes | PR 1 | Single PR covers all 7 files; well under 400-line budget |

---

## Phase 1: Foundation — New component + index wiring

- [x] 1.1 Create `src/components/Differentiator.astro` with 4 pillar cards (escalabilidad, best practices IA, mínimo consumo, seguridad). Frontmatter array `pillars[]` with `{icon, title, desc}`. Layout: `sm:grid-cols-2 lg:grid-cols-4`. Reuse tokens: `rounded-card`, `bg-cream-50`, `bark-900`, `gold-500`, inline SVGs with `stroke-width="1.6"`. Section `id="diferencial"`. Eyebrow: `text-gold-500 uppercase`. _(Satisfies: REQ-DIFF-1, REQ-DIFF-2, REQ-DIFF-3, REQ-DIFF-4; SCENARIO DIFF-A, DIFF-B, DIFF-C, DIFF-D)_

- [x] 1.2 In `src/pages/index.astro`, import `Differentiator` and slot it between `<Hero />` and `<Services />`. Final order in `<main>`: `Hero, Differentiator, Services, Process, Contact`. _(Satisfies: REQ-DIFF-1, INV-2; SCENARIO DIFF-A)_

- [x] 1.3 In `src/components/Nav.astro`, add `{ href: '#diferencial', label: 'Diferencial' }` as first item in the `links` array. Verify existing anchors (`#servicios`, `#proceso`, `#contacto`) are present and unchanged. _(Satisfies: Design Decision 5)_

---

## Phase 2: Core Copy — Hero, Services, Process, Contact

- [x] 2.1 Rewrite `src/components/Hero.astro` eyebrow (line ~23): change current text to copy combining IA + seguridad in plain business language (no tech jargon). _(Satisfies: REQ-HERO-2, REQ-HERO-3; SCENARIO HERO-A, HERO-B, HERO-C)_

- [x] 2.2 Rewrite `src/components/Hero.astro` `<h1>` and subtitle paragraph: headline must describe a business problem (tiempo, costo, error, proceso), subtitle must mention "IA", "seguridad", and "a medida". No tech keywords in `<h1>`. _(Satisfies: REQ-HERO-1, REQ-HERO-2, REQ-HERO-3; SCENARIO HERO-A, HERO-B, HERO-C)_

- [x] 2.3 In `src/components/Hero.astro`, delete the `stats` array from the frontmatter (lines ~2-6) AND delete the `<!-- stats strip -->` rendered block (lines ~62-75) entirely. Verify the Hero closes cleanly after the CTA buttons block. _(Satisfies: Design Decision 1)_

- [x] 2.4 Verify CTA button text in `src/components/Hero.astro` contains "diagnóstico" or "proyecto". Update if needed. _(Satisfies: REQ-HERO-4; SCENARIO HERO-D)_

- [x] 2.5 In `src/components/Services.astro`, collapse `services` array from 6 to 3–4 entries. Each `title` must be a business outcome (e.g. "Atendé y vendé 24/7"). Tech keywords (LangGraph, RAG, WhatsApp API, etc.) move exclusively to `tags[]`. Update `desc` per card to describe client benefit. Update section eyebrow/headline to results-oriented language. _(Satisfies: REQ-SVC-1, REQ-SVC-2, REQ-SVC-3, REQ-SVC-4, REQ-SVC-5; SCENARIO SVC-A, SVC-B, SVC-C, SVC-D)_

- [x] 2.6 In `src/components/Process.astro`, update the section headline and eyebrow to frame the process as a guarantee or differentiating method (not generic "¿Cómo trabajamos?"). Verify 4 steps named Entendemos/Diseñamos/Programamos/Crecemos exist with descriptions ≥15 words each. _(Satisfies: REQ-PROC-2, REQ-PROC-3, REQ-PROC-4, REQ-PROC-5; SCENARIO PROC-B, PROC-C, PROC-D)_

- [x] 2.7 In `src/components/Contact.astro`, ensure headline/intro copy includes "diagnóstico gratis" (or "diagnóstico sin costo"), "propuesta a medida", and "48" (as "48h", "48hs", or "48 horas"). Verify submit button text is NOT "Enviar"/"Submit" and includes "diagnóstico", "proyecto", "propuesta", or "consulta". _(Satisfies: REQ-CONT-2, REQ-CONT-3, REQ-CONT-5; SCENARIO CONT-B, CONT-C, CONT-D)_

---

## Phase 3: Integration Checks

- [x] 3.1 In `src/components/Footer.astro`, check if footer lists section anchors. If yes, add `#diferencial` anchor link matching `Differentiator.astro`'s section `id`. If footer has no section links, skip. _(Footer has no section links — skipped.)_

- [x] 3.2 Verify INV-1 (all copy in Spanish), INV-3 (no fabricated clients/testimonials/logos), INV-4 (no tech jargon in h1/h2/card titles), INV-5 ("a medida" appears ≥2 times across Hero + Contact) by reading all modified files. _(Satisfies: INV-1, INV-3, INV-4, INV-5)_

---

## Phase 4: Build Verification

- [x] 4.1 Run `npm run build` from project root. Confirm zero TypeScript/Astro compilation errors and no missing import warnings. _(Build succeeded cleanly. First run had transient Windows ENOENT in Astro v5 — known intermittent, not our changes.)_

- [ ] 4.2 Run `npm run dev` and visually verify page order in browser: Hero → Diferencial → Servicios → Proceso → Contacto. Confirm no layout breaks caused by Hero stats strip removal. _(Satisfies: INV-2; SCENARIO DIFF-A, PROC-A)_

- [x] 4.3 Check that Nav links scroll to correct anchors (`#diferencial`, `#servicios`, `#proceso`, `#contacto`). Confirm `Differentiator.astro` has `id="diferencial"` on its `<section>`. _(Confirmed in source files.)_
