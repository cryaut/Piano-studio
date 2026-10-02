# Rama 10 — Verificación y Pruebas

## Scripts de verificación (`scripts/`)

| Script | Comando | Qué verifica |
|---|---|---|
| `verify-keyboard-map.ts` | `npx tsx scripts/verify-keyboard-map.ts` | Orden ascendente de blancas, sin duplicados en capa normal, Shift solo-negras, sin fallback a la nota blanca |
| `verify-editor-grid.ts` | `npx tsx scripts/verify-editor-grid.ts` | Grid math del editor (canvas ↔ beat/pitch) |
| `runtime-smoke.py` | `python scripts/runtime-smoke.py` (requiere Playwright + dev server en `:3000`) | Smoke E2E: arranque, keydown C3, Shift+C#3, UNMAPPED_BLACK_KEY, navegación al editor, screenshot `tmp-runtime-smoke.png` |

## Cobertura

- No hay suite de tests unitarios automatizados (`npm test` no existe).
- La verificación se apoya en los scripts `tsx` + el smoke E2E de Playwright.
- Documentación de referencia: `docs/keyboard-mapping.md` y
  `docs/architecture.md` (regresión de mapeo).

## Lint/typecheck

`npm run lint` → `tsc --noEmit` sobre todo el proyecto.
