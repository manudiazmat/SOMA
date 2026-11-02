# SOMA

> Siéntelo. Entiéndelo.

SOMA le da la vuelta a la droga de *Un mundo feliz*: no busca que dejes de sentir, sino que entiendas lo que sientes. Este repositorio contiene la **Fase 1 — Cimientos**: app móvil nativa, Privacy Gateway, base de datos con aislamiento por usuario y conversación segura con Claude.

## Qué hay

```
apps/mobile         App Expo (React Native 0.88, Expo Router, Reanimated 4)
services/gateway    Privacy Gateway + orquestador (Hono, Node 22)
packages/tokens     Tokens de diseño «piedra y lavanda»
packages/contracts  Contratos de la API compartidos (zod)
packages/db-tests   Tests de aislamiento RLS sobre las migraciones reales (PGlite)
supabase/           Migraciones y configuración local
docs/               Decisiones, privacidad, movimiento
DESIGN.md           Sistema de diseño · PRODUCT.md  Contexto de producto
```

## Demo en GitHub Pages

Cada push a `main` publica la demo web en `https://manudiazmat.github.io/SOMA/` (flujo `.github/workflows/pages.yml`). La primera vez: **Settings → Pages → Source: GitHub Actions**.

En Pages la IA no está conectada: no hay servidor ni claves, así que las respuestas son frases de prueba y la página lo dice. La IA real funciona en la versión que se abre dentro de Claude (artefacto) y, en la app, a través del Gateway.

## Probarlo en 1 minuto (sin backend)

```bash
npm install
cd apps/mobile
npx expo run:ios          # o run:android; la primera vez compila un development build
npx expo start --web      # vista previa rápida en el navegador
```

Expo Go todavía no está disponible para el SDK 58, así que el móvil necesita un development build (local con Xcode o Android Studio, o `npx eas-cli@latest build --profile development`).

Sin variables de entorno, la app arranca en **modo demostración**: recorre el flujo real (correo → código → consentimiento → conversación) con todo en memoria y respuestas de prueba. Cualquier código de 6 cifras sirve. Solo existe en builds de desarrollo.

## Arrancarlo de verdad (local)

```bash
# 1. Supabase local (requiere Docker y la CLI de Supabase)
supabase start                    # aplica supabase/migrations

# 2. Gateway
cp services/gateway/.env.example services/gateway/.env    # rellena SUPABASE_JWT_SECRET, DATABASE_URL…
npm run dev:gateway               # MODEL_PROVIDER=mock no necesita credenciales

# 3. App
cp apps/mobile/.env.example apps/mobile/.env              # URL de Supabase, anon key, URL del Gateway
npm run dev:mobile
```

Para respuestas reales: `MODEL_PROVIDER=bedrock` con `AWS_REGION=eu-central-1` y los perfiles `eu.anthropic.claude-sonnet-5-5` / `eu.anthropic.claude-haiku-5-5`, o `MODEL_PROVIDER=anthropic` con una clave (solo desarrollo: la configuración rechaza esta ruta en producción).

## Comprobaciones

```bash
npm run typecheck     # los cinco paquetes
npm test              # 14 tests de RLS + 36 del Gateway
```

Lo que cubren los tests:

| Riesgo del brief | Cómo se comprueba |
| --- | --- |
| Fuga de datos entre usuarios | 14 tests sobre las migraciones reales: lectura, escritura, borrado y suplantación entre dos usuarios; prueba de mutación (sin RLS fallan) |
| Acceso no autorizado | Tokens ausentes, caducados, de otro emisor, otra audiencia u otro rol → 401 |
| Contenido íntimo en logs | El texto de los mensajes, el token y el id de usuario no aparecen en ninguna línea de log |
| Crisis | Recursos 024/112 antes de cualquier texto generado; el protocolo se impone al modo; el clasificador escala lo que las reglas no ven |
| SQL del Gateway con RLS | El repositorio real contra Postgres (PGlite) con rol y claims por transacción |

## Cómo se usaron las skills

| Skill | Su terreno en este proyecto |
| --- | --- |
| **taste** | Dirección de arte: la idea del pozo frente a la pastilla, la paleta, la escala tipográfica. La landing pública (Fase 6) también será suya |
| **impeccable** | La app como herramienta («Operate»): estructura, estados (vacío, error, sin conexión, crisis), guías nativas iOS/Android, contraste y áreas táctiles de 48 |
| **Emil Kowalski** | Movimiento: hundirse al pulsar, curvas ease-out, nada repetido por encima de 300 ms, sin animación al escribir, movimiento reducido. Ver `docs/movimiento.md` |

Cuando chocan (taste prohíbe sombras y lavanda; el brief pide neomorfismo), manda el brief.

## Estado de la Fase 1

Hecho: pantalla principal minimalista (pregunta, frase del día y barra de chat; el resto en un menú), estructura, sistema de diseño en código, temas claro y oscuro, navegación, acceso por código de un solo uso, bloqueo biométrico, consentimiento explícito, conversación con streaming y cinco modos, protocolo de crisis, Gateway con logs seguros y límites, RLS verificada.

Pendiente antes de la beta (puerta de la Fase 2): cifrado por usuario con KMS, exportación y borrado de cuenta completos, EIPD firmada, revisión legal de los textos de consentimiento. Ver `docs/privacidad.md`.

Verificación visual: hecha con el renderizador web de Expo, **no** en simulador iOS ni emulador Android (no disponibles en este entorno). Hay que repetirla en dispositivo, también con texto grande y con «reducir movimiento».
