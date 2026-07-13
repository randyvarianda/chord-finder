# Chord Finder 🎸

Detect chords from any song, right in your browser. Drop in an audio file and get a color-coded chord timeline synced to playback — no server, no upload, everything runs client-side.

**Live demo:** https://randyvarianda.github.io/chord-finder/

![Chord Finder](https://img.shields.io/badge/audio-client--side-blue) ![No dependencies](https://img.shields.io/badge/dependencies-none-green)

## Features

- 🎵 **Drag & drop** any audio file (mp3, wav, m4a, ogg)
- 🔒 **100% local** — audio never leaves your browser
- ⏱️ **Chord timeline** — blocks proportional to duration, click to seek
- 🎹 **Now-playing panel** — current chord updates in sync with playback, with next-chord preview
- 🌈 **Circle-of-fifths colors** — harmonically related chords share neighboring hues, so you can *see* when a song stays in key or modulates

## How it works

1. **Decode** — Web Audio API decodes the file to raw PCM, mixed down to mono and decimated to ~11 kHz
2. **Chromagram** — a Goertzel filterbank measures energy at every note from C2 to B5, folded into 12 pitch classes per frame
3. **Template matching** — each frame is compared (cosine similarity) against 24 chord templates: 12 major + 12 minor triads
4. **Smoothing** — temporal median filtering, silence gating (N.C.), and minimum-duration merging turn noisy frame-level guesses into clean chord segments

No ML model, no external libraries — the whole thing is one HTML file.

## Usage

Open the [live demo](https://randyvarianda.github.io/chord-finder/), or clone and open locally:

```bash
git clone https://github.com/randyvarianda/chord-finder.git
open chord-finder/index.html
```

No build step. No dependencies.

## Limitations

This is a classic signal-processing approach, so set expectations accordingly:

- ✅ Works well: clean recordings, pop/folk/acoustic songs with clear triads
- ⚠️ Struggles with: dense mixes, heavy vocals, distorted guitars
- ❌ Not detected: 7th chords, extensions (maj9, 13), inversions, slash chords — these get simplified to their nearest major/minor triad

For higher accuracy, the same pipeline can be upgraded with a pretrained deep learning model (e.g. [madmom](https://github.com/CPJKU/madmom)) or by running source separation ([Demucs](https://github.com/facebookresearch/demucs)) first — but that requires a backend.

## License

MIT
