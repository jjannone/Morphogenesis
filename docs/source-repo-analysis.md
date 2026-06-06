# Analysis of `jasonwebb/morphogenesis-resources`

> Source: <https://github.com/jasonwebb/morphogenesis-resources> (CC0).
> This is a curated, reference-style repository — primarily Markdown writeups,
> diagrams, and links to external implementations (Processing, p5.js, openFrameworks,
> shaders, etc.), rather than a single runnable codebase. That makes it an ideal
> *specification source*: each topic is a self-contained algorithm description we
> can re-implement natively in Max/MSP.

## 1. What the repository contains

The repo is organized into five major content areas, plus supporting material.

### 1a. Growth Algorithms
The core of the repo — step-by-step generative growth processes:

| Algorithm | One-line description | Data model |
| --- | --- | --- |
| **Dielectric Breakdown Model (DBM)** | Fractal electrical-discharge / lightning patterns | Grid + probabilistic potential field |
| **Diffusion-Limited Aggregation (DLA)** | Random walkers stick on contact → branching crystals | Particles + cluster |
| **Differential Growth** | A chain/mesh of nodes grows and folds via local forces | Connected node graph (polyline/mesh) |
| **Eden Growth Model** | Random accretion on a cluster boundary | Grid / cell set |
| **Physarum** | Slime-mold agents deposit & follow chemoattractant trails | Agents + trail field (grid) |
| **Primordial Particle System** | Simple particle rules → life-like motion/clusters | Particles + neighbor counts |
| **Reaction-Diffusion** | Gray-Scott chemical patterns (spots, stripes, mazes) | Two-chemical grid (A/B) |
| **Space Colonization** | Branching networks grow toward attractor points | Tree graph + attractor cloud |

These are the highest-value targets: each is visually striking, parameter-rich,
and produces structured spatial/temporal data that maps well to sound.

### 1b. Math & Physics topics (~25)
Foundational models, many of which are directly generative:

- Archimedean / Platonic solids, Geodesic dome, Spherical harmonics
- Cellular automata (CA)
- Cymatics
- Delaunay triangulation & Voronoi diagrams, Lloyd's relaxation, Medial axis
- Fibonacci, Golden angle/ratio, **Phyllotaxis**
- Fourier series, Laplace transform, **Lissajous curves**
- Fractals, Implicit surfaces, Minimal surfaces, Superellipse, **Superformula**
- Inverse/forward kinematics
- Packing problems, Percolation theory
- Saffman–Taylor instability
- **Strange attractors**
- Travelling Salesman Problem (TSP)

### 1c. Natural phenomena
A catalogue (in `Natural-phenomena.md`) of real-world morphogenesis examples
(shells, leaves, corals, slime molds, cracks, etc.) — useful as *aesthetic
references / presets* rather than as algorithms to port.

### 1d. Lab experiments
Physical analogues of the algorithms — strong inspiration for sonification
because they are inherently about energy, waves, and oscillation:

- **Belousov–Zhabotinsky (BZ) reaction** — oscillating chemistry (≈ reaction-diffusion)
- **Chladni plate** — sound-driven nodal patterns (literally audio→geometry)
- **Hele-Shaw cell** — viscous fingering (≈ Saffman–Taylor / DLA)
- **Schlieren imaging** — visualizing density/flow

### 1e. Useful code patterns & techniques (~22)
Reusable building blocks that underlie many of the algorithms above:

- Agent-based modeling, Boids, Particle systems, Flow fields
- Collision detection, Spatial indexing, Vectors
- Constructive solid geometry (CSG), Polygon clipping
- Marching squares/cubes, Metaballs, Signed distance functions (SDFs)
- Lloyd's relaxation, Dithering, Noise
- Fluid simulation, Physics engines, Ray tracing
- Recursion, Shaders, Wave Function Collapse (WFC)

### 1f. Supporting resources
Books/talks, software list, contributing guidelines, and an `images/` folder of
diagrams. Not portable, but valuable for documentation and presets.

## 2. Mapping the material onto Max/MSP

The algorithms cluster into a few computational shapes, and each shape maps to a
preferred Max implementation strategy:

| Computational shape | Examples | Best Max home |
| --- | --- | --- |
| **Grid / field update** | Reaction-diffusion, CA, DBM, Eden, Physarum trail, flow fields | `jit.gen` / `jit.pix` (GPU) or `jit.matrix` + JS (CPU) |
| **Dynamic node graph** | Differential growth, space colonization, TSP, medial axis | `v8`/`js` (needs lists, insertion, neighbors) |
| **Particle systems** | DLA, boids, primordial particles, packing | `v8`/`js` + spatial index, or `jit.gen` on a matrix of particles |
| **Closed-form / parametric** | Superformula, Lissajous, phyllotaxis, attractors, Fourier | `gen~` (audio-rate) and/or small `js` + paint |
| **Computational geometry** | Voronoi/Delaunay, Lloyd, metaballs, SDF, marching squares | `v8`/`js` (geometry) → `jit` (raster) |

### Visualization paths
- **Raster:** matrices rendered with `jit.pwindow` / `jit.world` / `jit.gl.*`.
- **Vector:** node graphs and parametric curves drawn in `jsui`/`v8ui` or sent
  to `jit.gl.path` / `jit.gl.sketch` / `jit.gl.mesh`.

### Sonification paths
The structured output of each algorithm becomes a sound source:
- **Field/matrix → audio:** scan rows/columns as wavetables (`jit.peek~`,
  `index~`, `wave~`); use chemical concentration as amplitude/filter maps.
- **Geometry → control:** node positions/curvature → pitch, pan, density,
  envelopes (drive `poly~` voices, granular clouds, FM banks).
- **Direct audio-rate generators:** strange attractors and Lissajous/superformula
  evaluated in `gen~` are themselves oscillators / chaotic sound sources.
- **Event triggers:** DLA "stick" events, node insertions, CA births/deaths →
  `bang`/note triggers, rhythmic patterns.

## 3. Licensing & attribution note

`morphogenesis-resources` is CC0 (public domain dedication), so re-implementing
its described algorithms is unrestricted. We will still credit the source
explicitly in each object's help patch and in this repo's README, because the
clarity of the writeups is what makes this project tractable.
