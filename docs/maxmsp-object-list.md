# Proposed Max/MSP Object Catalogue

This is the initial, working list of objects (abstractions, `v8`/`js` externals,
and `gen~`/`jit.gen` patches) to build from the morphogenesis-resources material.
Each entry lists the **kind**, **key inlets/parameters**, **outputs**, and
**viz + sonification** notes.

## Conventions

- **Prefix:** `mgen.` for control/visual objects, `mgen.*~` for audio-rate
  (MSP) objects, `jit.mgen.*` for Jitter/matrix objects. (Following Max's own
  `jit.*` / `*~` naming idioms.)
- **Kind** legend:
  - `JS` — JavaScript object (`v8`, fallback `js`)
  - `JIT` — Jitter object/abstraction (matrix or GL)
  - `GEN` — `gen~` (audio) or `jit.gen`/`jit.pix` (GPU/matrix) patch
  - `ABS` — `.maxpat` abstraction wrapping the above
- Every object ships a `.maxhelp` and credits the source topic.

---

## Tier 1 — Flagship growth algorithms (highest priority)

These are the marquee algorithms: visually rich, parameter-deep, and
sonically expressive. Each is a candidate "hero" object.

### `jit.mgen.rd` — Reaction-Diffusion (Gray-Scott)
- **Kind:** GEN (`jit.gen` / `jit.pix`, GPU) with `jit.matrix` feedback.
- **Params:** `feed` (f), `kill` (k), `dA`, `dB` (diffusion rates), `dt`,
  `seed`/`brush` (paint chemical B), `boundary` (wrap/clamp), `preset`
  (spots/stripes/maze/mitosis).
- **Inlets:** matrix in (state feedback), brush messages, param floats.
- **Outputs:** 2-plane float32 matrix (A,B); plus a thresholded/colormapped
  display matrix.
- **Viz:** colormap B-concentration; render in `jit.pwindow`/`jit.gl`.
- **Sonify:** scan a matrix row as a wavetable; map mean concentration → filter
  cutoff; treat the A/B oscillation rate as an LFO. Pattern "type" → timbre.

### `mgen.diffgrow` — Differential Growth
- **Kind:** JS (graph of nodes) → outputs geometry for `jit.gl.path`/`jsui`.
- **Params:** `attraction`, `repulsion`, `alignment`, `maxForce`, `maxSpeed`,
  `repulsionRadius`, `splitDistance` (node insertion), `mergeDistance`,
  `growthRate`, `closed` (open chain vs loop), `bounds`.
- **Inlets:** `bang` to step; seed shape (circle/line/from-list); params.
- **Outputs:** list/matrix of node positions; node count; insertion events.
- **Viz:** draw the undulating polyline/mesh (vector).
- **Sonify:** node count → density; per-node curvature → pitch; insertion
  events → note triggers; total path length → drone parameter.

### `mgen.dla` — Diffusion-Limited Aggregation
- **Kind:** JS (random walkers + cluster) + spatial index helper.
- **Params:** `walkers`, `stepSize`, `stickiness`, `seedShape` (point/line/circle),
  `spawnRadius`, `killRadius`, `maxParticles`.
- **Outputs:** cluster point list; "stick" event (position + generation index).
- **Viz:** accreting branching crystal (points/lines).
- **Sonify:** each stick = a note/grain; map stick radius → pitch, generation →
  timbre; aggregation rate → tempo/rhythm.

### `jit.mgen.physarum` — Physarum (slime mold)
- **Kind:** GEN agents-on-matrix (`jit.gen` for trail diffuse/decay) + JS or
  TFL-style agent update.
- **Params:** `agents`, `sensorAngle`, `sensorDistance`, `rotationAngle`,
  `stepSize`, `deposit`, `decay`, `diffuse`, `seed`.
- **Outputs:** trail-map matrix; agent positions matrix.
- **Viz:** glowing network/veins (matrix render).
- **Sonify:** trail-map brightness histogram → spectral envelope; network
  connectivity → reverb/feedback; scan trail map as audio.

### `mgen.spacecol` — Space Colonization
- **Kind:** JS (tree graph + attractor cloud + spatial index).
- **Params:** `attractors` (count/distribution or list), `attractionDist`,
  `killDist`, `segmentLength`, `root`(s), `bounds`.
- **Outputs:** branch segment list (tree edges); branch/leaf events.
- **Viz:** vein/tree/coral branching networks.
- **Sonify:** branching events → polyphonic note-ons; tree depth → register;
  number of active growth tips → voice count.

