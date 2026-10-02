# Piano-studio

> Piano virtual profesional y práctica-workstation que corre 100 % en el navegador: síntesis multi-capa con samples reales, física de velocidad de teclado, modos de juego con scoring, grabación/exportación MIDI y un editor de piano roll para crear lecciones.

**Piano-studio v1.0**

---

## 1. Descripción

### Problema

Aprender piano exige hardware caro (instrumento o controlador MIDI), y los
pianos web suelen sonar sintéticos y no modelan dinámica. Practicar además
carece de feedback: no sabes si tocaste la nota correcta, ni con qué precisión.

### Solución

Piano-studio convierte cualquier navegador en un piano con sonido de
samples de cola:

- **Motor multi-sample real**: tres capas de velocidad del Salamander Grand
  Piano vía `Tone.Sampler`, con routing dinámico y latencia mínima.
- **Física de martillo sobre QWERTY**: la velocity se simula midiendo el gap
  entre keydowns (más rápido → más fuerte), con curvas configurables.
- **Feedback de práctica**: modo juego con notas que caen, scoring por timing,
  combo y precisión.
- **Flujo completo de creación**: grabá tus interpretaciones, exportalas a
  MIDI, importá `.mid`/`.mxl`/`.abc`/`.json` y editalas en un piano roll.
- **Hardware real**: soporte Web MIDI (controladores) y WebHID (teclados
  magnéticos IROK/MG75).

### Usuarios objetivo

- Estudiantes de piano que practican con teclado o controlador MIDI.
- Profesores que arman lecciones en el editor y las comparten por link.
- Cualquier persona que quiera un piano de calidad en el navegador sin instalar nada.

### Diferenciadores

- 3 capas de velocity con samples reales (no un simple synth).
- Cadena de FX DSP propia: reverb por IR procedural, tremolo, chorus, distorsión, EQ y compresión.
- Simulación de física de martillo en teclado QWERTY.
- Editor de piano roll con transformaciones musicales (quantize, transpose, legato, chop, strum, chord stamp).

---

## 2. Badges

No hay badges de build/test/version configurados en el repositorio (no hay
`npm test` ni CI de tests). En su lugar, el estado real del proyecto:

| Estado | Valor |
|---|---|
| Deploy | ✅ GitHub Pages (workflow `pages.yml`) |
| Typecheck | `tsc --noEmit` (script `npm run lint`) |
| License | MIT |
| Status | Desarrollo activo |

---

## 3. Visuals

El repositorio no incluye screenshots o GIFs de la UI todavía. El smoke test
(`scripts/runtime-smoke.py`) genera un screenshot automático en
`tmp-runtime-smoke.png` cuando se ejecuta; la demo en vivo funciona como
visual:

- **Demo en vivo**: https://cryaut.github.io/Piano-proyectado/

Sería útil agregar en el futuro: captura del piano roll, GIF del modo juego y
diagrama de la cadena de audio.

---

## 4. Installation

### Requirements

- **Node.js 22+** (la CI usa Node 22).
- **npm** (cualquier versión reciente).
- Navegador moderno con Web Audio API (Chrome/Edge recomendados para Web MIDI;
  Chrome para WebHID).
- Python 3 + Playwright **solo** para el smoke test E2E (opcional).

### Dependencies

```bash
npm install
```

### Environment configuration

La app funciona sin variables de entorno para el desarrollo local.
`vite.config.ts` admite dos variables opcionales:

```text
# Build para GitHub Pages (base /Piano-proyectado/)
GITHUB_PAGES=true

# Desactiva HMR y file-watching (entornos de edición agéntica)
DISABLE_HMR=true
```

Existe un archivo `.env.example` con placeholders (`GEMINI_API_KEY`, `APP_URL`)
para integraciones futuras; ninguna clave es obligatoria para correr el proyecto
y los valores reales nunca deben subirse al repositorio.

### Database or service setup

Sin base de datos ni servicios externos. La persistencia es 100 % local en
`localStorage` (grabaciones, borradores del editor, settings de velocity).
Los samples de audio se cargan por red desde `tonejs.github.io` (con fallback
FM si la red falla).

### Installation steps

```bash
# 1. Clonar
git clone https://github.com/cryaut/Piano-proyectado.git
cd Piano-proyectado

# 2. Instalar dependencias
npm install

# 3. Levantar el dev server (puerto 3000)
npm run dev
```

