# Morphogenesis for Max/MSP

A toolkit of **Max/MSP/Jitter abstractions, JavaScript (`v8`/`js`) objects, and
`gen~`/`jit.gen` patches** for *visualizing* and *sonifying* the morphogenesis
algorithms and natural-pattern techniques catalogued in Jason Webb's excellent
[**morphogenesis-resources**](https://github.com/jasonwebb/morphogenesis-resources)
repository.

The goal is to take the growth algorithms, math/physics models, and code
patterns described there — reaction-diffusion, differential growth,
diffusion-limited aggregation, Physarum, space colonization, phyllotaxis,
strange attractors, cellular automata, and many more — and make them playable,
patchable, and audible inside Max.

> **Status: Phase 0 — Analysis & Planning.**
> This repository currently contains a study of the source material and a
> proposed catalogue of Max/MSP objects to build. No objects have been
> implemented yet. See the docs below.

## Documentation

| Document | Purpose |
| --- | --- |
| [`docs/source-repo-analysis.md`](docs/source-repo-analysis.md) | What lives in `morphogenesis-resources`, and which parts are good candidates for Max. |
| [`docs/maxmsp-object-list.md`](docs/maxmsp-object-list.md) | The proposed catalogue of Max/MSP/Jitter/JS objects, with parameters and viz/sonification notes. |
| [`docs/roadmap.md`](docs/roadmap.md) | Phased build plan, naming conventions, and shared infrastructure. |

## Why Max/MSP?

These algorithms are a natural fit for Max:

- **Jitter** (`jit.matrix`, `jit.gl.*`, `jit.gen`, `jit.pix`) handles the
  grid- and field-based models (reaction-diffusion, cellular automata, flow
  fields) on CPU or GPU.
- **JavaScript** (`v8` / `v8ui`, or legacy `js` / `jsui`) is ideal for the
  agent- and graph-based models (differential growth, DLA, Physarum, space
  colonization, boids) that need dynamic data structures.
- **MSP** turns the resulting geometry and fields into sound — scanning paths,
  reading matrices as wavetables, driving oscillator banks from node positions,
  and using strange attractors directly as audio-rate generators.

## Proposed package layout

```
morphogenesis/
├── docs/            ← analysis, object catalogue, roadmap (this phase)
├── javascript/      ← v8/js source for agent- & graph-based objects
├── patchers/        ← .maxpat abstractions (the "objects" users instantiate)
├── gen/             ← gen~ / jit.gen patches for signal- & GPU-rate models
├── help/            ← .maxhelp files
├── examples/        ← demo patches combining viz + sonification
└── media/           ← shared assets
```

(Directories beyond `docs/` are placeholders for the implementation phases.)

## Attribution

All algorithm descriptions and the original curation are the work of
**Jason Webb** and contributors to
[morphogenesis-resources](https://github.com/jasonwebb/morphogenesis-resources)
(CC0-licensed). This project is an independent companion that adapts those
ideas into Max/MSP tooling.
