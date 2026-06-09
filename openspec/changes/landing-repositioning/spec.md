# Spec: Reposicionamiento de landing — agencia-fix

## Scope del cambio
Reescritura de copy + reordenamiento de secciones en la landing Astro single-page.
No hay cambios de backend, estilos globales, paleta, tipografía ni integraciones.
El orden narrativo resultante DEBE ser: Hero → Diferencial → Servicios → Proceso → Contacto.

---

## SECCIÓN 1 — `landing-hero`

### Requisitos

**REQ-HERO-1**: El titular principal (`<h1>`) NO debe contener nombres de productos, frameworks ni herramientas de IA (LangGraph, LangChain, fine-tuning, RAG, Hugging Face, GPT, embeddings, tool use, etc.).

**REQ-HERO-2**: El titular principal comunica un PROBLEMA de negocio real que el cliente target padece — pérdida de tiempo, ineficiencia operativa, errores humanos repetibles — en lenguaje llano.

**REQ-HERO-3**: El subtítulo o bajada menciona explícitamente los dos diferenciadores: "IA" y "seguridad", con la frase de posicionamiento "a medida" o equivalente.

**REQ-HERO-4**: Existe exactamente un CTA principal con texto "Contanos tu proyecto" o "Pedí tu diagnóstico gratis" (o variante semánticamente equivalente que incluya "diagnóstico").

**REQ-HERO-5**: No hay logos de clientes, testimonios ni referencias a casos de estudio en esta sección.

### Escenarios de aceptación

**SCENARIO HERO-A — Titular sin jerga**
- Dado: la landing está renderizada
- Cuando: se lee el contenido del `<h1>` del Hero
- Entonces: el texto NO contiene ninguna de las palabras prohibidas: "LangGraph", "LangChain", "fine-tuning", "RAG", "Hugging Face", "embeddings", "tool use", "pipeline", "inference"

**SCENARIO HERO-B — Problema de negocio presente**
- Dado: la landing está renderizada
- Cuando: se lee el `<h1>` y el `<p>` de bajada del Hero
- Entonces: al menos uno de los dos contiene vocabulario de resultado/problema de negocio: "tiempo", "costo", "error", "proceso", "equipo", "escalar", "eficiencia", o equivalente semántico
- Y: el texto NO habla de "automatización genérica" ni usa "automatizamos" como única propuesta

**SCENARIO HERO-C — Diferencial IA+seguridad**
- Dado: la landing está renderizada
- Cuando: se lee el bloque completo del Hero (h1 + bajada + CTA)
- Entonces: el texto menciona "seguridad" al menos una vez
- Y: el texto menciona "IA" o "inteligencia artificial" al menos una vez

**SCENARIO HERO-D — CTA diagnóstico**
- Dado: la landing está renderizada
- Cuando: se inspecciona el Hero
- Entonces: existe un elemento interactivo (botón o enlace) cuyo texto contiene "diagnóstico" o "proyecto"
- Y: ese elemento apunta a la sección de contacto o a un formulario

---

## SECCIÓN 2 — `landing-differentiator` (NUEVA)

### Requisitos

**REQ-DIFF-1**: La sección existe en el DOM y está ubicada DESPUÉS del Hero y ANTES de Servicios.

**REQ-DIFF-2**: La sección presenta exactamente 4 pilares, cada uno con título y descripción propia.

**REQ-DIFF-3**: Los 4 pilares cubren obligatoriamente estos conceptos (en cualquier orden y redacción):
  - Escalabilidad (la solución crece con el negocio)
  - Best practices de IA (no se improvisa, hay criterio)
  - Mínimo consumo / eficiencia de costos (decisión de arquitectura, no azar)
  - Seguridad (protección de datos y procesos)

**REQ-DIFF-4**: La sección NO contiene logos de clientes, fotos de personas reales (testimonios), ni frases atribuidas a terceros.

**REQ-DIFF-5**: No hay sección de "prueba social" con logos de empresas en ninguna parte de la página.

### Escenarios de aceptación

**SCENARIO DIFF-A — Existencia y posición**
- Dado: la landing está renderizada
- Cuando: se recorre el DOM en orden de aparición
- Entonces: existe un elemento identificable como sección Diferencial (por id, clase, o contenido)
- Y: aparece después del Hero y antes de la sección de Servicios

**SCENARIO DIFF-B — Cuatro pilares presentes**
- Dado: la sección Diferencial está renderizada
- Cuando: se cuentan los ítems/cards de pilar
- Entonces: hay exactamente 4 ítems, cada uno con título no vacío y descripción no vacía

