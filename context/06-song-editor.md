# Rama 06 — Editor de Piano Roll

Archivos: `src/editor/SongEditor.tsx` (~1570 líneas), `src/editor/EditorGridMath.ts`.

## Estrategia UX

Comporta como un piano roll MIDI compacto, no como un formulario:
dibujar notas en un grid de beats/altura, seleccionar/mover/redimensionar.

## Funcionalidad implementada

- **Grid**: pintado en `<canvas>` con shading de escala (C mayor destacada),
  zoom horizontal, grid shading.
- **Edición**: crear (drag), mover, redimensionar, borrar, selección (click, lasso),
  copiar/pegar con offset, clipboard interno.
- **Transformaciones**:
  - **Quantize** (paso configurable, default 0.25)
  - **Transpose** ±1 semitono / ±1 octava
  - **Legato** (une notas tocándose)
  - **Chop por grid** (divide en piezas del paso)
  - **Strum** (desfase de 0.035 beats)
  - **Chord stamp**: maj, min, sus4, dom7, maj7, min7
- **Expresión**: velocity lane interactiva, ramp, humanize (jitter ±0.1),
  inspector con velocity por selección.
- **Undo/Redo** (`Ctrl+Z` / `Ctrl+Y`) y **metrónomo**.
- **Inspector**: pitch, start, duration, velocity, asignación de mano
  (left/right), borrar y duplicar nota.
- **Autosave**: borrador guardado en `localStorage` con recuperación manual.
- **Playback local** de la canción editada con el engine.
- **Export**: JSON descargable, MIDI, share link (base64 en URL) y pasar a modo
  juego.
- **Import**: desde el editor (ver rama 07).

## Grid math

`EditorGridMath.ts` resuelve punto de canvas ↔ beat/pitch con factor de zoom;
verificación en `scripts/verify-editor-grid.ts`.

## Pendiente (documentado en docs/architecture.md)

- Multi-track real (izquierda/derecha) en vez de solo metadata de mano.
- Command palette, snapshots/historial de versiones, loop region, punch-in.
