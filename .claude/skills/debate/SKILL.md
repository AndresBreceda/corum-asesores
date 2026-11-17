---
name: debate
description: Debate de decisión con 15 subagentes de distintas personalidades y un juez que cuenta votos y sintetiza. Admite modo estándar (veredicto con votos), modo top3 (las 3 mejores opiniones) y modo estrategia (para planear el futuro, sin SÍ/NO). Úsalo cuando el usuario quiera decidir algo mediante /debate <propuesta>.
---

# Debate de 15 arquetipos + Juez

Propuesta a debatir: `$ARGUMENTS` (si está vacía, pregúntale al usuario qué decisión quiere debatir, opciones y contexto relevante).

## Modos
Elige el modo según lo que pida el usuario (o su propuesta):
- **estándar** (por defecto): decisión con opciones claras. Veredicto completo con conteo de votos.
- **top3**: el usuario pide "las 3 mejores opiniones", o prefiere opiniones concretas a un conteo. El juez entrega además la sección "Top 3 opiniones".
- **estrategia**: la decisión no es SÍ/NO sino plantear el futuro o crear una estrategia. En el prompt a cada agente añade: "MODO ESTRATEGIA: no es una decisión SÍ/NO; en vez de votar entrega tu recomendación estratégica concreta según tu personalidad". Implica también el modo top3.
Si dudas entre modos, pregúntaselo al usuario en una línea.

## Anti-sesgo (importante)
- Redacta la propuesta de forma **neutral**, sin revelar tu propia opinión ni inclinaciones. Incluye contexto, opciones y restricciones tal como las dio el usuario; no añadas adornos.
- Los subagentes no pueden lanzar otros subagentes: **tú orquestas** las rondas.
- Postura obligada por diseño (no inflar el conteo): Entusiasta (tiende a SÍ), **Hacedor (siempre SÍ)**, Escéptico (tiende a NO) y Abogado del Diablo (voto de rol).

## Procedimiento

**Ronda 1 – posturas independientes.** Lanza en UNA sola llamada paralela los 15 subagentes (`subagent_type`):
`debate-01-entusiasta`, `debate-02-esceptico`, `debate-03-abogado-del-diablo`, `debate-04-moderador`, `debate-05-futurista`, `debate-06-historiador`, `debate-07-urgente`, `debate-08-financiero`, `debate-09-gestor-de-riesgos`, `debate-10-analista-de-datos`, `debate-11-experto-tecnico`, `debate-12-creativo`, `debate-13-estratega`, `debate-14-hacedor`, `debate-15-humanista`.
Prompt a cada uno: la propuesta neutral + "RONDA 1: da tu postura independiente según tu formato."

**Ronda 2 – réplica.** Lanza otra vez los 15 en paralelo, cada uno con: la propuesta + las 15 respuestas de la ronda 1 (textuales) + "RONDA 2: rebate los argumentos más fuertes, concede lo válido y entrega tu voto final actualizado, indicando si cambiaste y por qué." Si las respuestas son largas, puedes guardarlas en un archivo del scratchpad y pedir a los agentes que lo lean (tienen Read).

**Juicio.** Lanza `debate-juez` con la propuesta, las 15 respuestas de la ronda 1 y las 15 de la ronda 2. Pídele su formato completo de veredicto; en modo top3 o estrategia pídele explícitamente incluir "Top 3 opiniones".

## Entrega al usuario
- Modo estándar: presenta el veredicto del juez íntegro (conteo de votos incluido).
- Modo top3 o estrategia: presenta primero las 3 mejores opiniones del juez, con el agente que las dijo, y después el veredicto resumido.
- Añade 2-3 líneas tuyas solo si detectas algo que el juez omitió. Recuerda que la decisión final es del usuario.
