# Rama 03 — Motor de Audio

Archivos: `src/audio/PianoEngine.ts`, `src/audio/InstrumentPresets.ts`,
`src/audio/EffectsChain.ts` (placeholder vacío, la cadena vive en `PianoEngine`).

## Multi-sample por capas de velocity

- 3 × `Tone.Sampler` con los 30 archivos del **Salamander Grand Piano**
  (`https://tonejs.github.io/audio/salamander/`), ~80 MB en streaming.
- Routing por velocidad: `< 0.4` → capa **L** (filtro 2 kHz), `< 0.75` → capa **M**
  (4 kHz), resto → capa **H** (8 kHz). Volúmenes −12 / −6 / 0 dB.
- `Tone.getContext().lookAhead = 0.005` para ataque casi instantáneo.
- Re-ataque del mismo note: release inmediato de la frecuencia previa para evitar
  acumulación de voces.

## Fallback FM

- Si la red falla al cargar samples (`onerror`), se activa `Tone.PolySynth(Tone.FMSynth)`
  y el engine emite `piano-engine-error` + `piano-engine-ready`.
- La UI muestra "FMSynth Fallback" en la barra de estado.

## Cadena de efectos (master bus)

```
effectsBus → Tremolo(6Hz) → Chorus(4/2.5/0.5) → Distortion(0.4) → Lowpass(20kHz)
  ├─ dry: masterFilter → EQ3 (low/mid/high) 
  └─ wet: Convolver (IR procedural de 2.5 s) → Gain(0.15) → EQ3
EQ3 → Compressor (−18 dB, 3:1) → Tone.getDestination()
```

- IR de reverberación **procedural**: buffer de ruido con decaimiento exponencial
  generado en runtime (no depende de archivos externos).

## Presets de instrumento

5 presets en `InstrumentPresets.ts` que rampean parámetros reales de la cadena:

| id | nombre | carácter |
|---|---|---|
| `acoustic-grand` | Acoustic Grand | sample seco, sala media |
| `electric-piano` | Electric Piano | tremolo + chorus + lowpass |
| `soft-piano` | Soft Piano | ataque apagado, cola larga |
| `bright-piano` | Bright Piano | presencia 5 kHz, reverb corta |
| `stage-piano` | Stage Piano | compresión fuerte, saturación |

## Sustentación

- `setSustain()` mantiene un set de notas sostenidas; al soltar el pedal se
  liberan solo las que no siguen físicamente presionadas.
- Emite `piano-sustain-change`.

## API pública

`startAudioContext()`, `noteOn(note, velocity)`, `noteOff(note)`, `releaseAll()`,
`setSustain(bool)`, `applyPreset(id)`, `toggleMute()`, `isReady`.
