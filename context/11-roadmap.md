# Rama 11 — Estado y Roadmap

## Estado actual

- **Prototipo en desarrollo activo**. Repo: `cryaut/Piano-proyectado`.
- Deployed en GitHub Pages (workflow `pages.yml`).
- Funcional: engine multi-sample + fallback FM, entrada QWERTY/MIDI/HID,
  modos de juego con scoring, grabación + export MIDI, editor de piano roll con
  transformaciones y autosave.

## Documentado como siguiente expansión (docs/architecture.md)

- Multi-track/staff model (mano izquierda/derecha) en vez de metadata de mano.
- Command palette para operaciones del editor.
- Snapshots / version history más allá de un draft.
- Loop region y punch-in dentro del editor.
- Mejor normalización de import MIDI con tempo-map.
- Code splitting del bundle grande.
- Traducción de reportes HID (IROK/MG75) a notas — probablemente se implementará
  en la siguiente iteración del puente HID.
- Servidor Node/Express (`server.js` mencionado en el script `clean`) para
  servir la app / API — probablemente se implementará más adelante.

## Limitaciones conocidas

- Sample loading requiere red (fallback FM si falla).
- Persistencia solo local (`localStorage`); sin backend ni cuentas.
- Sin suite de tests unitarios (solo scripts de verificación + smoke E2E).
- UI en español en varios puntos, documentación mixta ES/EN.