**SCENARIO DIFF-C — Cobertura conceptual de los pilares**
- Dado: la sección Diferencial está renderizada
- Cuando: se lee el texto completo de los 4 pilares
- Entonces: el texto incluye al menos una referencia a escalabilidad (o "crece", "escala")
- Y: al menos una referencia a criterio de IA o best practices
- Y: al menos una referencia a consumo, costo, eficiencia o tokens
- Y: al menos una referencia a seguridad o protección de datos

**SCENARIO DIFF-D — Sin prueba social inventada**
- Dado: la landing completa está renderizada
- Cuando: se inspecciona todo el HTML
- Entonces: no existe ningún elemento `<img>` con alt text de empresa (Tarjeta Naranja, Canal 26, Endeavor, etc.)
- Y: no existe ningún bloque de testimonios ni comillas atribuidas a personas externas

---

## SECCIÓN 3 — `landing-services`

### Requisitos

**REQ-SVC-1**: La sección presenta entre 3 y 4 items de servicio (no 6 ni más).

**REQ-SVC-2**: El título principal de cada item describe un RESULTADO de negocio medible o concreto — NO el nombre de una tecnología ni de un feature técnico.

**REQ-SVC-3**: Cada item tiene un subtítulo o sección de tags donde sí puede aparecer tecnología (ejemplos: "Astro", "Python", "IA generativa", "n8n") como prueba del criterio.

**REQ-SVC-4**: Los resultados de negocio deben ser específicos y verificables. Frases como "mejoramos tus procesos" o "implementamos IA" sin contexto NO cumplen este requisito. Cada resultado debe responder implícitamente "¿qué gana el cliente?": ejemplo válido: "Reducí el tiempo de respuesta al cliente sin contratar más personas."

**REQ-SVC-5**: Ningún título de card de servicio contiene nombres de productos de terceros (LangGraph, n8n, Hugging Face, etc.) — esos quedan en subtítulo/tags.

### Escenarios de aceptación

**SCENARIO SVC-A — Cantidad de servicios**
- Dado: la sección Servicios está renderizada
- Cuando: se cuentan los items/cards de servicio
- Entonces: el conteo es >= 3 y <= 4

**SCENARIO SVC-B — Títulos orientados a resultado**
- Dado: la sección Servicios está renderizada
- Cuando: se leen los títulos (h3 o elemento de título de cada card)
- Entonces: ningún título contiene nombres de frameworks o herramientas (LangGraph, LangChain, n8n, RAG, Hugging Face, fine-tuning)
- Y: cada título describe un beneficio para el negocio del cliente, no una capacidad técnica de agencia-fix

**SCENARIO SVC-C — Tecnología en subtítulo/tags**
- Dado: la sección Servicios está renderizada
- Cuando: se inspecciona la estructura de cada card
- Entonces: existe al menos un elemento de subtítulo, descripción o tag por card
- Y: la tecnología relevante aparece en ese elemento secundario (no en el título principal)

**SCENARIO SVC-D — Resultado medible o concreto**
- Dado: la sección Servicios está renderizada
- Cuando: se lee cada título o descripción de card
- Entonces: al menos 3 de los 4 items contienen una referencia a impacto cuantificable o acción concreta del cliente: tiempo ahorrado, procesos eliminados, errores reducidos, crecimiento sin contratar, etc.

---

## SECCIÓN 4 — `landing-process`

### Requisitos

**REQ-PROC-1**: La sección Proceso está posicionada DESPUÉS de Servicios y ANTES de Contacto.

**REQ-PROC-2**: La sección presenta exactamente 4 pasos, en orden, con los nombres: Entendemos → Diseñamos → Programamos → Crecemos (o traducción semánticamente equivalente).

**REQ-PROC-3**: Cada paso tiene una descripción que explica QUÉ hace concretamente agencia-fix en esa etapa — no es una definición de diccionario ni una frase vacía.

**REQ-PROC-4**: La sección tiene un headline o introducción que la enmarca como garantía o método diferenciador (no solo como "cómo trabajamos" genérico).

**REQ-PROC-5**: La jerarquía visual/semántica del Proceso es IGUAL o mayor que la de Servicios — debe funcionar como sección protagonista, no como sección de relleno.

### Escenarios de aceptación

**SCENARIO PROC-A — Posición en la página**
- Dado: la landing está renderizada
- Cuando: se recorre el DOM
- Entonces: la sección Proceso aparece después de Servicios y antes de Contacto

**SCENARIO PROC-B — Cuatro pasos nombrados**
- Dado: la sección Proceso está renderizada
- Cuando: se cuentan y leen los títulos de cada paso
- Entonces: hay exactamente 4 pasos
- Y: los nombres (en cualquier combinación de mayúsculas/minúsculas) corresponden a: "entendemos" / "diseñamos" / "programamos" / "crecemos"

