# Rama 07 — Grabación e Importación

## Grabación (`src/record/Recorder.ts`)

- Captura NoteOn/NoteOff con timestamps en ms, velocity y eventos de sustain.
- Límite de 4 minutos por grabación (auto-stop).
- Notas abiertas al detener se cierran con duración mínima de 12 ms.
- Persistencia: `localStorage` (`realpiano_recordings`), máx 20 grabaciones.
- CRUD: guardar, renombrar, borrar; UI en `RecorderControls.tsx` y `RecordingsView.tsx`.

## Export MIDI

- `generateMidiFile()` construye un **Standard MIDI File Type 0** a mano
  (sin librerías): header `MThd`, track `MTrk`, delta-times variable-length,
  tempo meta-event, NoteOn/Off y CC64 (sustain). 480 ticks/beat, 120 BPM.

## Importadores (`src/import/FormatParser.ts`, `ImporterUI.tsx`)

| Formato | Librería | Notas |
|---|---|---|
| `.mid` / `.midi` | `midi-parser-js` | Tempo, firma, nombres de pista (right/melody → mano derecha), beats |
| `.mxl` / `.xml` | DOMParser nativo | Extracción básica: parts/measures, backup/forward, chords |
| `.abc` | `abcjs` | Aproximación de timing linear por eventos |
| `.json` | — | Formato interno de la app (auto-detectado) |

- Los tiempos de los importadores se normalizan a beats para el modelo interno.

## En el editor

El editor integra el mismo `FormatParser` para importar archivos directamente
("Archivo importado exitosamente"), y exporta grabación/MIDI/JSON/share link.
