# Exploration: lead-capture-and-clarity

> Persisted from engram (#85) because the explore phase could not write to disk on Windows.

## Current State (verified against code)

### Req 1 — Contact form (Web3Forms)
- File: `src/components/Contact.astro`
- Line 31: `<form class="space-y-4" action="#" method="post">` — submits to `#`, causes full page reload, no JS, no fetch, no state handling.
- Fields present: `name` (text, required), `email` (email, required), `message` (textarea, required).
- No `<script>` tag anywhere in the file.
- Submit button: line 65–70, plain `type="submit"`, text "Pedir diagnóstico →".
- Container: `grid items-center gap-10 md:grid-cols-2` — left col = copy/bullets, right col = form.

### Req 2 — Cal.com booking calendar
- No calendar embed exists anywhere in the codebase.
- `#contacto` section currently only has: left col (headline + copy + 2 bullets) + right col (form).
- Layout: `rounded-[2rem]` card, `bg-gradient-to-br from-ember-500 to-ember-700`, `px-8 py-14`.

### Req 3 — Tech jargon locations (verified)
- `src/components/Services.astro` tags (lines 8, 14, 20, 26):
  - Service 1 tags: `['WhatsApp API', 'Web chat', 'Multicanal', 'IA conversacional']`
  - Service 2 tags: `['Workflows', 'Integraciones', 'APIs', 'n8n']`
  - Service 3 tags: `['RAG', 'Data lake', 'Dashboards', 'LangGraph']`
  - Service 4 tags: `['Web apps', 'Astro', 'Python', 'IA integrada']`
- `src/components/Process.astro` step 02 desc (line 11): "Definimos la arquitectura a medida: qué modelos, qué integraciones, qué costo operativo." — "arquitectura" and "modelos" are jargon.
- `src/components/Differentiator.astro` pillar 3 (lines 15–17): desc "…para usar solo los tokens, llamadas y recursos que hacen falta." — "tokens" is tech jargon.
- `src/components/Hero.astro`: CLEAN.
- The previous `landing-repositioning` change explicitly kept tech tags "as proof of technical criterion". Req 3 supersedes that.

### Req 4 — Pain point cards
- No dedicated pain-point section exists.
- Services.astro cards already have outcome-focused titles; the pain/problem framing is not surfaced.

## Recommendation Summary

| Requirement | Recommended approach | Effort |
|-------------|---------------------|--------|
| Req 1 Web3Forms | Inline `<script>` in Contact.astro, data-state pattern | Low |
| Req 2 Cal.com | Embed for 30-min calls (placeholder link) | Low-Medium |
| Req 3 Jargon removal | Edit Services tags, Process step 02 desc, Differentiator pillar 3 desc | Low |
| Req 4 Pain cards | Reframe Services cards (problem framing + plain solution chip) | Low |

Open config: Web3Forms access_key, Cal.com link. Both are user-provided placeholders.
