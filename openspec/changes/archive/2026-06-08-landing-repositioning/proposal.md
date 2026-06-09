# Proposal: Reposicionamiento de landing — de catálogo de features de IA a criterio de negocio

## Intent
La landing actual le habla a ingenieros: el Hero abre con LangGraph, fine-tuning, Hugging Face, RAG. El dueño de negocio no entiende qué problema le resolvemos. Hay que reposicionar el mensaje hacia el RESULTADO de negocio, con un doble diferencial poco común en agencias chicas: **IA + seguridad a medida**. Como el proyecto recién arranca y NO hay clientes ni casos reales, la confianza se construye con método y transparencia técnica, no con prueba social inventada. Frase guía: *"dejar de vender herramientas, empezar a vender criterio."*

## Scope

### In Scope
- **Hero**: reescribir para liderar con el problema del cliente + diferencial "IA + seguridad a medida" (sin jerga técnica en el titular).
- **Nueva sección "Diferencial"**: 4 pilares (escalable, best practices de IA, mínimo consumo, seguridad). Reemplaza a la prueba social.
- **Servicios**: convertir las 6 cards de features técnicas en 3-4 RESULTADOS de negocio; la tecnología pasa a subtítulo/tags como prueba del criterio.
- **Proceso**: promover a protagonista — el método ES la garantía sin portfolio.
- **Contacto**: mantener y reforzar "diagnóstico gratis / propuesta a medida".
- Ajustes menores de Nav/Footer para reflejar la nueva estructura.

### Out of Scope
- Inventar logos, clientes o casos de estudio (decisión explícita: NO).
- Cambios de backend, datos, formularios funcionales o integraciones.
- Rediseño visual / nuevo sistema de design o paleta.
- i18n: el sitio sigue 100% en español rioplatense.

## Capabilities
> Cambio de copy + arquitectura de información sobre un sitio Astro single-page. No hay capabilities de software. Se nombran como secciones de página para el contrato con sdd-spec.

### New Capabilities
- `landing-differentiator`: nueva sección de 4 pilares que sustituye la prueba social.

### Modified Capabilities
- `landing-hero`: titular orientado a problema + diferencial IA+seguridad.
- `landing-services`: features → resultados de negocio, tech como tags.
- `landing-process`: el método como garantía (protagonista).
- `landing-contact`: refuerzo "diagnóstico gratis".

## Approach
Reposición de contenido + IA, no de plataforma. Se reescribe la copy y se reordena la jerarquía de secciones en componentes Astro existentes. La tecnología deja de ser titular y pasa a ser PRUEBA del criterio (tags/subtítulos). El orden narrativo nuevo: Hero → Diferencial → Servicios → Proceso → Contacto.

## Affected Areas

| Area | Impact | Description |
|------|--------|-------------|
| `src/components/Hero.astro` | Modified | Titular problema + IA+seguridad |
| `src/components/Differentiator.astro` | New | 4 pilares |
| `src/components/Services.astro` | Modified | 6 features → 3-4 resultados |
| `src/components/Process.astro` | Modified | Protagonista / garantía |
| `src/components/Contact.astro` | Modified | Diagnóstico gratis |
| `src/components/{Nav,Footer}.astro` | Modified | Links a nueva estructura |
| `src/pages/index.astro` | Modified | Reordenar secciones |

## Risks

| Risk | Likelihood | Mitigation |
|------|------------|------------|
| Sin prueba social, baja credibilidad | Med | Método + transparencia técnica como confianza |
| Copy genérico ("automatizamos") | Med | Anclar cada servicio a un resultado medible |
| Perder señales SEO técnicas | Low | Mover keywords técnicas a tags/subtítulos, no eliminarlas |

## Rollback Plan
Cambio puramente de contenido/markup en componentes Astro. Revertir vía `git revert` del commit/PR. Sin migraciones, datos ni dependencias nuevas — rollback inmediato y seguro.

## Dependencies
- Ninguna externa. No requiere nuevas librerías ni servicios.

## Success Criteria
- [ ] El Hero comunica el problema de negocio sin jerga técnica en el titular.
- [ ] Existe la sección Diferencial con 4 pilares; no hay logos/casos inventados.
- [ ] Servicios muestra 3-4 resultados de negocio con tech como tags.
- [ ] Proceso queda posicionado como garantía/protagonista.
- [ ] Toda la copy en español rioplatense, coherente con la voz actual.
