# Rama 08 — Interfaz y Controles

## Componentes (`src/ui/`)

| Componente | Rol |
|---|---|
| `PianoKeyboard.tsx` | Teclado visual 5 octavas (C3..C8), teclas físicas mapeadas, layout preciso de blancas/negras |
| `InstrumentSelector.tsx` | Selección de preset (aplica FX reales, sin medidores falsos) |
| `HUD.tsx` | Overlay de stats en modo juego (combo, aciertos, errores) |
| `SettingsModal.tsx` | Selección de dispositivo MIDI / HID y settings de velocity (hall-effect) |
| `ControlPanel.tsx` | Controles de contexto (sustain toggle, fullscreen) |

## Aplicación (`src/App.tsx`)

- Secciones: **Libre** (free), **Jugar** (play), **Grabaciones**, **Editor**.
- Pantallas de arranque: click para iniciar AudioContext, "Calentando el piano…"
  con barra de progreso de samples.
- Header: logo, nav, BPM display (estático 090), control de octava, indicador de
  entrada activa (MIDI / HID / teclado).
- Footer: CPU LOAD, samples cargados (Tone.js | FMSynth Fallback), map label,
  estado del motor.
- Huevo de pascua: tocar la escala de C mayor (C4..C5) en < 3 s dispara confetti.

## Shortcuts globales

| Tecla | Acción |
|---|---|
| `Space` | Sustain pedal |
| `←` / `→` | Bajar / subir octava (−2..+2) |
| `F11` / `F` | Fullscreen |
| `M` | Mute global |
| `R` | Loop de grabación (modo libre) |
| `Esc` | Cerrar modales / salir de fullscreen |
| `Shift` (mantenido) | Capa de negras |
| `Ctrl+Z` / `Ctrl+Y` | Undo / Redo (editor) |

## Nota honesta

Los medidores simulados fueron removidos explícitamente para no mostrar
información falsa (comentario en `App.tsx`).
