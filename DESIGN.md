# DESIGN.md · SOMA

Fuente de verdad del sistema visual. Los valores viven en `packages/tokens/src/index.ts`; este documento explica sus reglas.

## La idea

La soma de Huxley es una pastilla: convexa, algo que te tragas para no sentir. El símbolo de SOMA es su inverso: un **pozo**, un círculo hundido al que te asomas. De ahí sale todo lo demás.

## Gramática del relieve

| Relieve | Significa | Dónde |
| --- | --- | --- |
| Hundido | Lo tuyo | Campos de texto, casillas, tus entradas, lo que SOMA recuerda de ti |
| Plano | Lectura | Mensajes, textos largos |
| Elevado | Una acción que SOMA te ofrece | Botones, sugerencias, selectores |
| Tinta | Lo activo, lo elegido | Pestaña y modo actuales, botón principal |

Reglas: un único elemento de altura 3 por pantalla; los mensajes nunca llevan relieve; el estado activo se marca con tinta, nunca solo con sombra (contraste 3:1 para controles).

## Color

| Token | Claro | Oscuro | Contraste sobre el fondo |
| --- | --- | --- | --- |
| ground | #F1F0EC | #202124 | — |
| surface | #F1F0EC | #27292D | — |
| sunken | #EAE8E3 | #1C1D20 | — |
| bubble | #E6E4DE | #2E3035 | — |
| ink | #252525 | #F1F0EB | 13,4 / 14,1 |
| textSecondary | #6A6964 | #A2A2A2 | 4,8 / 6,3 |
| textSecondaryOnSunken | #5E5D58 | #A2A2A2 | ≥ 4,5 sobre sunken |
| lavender (no texto) | #76748A | #AAA4C4 | 4,0 / 6,8 |
| lavenderText | #5F5C73 | #AAA4C4 | 5,6 / 6,8 |
| critical | #A3352B | #E58A80 | 5,9 / 6,4 |

Sombras: luz arriba a la izquierda, sombra cálida abajo a la derecha; nunca negro puro en claro. Alturas 4/8, 5/10 y 10/20 (desplazamiento/difuminado).

## Tipografía

- **Geist**: interfaz y todo lo que escribe la persona.
- **Newsreader**: la voz de SOMA. Lo que dijiste y lo que SOMA interpreta nunca se parecen.

| Rol | Fuente | Tamaño / interlínea |
| --- | --- | --- |
| screenTitle | Geist Semibold, −2 % | 28 / 34 |
| blockTitle | Geist Medium | 20 / 26 |
| voice | Newsreader | 18 / 28 |
| voiceLarge | Newsreader | 22 / 30 |
| body | Geist | 17 / 26 |
| secondary | Geist | 15 / 22 |
| label | Geist Medium | 13 / 18 |
| caption | Geist | 13 / 18 |

Los tamaños siguen la configuración de texto del sistema; los controles limitan el multiplicador a 1,3.

## Pantalla principal

Minimalista por decisión de Manu (2 de noviembre): una pregunta, un botón pequeño que revela la frase del día y la barra de chat. Un único botón de menú arriba a la derecha abre todo lo demás (secciones, historial, Tú y Ayuda urgente). El modo de conversación vive dentro de la barra de chat.

La frase del día es una por fecha, igual para todos, de autores clásicos con su fuente real (`src/lib/frases.ts`).

## Forma y espacio

Rejilla de 4 pt. Margen lateral de 20. Áreas táctiles de 48 (cumple iOS y Material). Controles en píldora; tarjetas con radio 24; pozos circulares.

## Componentes

`T`, `Surface`, `Press`, `Pozo`, `Wordmark`, `DailyPhrase`, `Composer` (con el modo dentro), `ModeMenu`, `MessageView`, `CrisisCard`, `Field`, `Checkbox`, `Screen` / `IconButton`, `SectionPending`, `LockScreen`. En `apps/mobile/src/components`.

## Movimiento

Ver `docs/movimiento.md`.

## Referencias de Manu

Hoja neomórfica (método de sombra doble), barra flotante (tinta sobre piedra), control segmentado anidado (selector de modos), buscadores hundidos (campos), huella en un hueco (bloqueo), rueda de emociones (Diario, Fase 2). Los azules fríos de la hoja quedaron fuera por la calidez del brief.