### `jit.mgen.dbm` — Dielectric Breakdown Model
- **Kind:** GEN/JS (Laplace potential field + probabilistic growth, η exponent).
- **Params:** `eta` (branchiness), `gridSize`, `seed`, `boundary`, `relaxIters`.
- **Outputs:** growth-site matrix; discharge path events.
- **Viz:** lightning / Lichtenberg figures.
- **Sonify:** discharge events → transient/percussive hits; η → density;
  branch reach → pitch sweep.

### `jit.mgen.eden` — Eden Growth Model
- **Kind:** JIT/JS (boundary accretion on a grid).
- **Params:** `growthProb`, `seed`, `gridSize`, `neighborhood` (4/8).
- **Outputs:** occupancy matrix; perimeter length.
- **Viz:** rough fractal blob growth.
- **Sonify:** perimeter roughness → noise amount; growth rate → tempo.

### `mgen.primordial` — Primordial Particle System
- **Kind:** JS (particles + neighbor-count rule).
- **Params:** `alpha`, `beta`, `velocity`, `radius`, `particles`.
- **Outputs:** particle positions + state (neighbor count → color).
- **Viz:** life-like cell clusters / rotating clumps.
- **Sonify:** cluster sizes → chord voicings; global order parameter → harmonic
  tension; cell color states → timbral classes.

---

## Tier 2 — Parametric & math models (great for direct sonification)

These have closed-form or low-state definitions and translate beautifully to
audio-rate `gen~`.

### `mgen.superformula` / `mgen.superformula~`
- **Kind:** JS (curve) + GEN (audio-rate radius generator).
- **Params:** `m`, `n1`, `n2`, `n3`, `a`, `b`, `phase`, `points`.
- **Outputs:** 2D/3D curve points; or audio-rate `r(θ)`.
- **Viz:** organic shapes (starfish, flowers, shells).
- **Sonify:** evaluate `r(θ)` at audio rate as a waveshaper/wavetable;
  morph `m,n1..3` → timbral morph.

### `mgen.lissajous~`
- **Kind:** GEN (audio-rate x/y oscillator pair).
- **Params:** `freqX`, `freqY`, `phase`, `ratio`.
- **Outputs:** stereo signal (x,y) — usable as oscilloscope XY *and* audio.
- **Viz:** classic Lissajous figures (XY scope).
- **Sonify:** *is* sound — two related sine tones; ratio → consonance.

### `mgen.phyllotaxis`
- **Kind:** JS (golden-angle point placement).
- **Params:** `n` (points), `angle` (≈137.5°), `c` (scale), `spread`.
- **Outputs:** spiral point cloud (positions + index).
- **Viz:** sunflower/pinecone spirals; seed packing.
- **Sonify:** spiral index → arpeggio; radius → pan/amplitude; angle detune
  → microtonal sweep.

### `mgen.attractor~` — Strange Attractors
- **Kind:** GEN (audio-rate ODE integrators: Lorenz, Rössler, Thomas, Clifford,
  De Jong, Aizawa…).
- **Params:** attractor `type`, system constants, `dt`, `scale`.
- **Outputs:** 2–3 signal channels (x,y,z).
- **Viz:** phase-space plots (XY/3D).
- **Sonify:** chaotic audio-rate generator; low `dt` → drones, high → noise;
  use channels as cross-modulators.

### `mgen.fourier` / `mgen.fourier~` — Fourier Series / Epicycles
- **Kind:** JS (epicycle path) + GEN (additive synth from the same coefficients).
- **Params:** harmonic `coeffs` (amp/phase list), `terms`, `speed`.
- **Outputs:** traced curve; additive audio.
- **Viz:** rotating epicycles drawing a path.
- **Sonify:** the very same harmonic series as additive synthesis — visual and
  audio share coefficients.

### `mgen.superellipse`, `mgen.fibonacci`, `mgen.goldenangle`
- **Kind:** JS utility/curve objects feeding the above.
- Smaller helpers; bundled as a parametric-shapes library.

---

## Tier 3 — Cellular / field & lab-experiment models

### `jit.mgen.ca` — Cellular Automata
- **Kind:** GEN (`jit.gen`/`jit.pix`).
- **Params:** `rule` (Life/B-S notation or 1D rule number), `dim` (1D/2D),
  `neighborhood`, `wrap`, `seed`.
- **Viz:** Game of Life, elementary CA, totalistic rules.
- **Sonify:** column-as-spectrum (1D CA → spectral synth); live/death counts →
  rhythm; rows as step sequencer.