**SCENARIO PROC-C — Descripciones concretas**
- Dado: la sección Proceso está renderizada
- Cuando: se lee la descripción de cada paso
- Entonces: ninguna descripción tiene menos de 15 palabras
- Y: ninguna descripción es puramente definitoria ("En esta etapa definimos los requisitos") sin mencionar una acción concreta de agencia-fix o un entregable

**SCENARIO PROC-D — Headline de garantía**
- Dado: la sección Proceso está renderizada
- Cuando: se lee el título principal de la sección
- Entonces: el texto comunica método, criterio, o garantía — no solo un genérico "¿Cómo trabajamos?"
- Ejemplos válidos: "Nuestro método es tu garantía", "Criterio en cada etapa", "Por qué el proceso importa"

---

## SECCIÓN 5 — `landing-contact`

### Requisitos

**REQ-CONT-1**: La sección de contacto incluye un formulario con al menos campos de nombre, email y mensaje/descripción del proyecto.

**REQ-CONT-2**: El headline o copy principal de la sección menciona explícitamente "diagnóstico gratis" (o "diagnóstico sin costo") y "propuesta a medida".

**REQ-CONT-3**: El copy menciona el plazo de respuesta: "48 horas" o "48hs" o equivalente.

**REQ-CONT-4**: No hay promesas de resultado de negocio sin sustento (no se puede garantizar "ROI en 30 días" ni afirmaciones que expongan legalmente a la agencia).

**REQ-CONT-5**: El CTA del botón de envío es accionable y específico — no dice solo "Enviar" ni "Submit".

### Escenarios de aceptación

**SCENARIO CONT-A — Formulario presente**
- Dado: la sección Contacto está renderizada
- Cuando: se inspecciona el DOM
- Entonces: existe un elemento `<form>` con al menos 3 campos (nombre, email, y uno de mensaje/proyecto)

**SCENARIO CONT-B — Diagnóstico + propuesta en copy**
- Dado: la sección Contacto está renderizada
- Cuando: se lee el headline y el párrafo introductorio
- Entonces: el texto contiene "diagnóstico" (con o sin tilde)
- Y: el texto contiene "propuesta" o "a medida"

**SCENARIO CONT-C — Plazo de 48 horas**
- Dado: la sección Contacto está renderizada
- Cuando: se lee el copy de la sección
- Entonces: el texto contiene "48" (como "48h", "48hs", "48 horas")

**SCENARIO CONT-D — CTA específico**
- Dado: la sección Contacto está renderizada
- Cuando: se lee el texto del botón de envío del formulario
- Entonces: el texto NO es solo "Enviar", "Submit", "OK" o "Mandar"
- Y: el texto incluye al menos una palabra de acción con contexto: "proyecto", "diagnóstico", "propuesta", "consulta"

---

## Invariantes globales de la página

**INV-1**: Toda la copy de la landing está en español. No hay frases en inglés salvo términos técnicos en tags/badges (Python, IA generativa) donde el español sería forzado.

**INV-2**: El orden de secciones en `index.astro` es estrictamente: Hero → Diferencial → Servicios → Proceso → Contacto.

**INV-3**: No existe ninguna sección de "Clientes", "Casos de éxito", "Testimonios" o "Logos" con datos fabricados.

**INV-4**: Los nombres de herramientas de IA (LangGraph, RAG, fine-tuning, Hugging Face, etc.) NO aparecen en ningún `<h1>`, `<h2>` ni título de card principal de la landing.

**INV-5**: La frase "a medida" o el concepto de personalización aparece al menos 2 veces en la landing completa (Hero + Contacto mínimo).

---

## Assumptions y riesgos del spec

1. **Outcomes medibles (SVC-D)**: el spec exige que al menos 3 de 4 cards tengan resultado concreto. Si durante apply la copy queda genérica ("mejoramos la eficiencia"), sdd-verify debe marcar CRITICAL. El riesgo de genericidad es el más alto de este cambio.

2. **Diferencial IA+seguridad**: se asume que "seguridad" refiere a seguridad de datos/procesos del cliente, no solo a ciberseguridad de infraestructura. La copy debe clarificarlo en subtítulo si el título es ambiguo.

3. **Cuatro pasos del Proceso**: los nombres Entendemos/Diseñamos/Programamos/Crecemos vienen del proposal. Si la copy actual usa sinónimos exactos, sdd-verify acepta equivalentes semánticos.

4. **Sin testimonios**: el spec es explícito en rechazar prueba social fabricada. Si en el futuro hay clientes reales, este invariante (INV-3) se revisa vía nueva propuesta SDD.
