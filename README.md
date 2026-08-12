# Piano-studio

> Professional virtual piano and practice workstation that runs 100 % in the browser: multi-layer synthesis with real samples, keyboard velocity physics, game modes with scoring, MIDI recording/export and a piano-roll editor to create lessons.

**Piano-studio v1.0**

---

## 1. Description

### Problem

Learning piano requires expensive hardware (an instrument or a MIDI controller), and web pianos usually sound synthetic and do not model dynamics. Practicing also lacks feedback: you don't know if you hit the right note, or with what precision.

### Solution

Piano-studio turns any browser into a piano with full-sample sound:

- **Real multi-sample engine**: three velocity layers of the Salamander Grand Piano via `Tone.Sampler`, with dynamic routing and minimal latency.
- **Hammer physics on QWERTY**: velocity is simulated by measuring the gap between keydowns (faster → louder), with configurable curves.
- **Practice feedback**: game mode with falling notes, timing-based scoring, combo and accuracy.
- **Complete creation flow**: record your performances, export them to MIDI, import `.mid`/`.mxl`/`.abc`/`.json` and edit them in a piano roll.
- **Real hardware**: Web MIDI support (controllers) and WebHID (IROK/MG75 magnetic keyboards).

### Target users

- Piano students practicing with a keyboard or MIDI controller.
- Teachers who build lessons in the editor and share them via link.
- Anyone who wants a quality piano in the browser without installing anything.

### Differentiators

- 3 real-sample velocity layers (not a simple synth).
- Custom DSP FX chain: procedural-IR reverb, tremolo, chorus, distortion, EQ and compression.
- Hammer-physics simulation on a QWERTY keyboard.
- Piano-roll editor with musical transforms (quantize, transpose, legato, chop, strum, chord stamp).

---

## 2. Badges

No build/test/version badges are configured in the repository (there is no
`npm test` or test CI). Instead, the real project state:

| State | Value |
|---|---|
| Deploy | ✅ GitHub Pages (workflow `pages.yml`) |
| Typecheck | `tsc --noEmit` (script `npm run lint`) |
| License | MIT |
| Status | Active development |

---

## 3. Visuals

The repository does not include UI screenshots or GIFs yet. The smoke test
(`scripts/runtime-smoke.py`) generates an automatic screenshot at
`tmp-runtime-smoke.png` when run; the live demo works as a visual:

- **Live demo**: https://cryaut.github.io/Piano-proyectado/

It would be useful to add in the future: a piano-roll capture, a game-mode GIF
and an audio-chain diagram.

---

## 4. Installation

### Requirements

- **Node.js 22+** (CI uses Node 22).
- **npm** (any recent version).
- A modern browser with Web Audio API (Chrome/Edge recommended for Web MIDI;
  Chrome for WebHID).
- Python 3 + Playwright **only** for the E2E smoke test (optional).

### Dependencies

```bash
npm install
```

### Environment configuration

The app runs without environment variables for local development.
`vite.config.ts` supports two optional variables:

```text
# Build for GitHub Pages (base /Piano-proyectado/)
GITHUB_PAGES=true

# Disables HMR and file-watching (agent-editing environments)
DISABLE_HMR=true
```

There is a `.env.example` file with placeholders (`GEMINI_API_KEY`, `APP_URL`)
for future integrations; no key is required to run the project, and real values
must never be committed to the repository.

### Database or service setup

No database or external services. Persistence is 100 % local in `localStorage`
(recordings, editor drafts, velocity settings). Audio samples load over the
network from `tonejs.github.io` (with an FM fallback if the network fails).

### Installation steps

```bash
# 1. Clone
git clone https://github.com/cryaut/Piano-proyectado.git
cd Piano-proyectado

# 2. Install dependencies
npm install

# 3. Start the dev server (port 3000)
npm run dev
```

Open `http://localhost:3000`.

---

## 5. Usage

### Main flow

