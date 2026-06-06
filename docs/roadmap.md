# Roadmap

A phased plan for turning the [object catalogue](maxmsp-object-list.md) into a
working Max **Package**.

## Phase 0 — Analysis & planning *(current)*
- [x] Study `morphogenesis-resources` and categorize its content.
- [x] Draft the Max/MSP object catalogue (tiers, params, viz/sonification).
- [x] Define naming conventions and package layout.
- [ ] Decide `v8` vs `js`, GPU vs CPU defaults (see catalogue "Open questions").

## Phase 1 — Foundations
Build the shared infrastructure first, because every algorithm depends on it.
- [ ] `mgen.map` (range/scale/curve mapper)
- [ ] `mgen.spatialhash` (neighbor queries)
- [ ] `mgen.noise` (perlin/simplex/curl)
- [ ] `mgen.scan~` and `mgen.field2sig~` (the sonification bridge)
- [ ] `mgen.nodes2voices`, `mgen.events2bang`
- [ ] Package skeleton: `package-info.json`, `init/`, `help/`, `extras/`.

## Phase 2 — Two vertical slices (prove viz + sonification end-to-end)
Pick one field-based and one graph-based algorithm and finish them completely
(object + help + example patch + sound):
- [ ] `jit.mgen.rd` (reaction-diffusion) — field/GPU slice.
- [ ] `mgen.diffgrow` (differential growth) — graph/JS slice.

## Phase 3 — Remaining flagship growth algorithms (Tier 1)
- [ ] `mgen.dla`, `jit.mgen.physarum`, `mgen.spacecol`
- [ ] `jit.mgen.dbm`, `jit.mgen.eden`, `mgen.primordial`

## Phase 4 — Parametric/math models (Tier 2)
- [ ] `mgen.attractor~`, `mgen.lissajous~`, `mgen.superformula(~)`
- [ ] `mgen.phyllotaxis`, `mgen.fourier(~)`, parametric-shape helpers

## Phase 5 — Cellular/field & lab experiments (Tier 3)
- [ ] `jit.mgen.ca`, `jit.mgen.bz~`, `jit.mgen.chladni`, `jit.mgen.heleshaw`

## Phase 6 — Geometry/pattern toolkit (Tier 4) & polish
- [ ] Voronoi/Delaunay/Lloyd, metaball/SDF/marching, boids, TSP, etc.
- [ ] Example "performance" patches combining multiple objects.
- [ ] Documentation pass, presets, and Package Manager release.

## Engineering conventions
- **Naming:** `mgen.*` (control/visual), `mgen.*~` (MSP), `jit.mgen.*` (Jitter).
- **Every object** ships with: a `.maxhelp`, a one-line description in the
  package's `interfaces`/`object` metadata, and a credit to the source topic.
- **Inlets/outlets** documented in the help patch; common params share names
  across objects (`seed`, `dt`, `bounds`, `rate`).
- **Sonification is first-class:** every visual object must expose a clean data
  tap (list or matrix) that the `mgen.*~` bridge objects can consume.
- **Cross-platform:** prefer abstractions + `v8`/`gen~` over compiled C
  externals until performance forces otherwise.

## Target environment
- Max 8.2+ (for `v8`/`v8ui`); graceful `js`/`jsui` fallback where feasible.
- Jitter (bundled with Max) for matrix/GL; `gen~`/`jit.gen` for signal/GPU rate.