### `jit.mgen.bz~` — Belousov–Zhabotinsky / oscillating reaction
- **Kind:** GEN (3-species excitable-medium update).
- **Params:** `alpha`, `beta`, `gamma`, `dt`, `seed`.
- **Viz:** rotating spiral waves.
- **Sonify:** local oscillation phase → LFO bank; spiral rotation → tremolo/pan.

### `jit.mgen.chladni` — Chladni Plate
- **Kind:** GEN (standing-wave nodal pattern) — *sound-driven by design*.
- **Params:** plate `mode` (m,n), `freq`, `damping`.
- **Inlet:** audio/frequency in → pattern out (the experiment runs "backwards"
  too: drive geometry from incoming pitch).
- **Viz:** nodal line figures.
- **Sonify:** bidirectional — frequency ↔ pattern is the whole point.

### `jit.mgen.heleshaw` — Hele-Shaw / viscous fingering (Saffman–Taylor)
- **Kind:** GEN (pressure-driven interface growth).
- **Params:** `viscosityRatio`, `injectionRate`, `surfaceTension`, `seed`.
- **Viz:** branching fingering fronts.
- **Sonify:** finger count → partial count; front velocity → glissando.

---

## Tier 4 — Computational-geometry & pattern toolkit (shared infrastructure)

Reusable objects that both stand alone and power the Tier 1–3 algorithms.

| Object | Kind | Purpose | Sonification hook |
| --- | --- | --- | --- |
| `mgen.voronoi` / `mgen.delaunay` | JS | Voronoi cells / Delaunay mesh from points | cell area → grain size; edges → connection graph |
| `mgen.lloyd` | JS | Lloyd's relaxation (even point spacing) | relaxation energy → settling envelope |
| `mgen.metaball` | JIT/GEN | Metaball field + iso-threshold | field value → amplitude map |
| `mgen.sdf` | GEN | Signed distance field of shapes | distance → filter/delay modulation |
| `jit.mgen.march` | JIT/JS | Marching squares (iso-contours) | contour length → density |
| `jit.mgen.flow` | GEN | Flow field (noise-driven vectors) | direction histogram → spatial pan field |
| `mgen.boids` | JS | Flocking (alignment/cohesion/separation) | flock cohesion → chord tightness; agents → voices |
| `mgen.particles` | JS | General particle system w/ forces | particle events → granular triggers |
| `mgen.spatialhash` | JS | Spatial index (neighbor queries) | (infrastructure, used by agent objects) |
| `mgen.tsp` | JS | Travelling-salesman path through points | tour order → melodic sequence |
| `mgen.medialaxis` | JS | Skeleton/medial axis of a shape | branch points → trigger map |
| `mgen.noise` | GEN | Perlin/simplex/curl noise utility | (infrastructure) |
| `jit.mgen.dither` | JIT | Dithering / halftone of a matrix | dot density → rhythmic density |

---

## Sonification utility objects (shared across all of the above)

These bridge geometry/fields → MSP so every visual object can make sound the
same way:

- **`mgen.scan~`** — scan a path or matrix row/column at a given rate, output as
  an audio-rate signal (wavetable readout). Inlets: matrix/list, rate, interp.
- **`mgen.field2sig~`** — read a `jit.matrix` cell (or region average) as a
  control/audio signal (`jit.peek~`-based), with smoothing.
- **`mgen.nodes2voices`** — distribute a node/point list across `poly~` voices
  (position → pitch/pan/amp mapping, voice stealing).
- **`mgen.events2bang`** — convert algorithm events (stick/split/birth) into
  banged note triggers with rate limiting.
- **`mgen.map`** — flexible range/scale/curve mapper (one shared mapping object
  reused everywhere, instead of bespoke scaling per object).

---

## Open questions for the build phase

1. **`v8` vs `js`:** target the modern `v8`/`v8ui` (ES2020, faster) as default,
   keep `js` fallback? — likely yes, `v8` first.
2. **GPU vs CPU for fields:** `jit.gen`/`jit.pix` (GPU, fast, but readback cost
   for sonification) vs `jit.matrix` + JS (slower, easy CPU readback). Probably
   offer both where it matters; default GPU for viz, expose a CPU "tap" for sound.
3. **Abstractions vs compiled externals:** start as abstractions + JS (no SDK
   build needed, cross-platform, easy to share via the Package format), revisit
   C externals only if performance demands.
4. **Packaging:** ship as a single Max **Package** (with `package-info.json`,
   `init/`, `help/`, `extras/`) so it installs cleanly via the Package Manager.