1. **Click to start**: the app asks for a click to initialize the AudioContext.
2. **Free Mode**: play with the QWERTY keyboard (see mapping table) or connect a
   MIDI controller / HID keyboard from the header input icon.
3. **Play Mode**: pick a song, pick a mode (`Practice`, `Rhythm` or `Listen`)
   and play the notes falling onto the hit line.
4. **Recordings**: record your performance (`R` or button), play it back,
   export it to `.mid` or delete it.
5. **Editor**: draw notes in the piano roll, transform them, and export to
   JSON / MIDI / share link (base64 in the URL).

### QWERTY keyboard mapping (3 rows)

| Row | Keys | Notes |
|---|---|---|
| Bottom | `Z X C V B N M , . /` | `C3 D3 E3 F3 G3 A3 B3 C4 D4 E4` |
| Middle | `A S D F G H J K L ; '` | `F4 G4 A4 B4 C5 D5 E5 F5 G5 A5 B5` |
| Top | `Q W E R T Y U I O P [ ] \` | `C6 D6 E6 F6 G6 A6 B6 C7 D7 E7 F7 G7 A7` |

- **Shift** = black-key layer (sharps). `E` and `B` with Shift intentionally
  produce no note (no black key above). Details: `docs/keyboard-mapping.md`.

### Shortcuts

| Key | Action |
|---|---|
| `Space` | Sustain pedal |
| `←` / `→` | Lower / raise octave (−2..+2) |
| `F11` / `F` | Fullscreen |
| `M` | Global mute |
| `R` | Recording loop (free mode) |
| `Esc` | Close modals / exit fullscreen |
| `Ctrl+Z` / `Ctrl+Y` | Undo / Redo (editor) |

### Song JSON format

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

Full specification and examples (chords, sustain): `src/songs/formato.md`.
Timings can be expressed in beats or milliseconds (auto-detected).

---

## 6. Support

- **Issues and bugs**: GitHub Issues at https://github.com/cryaut/Piano-proyectado/issues
- **Technical documentation**: `docs/architecture.md`, `docs/keyboard-mapping.md`
- **Project context**: the `context/` directory (documented branch graph)

---

## 7. Roadmap

### Completed

- ✅ Multi-sample engine with 3 velocity layers + FM fallback.
- ✅ DSP chain (procedural reverb, compression, EQ, chorus, tremolo, distortion).
- ✅ QWERTY input (3 rows + Shift), Web MIDI, WebHID discovery.
- ✅ Game modes with scoring (combo, accuracy, misses).
- ✅ Recording + MIDI export (manual SMF Type 0) + local persistence.
- ✅ MIDI / MusicXML / ABC / JSON import.
- ✅ Piano-roll editor: draw/select/move/resize, quantize, transpose, legato,
  chop, strum, chord stamp, velocity lane, undo/redo, autosave, metronome.
- ✅ Song share link (base64 in URL) and GitHub Pages deploy.

### In progress

- 🔄 Polishing the game and editor experience.

### Planned (documented in `docs/architecture.md`)

- Real multi-track model (left/right hand, not just metadata).
- Command palette for the editor.
- Snapshots / version history.
- Loop region and punch-in in the editor.
- Better MIDI import normalization (tempo-map).
- Code splitting of the bundle.

### Future ideas

- Translating HID reports (IROK/MG75) into notes — it will probably be implemented in the next iteration of the bridge.
- Node/Express server (`server.js` already referenced by the `clean` script) to serve the app / Gemini API — it will probably be implemented later.

---

## 8. Contributing

Contributions are welcome. Suggested flow:

1. Fork the repo and use your own branch: `git checkout -b feature/your-thing`.
2. Commands your change must pass:

```bash
npm run lint        # tsc --noEmit (typecheck)
npx tsx scripts/verify-keyboard-map.ts   # mapping regression
npx tsx scripts/verify-editor-grid.ts    # editor grid regression
```

3. If you touch input/UI flows, run the smoke test (requires Playwright and a
   dev server on `:3000`):

```bash
python scripts/runtime-smoke.py
```

4. Submit a PR describing the change; commits with clear, short messages.

Conventions: strict TypeScript via `tsc`, React components without an external
store (shared state via singletons + `window` events), Spanish UI text for
users, English code/comments.

---

## 9. Authors and acknowledgment

**Author**: [cris angel](https://github.com/cryaut) (`cryaut`)

Thanks and credits:

- **Tone.js** — web audio engine.
- **Salamander Grand Piano** — samples by Alexander Holm, licensed under
  **Creative Commons Attribution 3.0 (CC BY 3.0)**.
- **abcjs** and **midi-parser-js** — ABC and MIDI parsing.
- **React**, **Vite**, **Tailwind CSS** and the frontend ecosystem.
- **GitHub Pages** — demo hosting.

---

## 10. License

**MIT License** — Copyright (c) 2026 cryaut. See [LICENSE](./LICENSE).

---

## 11. Project status

**Active development.**

Deployed to GitHub Pages with the main functionality complete and playable.
The areas marked in Roadmap are not finished yet.

---

# Appendices

## Technology Stack

| Technology | Version | Purpose |
|---|---|---|
| TypeScript | ~5.8.2 | Language |
| React | ^19.0.1 | UI |
| Vite | ^6.2.3 | Bundler / dev server |
| Tailwind CSS | ^4.1.14 | Styling (`@tailwindcss/vite` plugin) |
| Tone.js | ^15.1.22 | Audio engine (samples + FX) |
| abcjs | ^6.6.3 | ABC notation parser |
| midi-parser-js | ^4.0.4 | MIDI file parser |
| canvas-confetti | ^1.9.4 | Easter egg (C scale → confetti) |
| lucide-react | ^0.546.0 | Icons |
| motion | ^12.23.24 | Animations (framer-motion) |
| express / dotenv | ^4.21.2 / ^17.2.3 | Declared; Node server not implemented yet |
| @google/genai | ^2.4.0 | Google GenAI library (declared, unused in code yet) |
| Node.js | 22 (CI) | Runtime |
| GitHub Actions | — | CI/CD (Pages deploy) |

## Features

- Multi-sample engine with 3 velocity layers (L/M/H) and dynamics routing.
- FM fallback synth if samples fail to load over the network.
- DSP chain: procedural reverb (runtime-generated IR), tremolo, chorus,
  distortion, lowpass, parametric EQ3, compressor.
- 5 instrument presets with real FX changes.
- 3-row QWERTY input + Shift black-key layer + ±2 octaves.
- Velocity simulation by key-depression speed + configurable curves.
- Web MIDI (NoteOn/Off, CC64 sustain) and WebHID (IROK/MG75 detection).
- Game mode: falling notes, 3 modes (practice/rhythm/listen), scoring with
  combo and accuracy.
- Recording with sustain, local persistence (max 20), MIDI export (SMF Type 0).
- MIDI / MusicXML / ABC / JSON import normalized to beats.
- Piano-roll editor: lasso, move/resize, quantize, transpose, legato, chop,
  strum, chord stamp, velocity lane, ramp, humanize, undo/redo, metronome,
  autosave and recovery.
- Song share link (base64 in URL) and integrated game mode.
- Easter egg: C major scale in < 3 s → confetti.
- Key Tester debug panel to verify inputs.

## Architecture

See `context/02-architecture.md` and `docs/architecture.md` for details.

- **SPA frontend**: React + Vite + Tailwind.
- **Audio**: Tone.js outside the render thread; service singletons
  (`engine`, `keyHandler`, `songPlayer`, `scoringEngine`, `recorder`).
- **Input**: 3 sources (QWERTY / MIDI / HID) unified in `engine.noteOn`.
- **State**: shared via `window` `CustomEvent`s + `localStorage`.

```
Input (QWERTY|MIDI|HID) → PianoEngine → 3 Sampler/FM layers → FX bus
     → dry/wet → EQ → Compressor → Destination
