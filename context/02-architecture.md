# Rama 02 — Arquitectura

## Vista general

- Frontend React + Vite + Tailwind CSS (SPA en `src/`).
- Audio procesado fuera del hilo de render por **Tone.js** (singleton `engine` en
  `src/audio/PianoEngine.ts`).
- Entrada dividida en tres fuentes: QWERTY, Web MIDI y WebHID.
- Comunicación UI ↔ lógica mediante eventos de `window` (`piano-note-on`,
  `piano-sustain-change`, `piano-engine-ready`, `scoring-update`, etc.).
- Persistencia local en `localStorage`: grabaciones, borradores del editor y
  settings de hall-effect.

## Flujo de una nota

```
KeyHandler / MidiBridge / IrokHidBridge
        → engine.noteOn(note, velocity)      (PianoEngine)
        → selección de capa L/M/H según velocity (Sampler ×3 o FMSynth fallback)
        → filtro + volumen por capa → effectsBus
        → tremolo → chorus → distortion → lowpass → dry/wet (Convolver procedural)
        → EQ3 → Compressor → destino
```

## Componentes principales

| Módulo | Archivos | Rol |
|---|---|---|
| Audio | `src/audio/PianoEngine.ts`, `InstrumentPresets.ts` | Motor de samples, FX, presets |
| Entrada | `src/input/*` | KeyHandler, MIDI, HID, velocity |
| Juego | `src/game/*` | NoteHighway, SongPlayer, ScoringEngine |
| Editor | `src/editor/*` | Piano roll y transformaciones |
| Grabación | `src/record/*` | Recorder + export MIDI |
| Import | `src/import/*` | MIDI / MusicXML / ABC / JSON |
| UI | `src/ui/*`, `src/App.tsx` | HUD, teclado visual, modales |
| Debug | `src/debug/*` | KeyTesterPanel e InputDebug |

## Estado global

Sin store externo: el estado se comparte vía singletons de servicio
(`engine`, `keyHandler`, `songPlayer`, `scoringEngine`, `recorder`) y eventos
`CustomEvent` de ventana.