Abrir `http://localhost:3000`.

---

## 5. Usage

### Flujo principal

1. **Click para empezar**: la app pide un click para inicializar el AudioContext.
2. **Modo Libre**: tocá con el teclado QWERTY (ver tabla de mapeo) o conectá un
   controlador MIDI / teclado HID desde el ícono de entrada del header.
3. **Modo Jugar**: elegí una canción, elegí modo (`Practica`, `Ritmo` o
   `Escuchar`) y tocá las notas que caen sobre la línea de golpe.
4. **Grabaciones**: grabá tu interpretación (`R` o botón), reproducila,
   exportala a `.mid` o borrala.
5. **Editor**: dibujá notas en el piano roll, transformalas, y exportá a JSON /
   MIDI / share link (base64 en la URL).

### Mapeo del teclado QWERTY (3 filas)

| Fila | Teclas | Notas |
|---|---|---|
| Inferior | `Z X C V B N M , . /` | `C3 D3 E3 F3 G3 A3 B3 C4 D4 E4` |
| Media | `A S D F G H J K L ; '` | `F4 G4 A4 B4 C5 D5 E5 F5 G5 A5 B5` |
| Superior | `Q W E R T Y U I O P [ ] \` | `C6 D6 E6 F6 G6 A6 B6 C7 D7 E7 F7 G7 A7` |

- **Shift** = capa de negras (sostenidos). `E` y `B` con Shift no suenan a
  propósito (no hay negra encima). Detalle: `docs/keyboard-mapping.md`.

### Shortcuts

| Tecla | Acción |
|---|---|
| `Space` | Sustain pedal |
| `←` / `→` | Bajar / subir octava (−2..+2) |
| `F11` / `F` | Fullscreen |
| `M` | Mute global |
| `R` | Loop de grabación (modo libre) |
| `Esc` | Cerrar modales / salir de fullscreen |
| `Ctrl+Z` / `Ctrl+Y` | Undo / Redo (editor) |

### Formato de canción JSON

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

Especificación completa y ejemplos (acordes, sustain): `src/songs/formato.md`.
Los tiempos pueden venir en beats o milisegundos (auto-detectado).

---

## 6. Support

- **Issues y bugs**: GitHub Issues en https://github.com/cryaut/Piano-proyectado/issues
- **Documentación técnica**: `docs/architecture.md`, `docs/keyboard-mapping.md`
- **Contexto del proyecto**: directorio `context/` (grafo de ramas documentadas)

---

## 7. Roadmap

### Completado

- ✅ Engine multi-sample con 3 capas de velocity + fallback FM.
- ✅ Cadena DSP (reverb procedural, compresión, EQ, chorus, tremolo, distorsión).
- ✅ Entrada QWERTY (3 filas + Shift), Web MIDI, descubrimiento WebHID.
- ✅ Modos de juego con scoring (combo, precisión, misses).
- ✅ Grabación + export MIDI (SMF Type 0 manual) + persistencia local.
- ✅ Import MIDI / MusicXML / ABC / JSON.
- ✅ Editor piano roll: draw/select/move/resize, quantize, transpose, legato,
  chop, strum, chord stamp, velocity lane, undo/redo, autosave, metrónomo.
- ✅ Share link (canción en base64 por URL) y deploy GitHub Pages.

### En progreso

- 🔄 Pulido de la experiencia de juego y del editor.

### Planeado (documentado en `docs/architecture.md`)

- Modelo multi-track (mano izquierda/derecha real, no solo metadata).
- Command palette para el editor.
- Snapshots / historial de versiones.
- Loop region y punch-in en el editor.
- Mejor normalización del import MIDI (tempo-map).
- Code splitting del bundle.

### Ideas futuras

- Traducción de reportes HID (IROK/MG75) a notas — probablemente se implementará en la siguiente iteración del puente.
- Servidor Node/Express (`server.js` ya referenciado en el script `clean`) para servir la app / API Gemini — probablemente se implementará más adelante.

---

## 8. Contributing

Las contribuciones son bienvenidas. Flujo sugerido:

1. Fork del repo y rama propia: `git checkout -b feature/lo-que-sea`.
2. Comandos que debe pasar el cambio:

```bash
npm run lint        # tsc --noEmit (typecheck)
npx tsx scripts/verify-keyboard-map.ts   # regresión de mapeo
npx tsx scripts/verify-editor-grid.ts    # regresión de grid del editor
```

3. Si tocás flujos de entrada/UI, corré el smoke test (requiere Playwright y
   dev server en `:3000`):

```bash
python scripts/runtime-smoke.py
```

4. Enviá un PR describiendo el cambio; commits en mensajes claros y cortos.

Convenciones: TypeScript estricto vía `tsc`, componentes React sin store
externo (estado compartido por singletons + eventos de `window`), UI en español
para texto de usuario, código/comentarios en inglés.

---

## 9. Authors and acknowledgment

**Autor**: [cris angel](https://github.com/cryaut) (`cryaut`)

Agradecimientos y créditos:

- **Tone.js** — motor de audio web.
- **Salamander Grand Piano** — samples de Alexander Holm, licencia
  **Creative Commons Attribution 3.0 (CC BY 3.0)**.
- **abcjs** y **midi-parser-js** — parsing de ABC y MIDI.
- **React**, **Vite**, **Tailwind CSS** y el ecosistema frontend.
- **GitHub Pages** — hosting de la demo.

---

## 10. License

**MIT License** — Copyright (c) 2026 cryaut. Ver [LICENSE](./LICENSE).

---

## 11. Project status

**Desarrollo activo.**

Desplegado en GitHub Pages con la funcionalidad principal completa y jugable.
Las áreas marcadas en Roadmap aún no están cerradas.

---

# Apéndices

## Technology Stack

| Tecnología | Versión | Propósito |
|---|---|---|
| TypeScript | ~5.8.2 | Lenguaje |
| React | ^19.0.1 | UI |
| Vite | ^6.2.3 | Bundler / dev server |
| Tailwind CSS | ^4.1.14 | Estilos (plugin `@tailwindcss/vite`) |
| Tone.js | ^15.1.22 | Motor de audio (samples + FX) |
| abcjs | ^6.6.3 | Parser de notación ABC |
| midi-parser-js | ^4.0.4 | Parser de archivos MIDI |
| canvas-confetti | ^1.9.4 | Easter egg (escala de C → confetti) |
| lucide-react | ^0.546.0 | Iconos |
| motion | ^12.23.24 | Animaciones (framer-motion) |
| express / dotenv | ^4.21.2 / ^17.2.3 | Declarados; servidor Node aún no implementado |
| @google/genai | ^2.4.0 | Librería de Google GenAI (declarada, sin uso en el código aún) |
| Node.js | 22 (CI) | Runtime |
| GitHub Actions | — | CI/CD (deploy Pages) |

## Features

- Multi-sample engine con 3 capas de velocity (L/M/H) y routing por dinámica.
- Fallback FM sintetizado si la red falla al cargar samples.
- Cadena DSP: reverb procedural (IR generado en runtime), tremolo, chorus,
  distorsión, lowpass, EQ3 paramétrico, compresor.
- 5 presets de instrumento con cambios de FX reales.
- Entrada QWERTY de 3 filas + capa Shift de negras + octavas ±2.
- Simulación de velocity por velocidad de depresión + curvas configurables.
- Web MIDI (NoteOn/Off, sustain CC64) y WebHID (detección IROK/MG75).
- Modo juego: falling notes, 3 modos (práctica/ritmo/escucha), scoring con
  combo y precisión.
- Grabación con sustain, persistencia local (máx 20), export MIDI (SMF Type 0).
- Import MIDI / MusicXML / ABC / JSON con normalización a beats.
- Editor piano roll: lasso, mover/redimensionar, quantize, transpose, legato,
  chop, strum, chord stamp, velocity lane, ramp, humanize, undo/redo, metrónomo,
  autosave y recuperación.
- Share link de canciones (base64 en URL) y modo juego integrado.
- Easter egg: escala de C mayor en < 3 s → confetti.
- Key Tester panel de debug para verificar inputs.

## Architecture

Ver `context/02-architecture.md` y `docs/architecture.md` para el detalle.

- **Frontend SPA**: React + Vite + Tailwind.
- **Audio**: Tone.js por fuera del hilo de render; singletons de servicio
  (`engine`, `keyHandler`, `songPlayer`, `scoringEngine`, `recorder`).
- **Entrada**: 3 fuentes (QWERTY / MIDI / HID) unificadas en `engine.noteOn`.
- **Estado**: compartido por eventos `CustomEvent` de `window` + `localStorage`.

```
Input (QWERTY|MIDI|HID) → PianoEngine → 3 capas Sampler/FM → FX bus
     → dry/wet → EQ → Compressor → Destination