```

## Project Structure

```
Piano-proyectado/
├── .github/workflows/pages.yml   # CI/CD GitHub Pages
├── assets/                       # static assets
├── context/                      # project context graph (md branches)
├── docs/
│   ├── architecture.md           # architecture and expansion notes
│   └── keyboard-mapping.md       # QWERTY mapping specification
├── scripts/
│   ├── verify-keyboard-map.ts    # mapping regression
│   ├── verify-editor-grid.ts     # grid math regression
│   └── runtime-smoke.py          # E2E smoke test (Playwright)
├── src/
│   ├── App.tsx                   # app shell, sections, shortcuts
│   ├── audio/                    # PianoEngine, presets, FX
│   ├── input/                    # KeyHandler, KeyboardMap, MIDI, HID, velocity
│   ├── game/                     # NoteHighway, SongPlayer, ScoringEngine
│   ├── editor/                   # SongEditor (piano roll), grid math
│   ├── record/                   # Recorder, controls, view
│   ├── import/                   # FormatParser (MIDI/XML/ABC/JSON)
│   ├── songs/                    # formato.md + twinkle.json
│   ├── ui/                       # visual keyboard, HUD, modals, presets
│   └── debug/                    # KeyTesterPanel, InputDebug
├── index.html
├── vite.config.ts
├── tsconfig.json
├── package.json
└── LICENSE
```

## Environment Variables

| Variable | Purpose | Required |
|---|---|---|
| `GITHUB_PAGES` | `true` → base `/Piano-proyectado/` (Pages deploy) | No (CI only) |
| `DISABLE_HMR` | `true` → disables HMR and file-watching | No |
| `GEMINI_API_KEY` | Placeholder in `.env.example`; future Gemini integration | No |
| `APP_URL` | Placeholder in `.env.example`; public deployment URL | No |

No secrets are required for local development. Reference: `.env.example`
(contains placeholders only).

## Testing

- **Typecheck**: `npm run lint` (`tsc --noEmit`).
- **Mapping regression**: `npx tsx scripts/verify-keyboard-map.ts`.
- **Grid regression**: `npx tsx scripts/verify-editor-grid.ts`.
- **E2E smoke** (Playwright, dev server on `:3000`): `python scripts/runtime-smoke.py`
  — verifies startup, QWERTY notes, Shift layer, UNMAPPED_BLACK_KEY and editor
  navigation.

There is no automated unit-test suite yet; verification relies on the scripts
above.

## Deployment

- **Target**: GitHub Pages.
- **Workflow**: `.github/workflows/pages.yml` — push to `main`/`master` or
  `workflow_dispatch` → Node 22, `npm ci`, `npm run build` with
  `GITHUB_PAGES=true`, upload of `dist/`, deploy.
- **Demo**: https://cryaut.github.io/Piano-proyectado/

## API Documentation

There is no public API in the repository. The app consumes audio assets over
the network (`tonejs.github.io`) and browser APIs (Web Audio, Web MIDI,
WebHID). The `express` dependency suggests a future Node server (it will
probably be implemented later); until then there are no endpoints.

## Database Schema

There is no database. Local persistence in `localStorage`:

| Key | Content |
|---|---|
| `realpiano_recordings` | Recordings (max 20) |
| `piano-hall-effect-settings` | Velocity/hall-effect settings |
| (editor draft) | Piano-roll autosave |

## Demo

- **Live**: https://cryaut.github.io/Piano-proyectado/
- **Local**: `npm install && npm run dev` → http://localhost:3000

## Known Limitations

- Samples (~80 MB) load over the network from a CDN; offline triggers the FM
  fallback (it sounds different).
- No backend or accounts: everything is local to the browser.
- Web MIDI requires Chrome/Edge; WebHID requires Chrome and a secure context
  (localhost or HTTPS).
- The HID bridge does not translate magnetic-keyboard reports into notes yet.
- No unit-test suite.
- UI is in Spanish in several places; mixed ES/EN documentation.
