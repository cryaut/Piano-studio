# Rama 05 — Modo Juego

## Canción (`src/game/SongPlayer.ts`)

- `Song`: title, bpm, timeSignature, sustain blocks opcionales, notas con
  `start`/`duration` en **beats**, velocity, hand.
- Auto-detección de unidades: si los tiempos parecen milisegundos, convierte a
  beats con `60000 / bpm`.
- Modos: `practice`, `rhythm`, `listen` (UI en español: Practica / Ritmo / Escuchar).
- Pausa para práctica: detiene el reloj sin perder posición.
- Carga de canción compartida por URL (`?song=<base64>`), con confirmación.

## NoteHighway (`src/game/NoteHighway.tsx`)

- Notas que caen hacia una línea de golpe (falling notes).
- En `practice`/`rhythm` el jugador debe tocar; el error de timing se mide
  contra la línea de golpe.
- En `listen` reproduce solo, marcando las notas como perfect al pasar.
- Stats en HUD: combo, máx combo, precisión promedio (timingErrorMs).

## Scoring (`src/game/ScoringEngine.ts`)

- `registerHit(errorMs)` → correct++, combo++, acumula error medio.
- `registerMiss()` → missed++, combo = 0.
- Emite `scoring-update` para la UI.

## Formato de canción JSON

Especificado en `src/songs/formato.md`:

```json
{
  "title": "Melodía Simple",
  "bpm": 100,
  "timeSignature": [4, 4],
  "sustain": [{ "start": 0.0, "end": 3.0 }],
  "tracks": [
    {
      "instrument": "acoustic-grand",
      "notes": [
        { "pitch": "C4", "start": 0.0, "duration": 1.0, "velocity": 0.8 }
      ]
    }
  ]
}
```

- Canción por defecto: `src/songs/twinkle.json` (Twinkle Twinkle).
- Selección de canción en `SongSelectionView.tsx`.
