# Contexto del Proyecto — Piano-studio

Este directorio es el grafo de contexto del proyecto. Cada archivo es una rama
del grafo y documenta un dominio del sistema. El README raíz se generó a partir
de este contexto.

## Ramas del grafo

| Rama | Archivo | Contenido |
|---|---|---|
| 01 | [01-project-overview.md](./01-project-overview.md) | Identidad, problema, usuarios, objetivos |
| 02 | [02-architecture.md](./02-architecture.md) | Arquitectura general, flujos y estado |
| 03 | [03-audio-engine.md](./03-audio-engine.md) | Motor de audio Tone.js, capas de velocity, FX |
| 04 | [04-input-systems.md](./04-input-systems.md) | Teclado QWERTY, MIDI, HID, simulación de velocidad |
| 05 | [05-game-mode.md](./05-game-mode.md) | Modo juego, scoring, canción y formato JSON |
| 06 | [06-song-editor.md](./06-song-editor.md) | Piano roll, transformaciones, autosave |
| 07 | [07-recording-import.md](./07-recording-import.md) | Grabación, export MIDI, importadores |
| 08 | [08-ui-components.md](./08-ui-components.md) | Componentes de interfaz y shortcuts |
| 09 | [09-build-deploy.md](./09-build-deploy.md) | Tooling, CI/CD, GitHub Pages |
| 10 | [10-testing.md](./10-testing.md) | Scripts de verificación y smoke test |
| 11 | [11-roadmap.md](./11-roadmap.md) | Estado actual y próximos pasos |

## Convenciones

- La fuente de verdad es el código en `src/` y `docs/`.
- Si una rama menciona algo no verificado, se marca como `Probablemente se implementará...`.
