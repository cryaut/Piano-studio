# Rama 04 — Sistemas de Entrada

## Teclado QWERTY (`src/input/KeyHandler.ts`, `KeyboardMap.ts`)

- Mapeo **ISO-3-ROW**: 3 filas de blancas sin duplicar notas MIDI.
  - Fila inferior `Z`→`/`: `C3`–`E4`
  - Fila media `A`→`'`: `F4`–`B5`
  - Fila superior `Q`→`\`: `C6`–`A7`
- **Capa Shift**: solo negras (la sostenido sobre la blanca). `E` y `B` con Shift
  no producen nota (no hay negra encima) — comportamiento intencional.
- Keyup libera siempre la nota exacta almacenada en keydown, aunque Shift/octava
  cambien entre medias.
- `Space` = sustain; flechas `←`/`→` = ±1 octava (rango −2..+2); ignora eventos
  en inputs/textarea/editor.
- Documentación detallada: `docs/keyboard-mapping.md`.

## Simulador de velocidad (`src/input/VelocitySimulator.ts`, `HallEffectSettings.ts`)

- Modela la *física del martillo*: la velocidad se deriva del gap temporal entre
  keydowns (menor gap → más presión).
- `Shift` → velocity 1.0; `Ctrl` → 0.15.
- Settings de hall-effect con persistencia en `localStorage`:
  enabled, sensibilidad 1–10, min/max velocity, curvas **linear / soft / hard**
  (`√x` / `x²`).

## Web MIDI (`src/input/MidiBridge.ts`)

- `navigator.requestMIDIAccess`, auto-selección del primer input, soporte
  `onmidimessage` + `addEventListener`.
- Note On/Off con velocity escalada `/127`, sustain por CC64 (≥64).
- Aftertouch detectado pero no se registra para no fingir "presión analógica".

## WebHID — teclados magnéticos (`src/input/IrokHidBridge.ts`)

- Auto-detección de dispositivos IROK / MG75 / SparkLink (regex sobre productName).
- Recibe `inputreport` y emite `piano-hid-report` + logs de debug.
- Estado actual: el puente **no mapea aún** los reportes HID a notas (solo
  descubrimiento y raw reports). La traducción a notas probablemente se
  implementará en una siguiente iteración.

## Debug

`src/debug/InputDebug.ts` + `KeyTesterPanel.tsx`: registra cada press/release con
origen (qwerty/midi/hid), mapping resuelto y match con el audio. Accesible con el
botón "Key Tester".
