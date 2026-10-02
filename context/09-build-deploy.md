# Rama 09 — Build, Deploy y Tooling

## Stack de build

- **Vite** ^6.2.3 (`vite.config.ts`): plugin React + plugin Tailwind CSS v4,
  alias `@/` → raíz, `base` condicional para GitHub Pages
  (`GITHUB_PAGES=true` → `/Piano-proyectado/`).
- **TypeScript** ~5.8.2 (target ES2022, jsx react-jsx, `noEmit`).
- HMR desactivable con `DISABLE_HMR=true` (evita flickering en edición agéntica).

## Scripts npm

| Script | Comando | Efecto |
|---|---|---|
| dev | `vite --port=3000 --host=0.0.0.0` | Servidor dev en `:3000` |
| build | `vite build` | Build de producción → `dist/` |
| preview | `vite preview` | Sirve el build |
| lint | `tsc --noEmit` | Typecheck (sin emitir) |
| clean | `rm -rf dist server.js` | Limpia build y server (Unix) |

## Dependencias del runtime

- `tone` ^15.1.22 — motor de audio
- `abcjs` ^6.6.3 — parser ABC
- `midi-parser-js` ^4.0.4 — parser MIDI
- `canvas-confetti` ^1.9.4 — confetti easter egg
- `lucide-react` ^0.546.0 — iconos
- `motion` ^12.23.24 — animaciones (framer-motion / motion)
- `react` / `react-dom` ^19.0.1
- `@google/genai` ^2.4.0 — librería de Google GenAI (declarada, sin uso en el código aún)
- `express` ^4.21.2 + `dotenv` ^17.2.3 — dependencias declaradas; **no hay aún
  archivo `server.js` en el repo** (el script `clean` lo referen-cia). Un servidor
  Node con Express probablemente se implementará más adelante para servir la app
  o exponer la API Gemini.

## CI/CD — GitHub Pages (`.github/workflows/pages.yml`)

- Disparadores: push a `main`/`master`, `workflow_dispatch`.
- Node 22, `npm ci`, build con `GITHUB_PAGES=true`, upload de `dist/` como
  Pages artifact, deploy con `actions/deploy-pages@v4`.
- URL de demo: https://cryaut.github.io/Piano-proyectado/