```

## Project Structure

```
Piano-proyectado/
├── .github/workflows/pages.yml   # CI/CD GitHub Pages
├── assets/                       # recursos estáticos
├── context/                      # grafo de contexto del proyecto (ramas md)
├── docs/
│   ├── architecture.md           # notas de arquitectura y expansión
│   └── keyboard-mapping.md       # especificación del mapeo QWERTY
├── scripts/
│   ├── verify-keyboard-map.ts    # regresión de mapeo
│   ├── verify-editor-grid.ts     # regresión de grid math
│   └── runtime-smoke.py          # smoke test E2E (Playwright)
├── src/
│   ├── App.tsx                   # shell de la app, secciones, shortcuts
│   ├── audio/                    # PianoEngine, presets, FX
│   ├── input/                    # KeyHandler, KeyboardMap, MIDI, HID, velocity
│   ├── game/                     # NoteHighway, SongPlayer, ScoringEngine
│   ├── editor/                   # SongEditor (piano roll), grid math
│   ├── record/                   # Recorder, controles, vista
│   ├── import/                   # FormatParser (MIDI/XML/ABC/JSON)
│   ├── songs/                    # formato.md + twinkle.json
│   ├── ui/                       # teclado visual, HUD, modales, presets
│   └── debug/                    # KeyTesterPanel, InputDebug
├── index.html
├── vite.config.ts
├── tsconfig.json
├── package.json
└── LICENSE
```

## Environment Variables

| Variable | Uso | Obligatoria |
|---|---|---|
| `GITHUB_PAGES` | `true` → base `/Piano-proyectado/` (deploy Pages) | No (solo CI) |
| `DISABLE_HMR` | `true` → desactiva HMR y file-watching | No |
| `GEMINI_API_KEY` | Placeholder en `.env.example`; integración Gemini futura | No |
| `APP_URL` | Placeholder en `.env.example`; URL pública de despliegue | No |

No se requieren secrets para el desarrollo local. Referencia: `.env.example`
(contiene solo placeholders).

## Testing

- **Typecheck**: `npm run lint` (`tsc --noEmit`).
- **Regresión de mapeo**: `npx tsx scripts/verify-keyboard-map.ts`.
- **Regresión de grid**: `npx tsx scripts/verify-editor-grid.ts`.
- **Smoke E2E** (Playwright, dev server en `:3000`): `python scripts/runtime-smoke.py`
  — verifica arranque, notas QWERTY, capa Shift, UNMAPPED_BLACK_KEY y navegación
  al editor.

No existe suite de tests unitarios automatizada aún; la verificación se apoya
en los scripts anteriores.

## Deployment

- **Target**: GitHub Pages.
- **Workflow**: `.github/workflows/pages.yml` — push a `main`/`master` o
  `workflow_dispatch` → Node 22, `npm ci`, `npm run build` con
  `GITHUB_PAGES=true`, upload de `dist/`, deploy.
- **Demo**: https://cryaut.github.io/Piano-proyectado/

## API Documentation

No hay API pública en el repositorio. La aplicación consume assets de audio por
red (`tonejs.github.io`) y APIs del navegador (Web Audio, Web MIDI, WebHID).
La dependencia `express` sugiere un futuro servidor Node (probablemente se
implementará más adelante); hasta entonces no existen endpoints.

## Database Schema

No hay base de datos. Persistencia local en `localStorage`:

| Key | Contenido |
|---|---|
| `realpiano_recordings` | Grabaciones (máx 20) |
| `piano-hall-effect-settings` | Settings de velocity/hall-effect |
| (borrador del editor) | Autosave del piano roll |

## Demo

- **Live**: https://cryaut.github.io/Piano-proyectado/
- **Local**: `npm install && npm run dev` → http://localhost:3000

## Known Limitations

- Los samples (~80 MB) se cargan por red desde un CDN; sin conexión se activa
  el fallback FM (suena diferente).
- Sin backend ni cuentas: todo es local al navegador.
- Web MIDI requiere Chrome/Edge; WebHID requiere Chrome y una conexión segura
  (localhost o HTTPS).
- El puente HID aún no traduce reportes de teclado magnético a notas.
- Sin suite de tests unitarios.
- Interfaz en español en varios puntos; documentación mixta ES/EN.
