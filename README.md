# corum-asesores

Este repositorio no contiene código de aplicación. Es una configuración de **Claude Code** que convierte a Claude en un **consejo de asesores** para ayudarte a tomar decisiones o planear estrategias.

Se usa con un solo comando: `/debate <tu decisión>`.

## ¿Qué hace?

`/debate` reúne a 15 subagentes con personalidades distintas, los hace debatir en dos rondas y un juez entrega un veredicto con los mejores argumentos.

1. **Ronda 1: posturas independientes.** Los 15 agentes responden en paralelo, sin verse entre sí.
2. **Ronda 2: réplica.** Cada agente lee las 15 posturas, rebate las más fuertes, concede lo válido y actualiza su voto.
3. **Juicio.** El juez cuenta votos, detecta consenso falso, resume los desacuerdos reales y entrega una recomendación con pasos concretos.

Claude redacta la propuesta de forma neutral, sin revelar su propia opinión, para no sesgar el debate.

### Ejemplo

```
/debate debería comprar un disco duro de 300 pesos para mi Xbox si tengo una deuda de 1000 pesos en la tarjeta y me deben 3600 el viernes
```

## Los participantes

| # | Agente | Qué aporta |
|---|---|---|
| 01 | Entusiasta | Defiende la propuesta y el costo de no actuar |
| 02 | Escéptico | La rechaza y busca debilidades |
| 03 | Abogado del Diablo | Defiende la postura contraria a la mayoría |
| 04 | Moderador | Ordena el debate y busca puntos en común |
| 05 | Futurista | Piensa a 5, 10 y 20 años |
| 06 | Historiador | Busca precedentes y casos parecidos |
| 07 | Urgente | Se enfoca en el aquí y ahora; evita la parálisis por análisis |
| 08 | Financiero | Costos, retorno y presupuesto |
| 09 | Gestor de Riesgos | Qué puede salir mal y cómo prevenirlo |
| 10 | Analista de Datos | Pide evidencia y detecta sesgos |
| 11 | Experto Técnico | Factibilidad y limitaciones reales |
| 12 | Creativo | Alternativas poco convencionales |
| 13 | Estratega | Coherencia con objetivos y rumbo general |
| 14 | Hacedor | Siempre vota SÍ; convierte el SÍ en un plan ejecutable |
| 15 | Humanista | Bienestar, valores, ética y a quién más afecta |
| — | Juez | Cuenta votos y emite el veredicto |

Cuatro agentes tienen postura obligada por diseño: Entusiasta, Hacedor, Escéptico y Abogado del Diablo. El juez muestra el conteo de votos con y sin ellos para que no inflen el resultado.

## Modos

- **Estándar** (por defecto): decisiones con opciones claras. Entrega el veredicto completo con conteo de votos.
- **top3**: además del veredicto, el juez elige las 3 mejores opiniones individuales del debate, con el agente que las dijo y por qué valen la pena.
- **estrategia**: para decisiones que no son SÍ/NO sino de planear el futuro. Los agentes recomiendan una estrategia en vez de votar, y se entregan las 3 mejores opiniones.

Ejemplo: `/debate estrategia: cómo plantear el siguiente año de mis clases`

## Formato del veredicto

- Conteo de votos (con y sin los agentes de postura obligada) y cambios entre rondas
- Los 3 mejores argumentos a favor y en contra
- Consensos y desacuerdos reales
- Alternativas valiosas
- Recomendación (SÍ, NO, SÍ condicionado o NO por ahora), con pasos concretos, nivel de confianza y qué evidencia la cambiaría

## Estructura del repositorio

```
.claude/
├── agents/
│   ├── debate-01-entusiasta.md … debate-15-humanista.md   # un archivo por debatiente
│   └── debate-juez.md                                      # el juez
└── skills/
    └── debate/
        └── SKILL.md                                        # orquesta las rondas (comando /debate)
```

- Cada archivo en `agents/` define una personalidad, sus reglas y su formato de respuesta. Solo tienen herramientas de lectura (`Read`, `Grep`, `Glob`).
- `skills/debate/SKILL.md` define el procedimiento. Los subagentes no pueden lanzar a otros, así que Claude orquesta las rondas.

## Cómo usarlo

1. Abre este directorio con Claude Code.
2. Escribe `/debate` seguido de tu decisión, con el contexto y las opciones que tengas.
3. Espera a que terminen las dos rondas y el juez.
4. Lee el veredicto. Si faltan datos que los agentes señalaron, complétalos y vuelve a correrlo.

## Límites

- Los 15 agentes comparten el mismo modelo base. El consenso puede ser en parte conformidad de grupo.
- El veredicto es una herramienta de reflexión, no un oráculo. **La decisión final es tuya.**
- Para decisiones médicas, legales o financieras de alto riesgo, consulta a un profesional.
