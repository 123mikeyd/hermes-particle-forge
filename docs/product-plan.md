# Hermes Particle Forge Master Plan

> For Hermes: this is the master product/build plan. Use it as the north star before writing code. The goal is not merely to clone existing AI particle demos. The goal is to build a magical, beautiful, local-first creative instrument powered by Hermes.

**Product Name:** Hermes Particle Forge

**One-line vision:** A local AI particle-art instrument where Mike can describe impossible visual phenomena and watch Hermes turn them into smooth, beautiful, exportable particle worlds.

**Core promise:** Prompt → variants → live particle worlds → refine with Hermes → export art/code/video/game assets.

**Project path:** `/mnt/c/Users/Mike/Desktop/hermes-particle-forge/`

**Launcher:** `/mnt/c/Users/Mike/Desktop/start_particle_forge.bat`

**Master plan path:** `/mnt/c/Users/Mike/Desktop/hermes-particle-forge-master-plan.md`

---

## 0. Product Truth

Hermes already has access to cool particle/generative-art workflows. That means this project cannot win by being “an AI particle simulator.” That category is already becoming commodity.

It wins by being:

1. Beautiful on first launch.
2. Magical in interaction.
3. Local-first and private.
4. Hermes-native rather than browser-key-native.
5. Performance-aware rather than demo-fragile.
6. Export-focused for real creative use.
7. A creative instrument, not just a code generator.

If it does not feel like opening a little black-magic art machine on the Desktop, we missed.

---

## 1. Product Pillars

### Pillar 1: Beauty Before Features

Every default scene must be frame-worthy.

No tutorial-looking dots.
No default black background with random RGB specks.
No generic UI chrome swallowing the artwork.
No “AI slop dashboard.”

First-launch scene should feel like:
- a living constellation
- a sacred instrument panel
- a glass/neon artifact
- something you could screen-record immediately

Quality bar:
- If a screenshot of the default scene is not worth posting, do not add more features yet.

### Pillar 2: Magic Loop

The central loop must feel like conjuring:

1. Type a poetic/visual prompt.
2. Hermes generates multiple interpretations.
3. The app previews them as living thumbnails or quick-switch variants.
4. User picks one.
5. User says “make it more alien / calmer / faster / like smoke / like a jellyfish.”
6. Hermes refines the formula.
7. The app validates, runs, and saves the evolution.

This is the magic:
- Not “generate code.”
- “Summon visual phenomena.”

### Pillar 3: Local First

Runs on Mike’s machine.
No account.
No cloud dependency.
No browser API key box.
No invisible checkout.
No forced publishing.

Everything saves locally:
- projects
- variants
- prompt history
- exports
- standalone HTML
- generated code

### Pillar 4: Performance Is Part of the Art

The closest open-source clone dropped to 2K particles and ~4 FPS in testing. We should treat that as a warning.

Hermes Particle Forge must be honest about performance:
- target 20K smooth particles for V1
- visible FPS/particle count
- no silent quality downgrade
- linter detects slow generated formulas
- deterministic helpers avoid per-frame random flicker
- architecture leaves room for GLSL/GPGPU modes

### Pillar 5: Export Is Not an Afterthought

The app should produce things Mike can use:
- PNG stills
- transparent PNG where possible
- short WebM/GIF loops
- standalone HTML art pieces
- React/Three.js component export later
- sprite sheet export later for games
- JSON project files for remixing

---

## 2. Target Feeling

The emotional target is not “technical demo.”

It should feel like:
- a particle synthesizer
- a visual spellbook
- a sci-fi observatory
- a tiny demoscene lab
- Hermes sitting beside the canvas as a creative partner

Aesthetic vocabulary:
- black, near-black, deep blue, spectral green, electric cyan, ultraviolet, ember gold
- luminous particles, not flat points
- subtle bloom and depth fog
- restrained UI, with the artwork dominant
- Courier New for technical/code panels, matching Mike’s font preference
- precise, instrument-like controls
- tiny details: scanlines, spectral ticks, coordinate readouts, soft pulse indicators

Avoid:
- generic SaaS sidebars
- emoji-heavy UI
- cluttered dashboards
- noisy rainbow presets by default
- glassmorphism everywhere
- fake stats

---

## 3. The Core Creative Workflow

### 3.1 The Three Modes

Hermes Particle Forge should have three creative modes over time.

#### Mode A: Swarm Forge

3D particle worlds.

Use for:
- galaxies
- smoke
- jellyfish
- neural clouds
- waveform sculptures
- black holes
- magical artifacts
- text as particles

This is V1’s main mode.

#### Mode B: Tweet Processing Lab

2D p5 / tiny-code mode inspired by #つぶやきProcessing.

Translation note: つぶやきProcessing = “Tweet Processing,” a creative-coding practice where the whole Processing/p5 sketch fits in a tweet.

Use for:
- tiny math sketches
- p5 snippets from X
- expanding compressed code into readable code
- porting 2D sketches into 3D swarms

This can be V1.5.

#### Mode C: Shader Shrine

GLSL/shader mode.

Use for:
- ultra-fast particles
- background fields
- raymarched or shader-style effects
- 100K+ particle ambitions

This is V2.

---

## 4. V1 Scope: The Magical Minimum

V1 must be small enough to build, but beautiful enough to matter.

### V1 must include

1. Desktop launcher
   - `start_particle_forge.bat`
   - opens local app at `http://127.0.0.1:7867`

2. Local web app
   - Flask backend
   - static frontend
   - vanilla Three.js or lightweight Vite vanilla modules
   - avoid React/R3F for V1 performance simplicity

3. Fullscreen particle canvas
   - default black/neon scene
   - OrbitControls
   - 20K particles target
   - FPS + particle count visible but tasteful

4. Beautiful default presets
   - Geodesic Ghost Sphere
   - Aurora Jellyfish
   - Spiral Galaxy
   - Smoke Cathedral
   - Neural Fireflies
   - Waveform Halo
   - Black Hole Thread
   - Crystal Rain

5. Formula engine
   - safe JS formula mode
   - deterministic helpers
   - two-stage init/update architecture

6. Particle linter
   - blocks dangerous APIs
   - warns about slow code
   - detects `Math.random()` in hot loop
   - detects `new` allocations in hot loop
   - catches NaN/Infinity risk

7. Prompt helper
   - structured prompt copied/sent to Hermes
   - Hermes generates safe formula code
   - app imports/applies it

8. Variant gallery
   - at least 4 variants per prompt manually/Hermes-assisted in V1
   - names + thumbnails can be simple at first

9. Save/load local projects
   - JSON files under `projects/`

10. Export
   - PNG snapshot
   - JSON project
   - standalone HTML export

### V1 must not include

- cloud community publishing
- account/auth system
- Firebase
- 3D model import
- full node editor
- gesture controls
- audio reactivity
- GPGPU/FBO
- multiplayer/collab

Those can come later. V1 must feel excellent before it feels large.

---

## 5. Architecture

### 5.1 Recommended folder structure

```text
/mnt/c/Users/Mike/Desktop/hermes-particle-forge/
  app.py
  requirements.txt
  README.md
  start_particle_forge.bat
  projects/
    .gitkeep
  exports/
    .gitkeep
  static/
    index.html
    css/
      app.css
      themes.css
    js/
      main.js
      engine/
        renderer.js
        particles.js
        formulas.js
        linter.js
        helpers.js
        presets.js
        export.js
      ui/
        controls.js
        gallery.js
        prompt.js
        inspector.js
      vendor/
        three.module.js optional if vendored
  templates/
    standalone_export.html
  tests/
    test_linter.py
    test_projects_api.py
```

### 5.2 Backend

Use Flask only for:
- serving the app
- saving/loading project JSON
- writing exports
- optionally invoking Hermes CLI later

Keep backend boring.

API endpoints:

```text
GET  /api/health
GET  /api/projects
GET  /api/projects/<slug>
POST /api/projects
POST /api/export/html
POST /api/export/png-metadata optional
```

### 5.3 Frontend

Use vanilla JS modules:
- less framework overhead
- simpler debugging
- easier standalone export
- less chance of React rerender noise hurting FPS

Core render path:
- Three.js Scene
- PerspectiveCamera
- WebGLRenderer
- BufferGeometry
- `position`, `color`, `size`, `alpha` attributes
- ShaderMaterial for soft circular glowing particles
- optional postprocessing later

### 5.4 Formula API

Use two stages.

```js
// Runs once per particle when preset/formula loads.
function initParticle(i, count, seed, base, helpers) {
  base.u = i / count;
  base.h = helpers.hash(i + seed);
  base.h2 = helpers.hash(i * 17 + seed);
}

// Runs every frame.
function updateParticle(i, count, time, base, out, params, helpers) {
  out.x = 0;
  out.y = 0;
  out.z = 0;
  out.r = 0.2;
  out.g = 0.9;
  out.b = 1.0;
  out.a = 1.0;
  out.size = 1.5;
}
```

Why this is better than most demos:
- stable randomness
- precomputation
- less flicker
- easier optimization
- clearer mental model

### 5.5 Helper API

Built-in deterministic helpers:

```js
hash(n)
hash2(a, b)
noise1(n)
noise2(a, b)
fbm2(x, y, octaves)
palette(name, t)
easeInOut(t)
smoothstep(edge0, edge1, x)
rotate2(x, y, angle)
clamp(x, min, max)
safeFinite(x, fallback)
```

Palette helpers are important. Beautiful color should not depend on every generated formula inventing colors from scratch.

### 5.6 Linter rules

Block:
- `document`
- `window`
- `fetch`
- `XMLHttpRequest`
- `WebSocket`
- `localStorage`
- `sessionStorage`
- `indexedDB`
- `eval`
- `Function`
- `import`
- `require`
- `process`
- `crypto`
- `setTimeout`
- `setInterval`
- `navigator`
- `location`
- `__proto__`
- `.prototype`

Warn:
- `new` inside update
- `Math.random()` inside update
- loops inside update
- too many trig calls
- branch-heavy code
- assignment to unknown globals

Runtime guard:
- if x/y/z/r/g/b/a/size is not finite, replace with safe fallback and count error
- display “formula instability” if many particles fail

---

## 6. The Magic Features

These are what make it more than a simulator.

### 6.1 The Conjure Box

A prompt input that feels like a spell line, not a chat box.

Placeholder examples:
- “a ghost jellyfish made from emerald sparks, breathing slowly”
- “a spiral galaxy that collapses into a silver ring”
- “rain falling upward into a glass cathedral”
- “a waveform halo around an invisible singer”
- “blue fireflies learning to become a word”

Buttons:
- Conjure 4 variants
- Make it calmer
- Make it stranger
- Make it faster
- Make it cinematic
- Explain the math
- Optimize formula

### 6.2 Variant Constellation

Instead of one result, show variants as small cards:

- Elegant
- Chaotic
- Cinematic
- Minimal

Each card stores:
- prompt
- formula
- params
- linter score
- FPS estimate
- thumbnail

### 6.3 The Formula Oracle

A side panel that explains what the current particle system is doing in plain English.

Example:

```text
This scene distributes particles along a golden-angle sphere.
Each particle breathes outward according to a slow sine wave.
Color shifts by latitude, creating the green rim and blue core.
No per-frame random calls detected.
Performance: green.
```

This gives the user trust and makes the art teachable.

### 6.4 Dream Knobs

Controls should be poetic but precise.

New naming convention from Benchmark 001:
- The editable math family is called the **Bloom Kernel**.
- The dropdown/group of preloaded equations is called **Formula Family**.
- A specific saved equation + parameters is called a **Bloom Preset**.

Benchmark 001 now includes preloaded Formula Families:
- Trig Bloom / Yuruyurau
- Aurora Jellyfish
- Rose / Rhodonea Bloom
- Lissajous Orchid
- Golden Phyllotaxis
- De Jong Attractor
- Biolume Fireflies

Instead of only:
- speed
- size
- scale

Use scene-aware labels:
- Breath
- Orbit Drift
- Tendril Length
- Core Gravity
- Spark Decay
- Halo Width
- Turbulence
- Bloom Weight
- Ghost Trails
- Collapse

Internally they are numbers. Externally they feel like an instrument.

### 6.5 Remix History

Every major generation/refinement becomes a node in local history:

```text
Original prompt
  -> calmer variant
    -> more jellyfish
      -> optimized
  -> chaotic variant
```

V1 can simply save this as an array in project JSON.

### 6.6 Beauty Gate

Before a preset ships, it must pass:
- looks good paused
- looks good in motion
- readable at 1080p screenshot
- not generic
- stable color palette
- no console errors
- acceptable FPS

---

## 7. Visual Design System

### 7.1 Name and tone

Name: Hermes Particle Forge

Tone:
- technical but mystical
- instrument, not toy
- precise but poetic

Possible UI copy:
- Conjure
- Refine
- Stabilize
- Export Relic
- Formula Oracle
- Variant Constellation
- Dream Knobs
- Forge Memory

Avoid too much fantasy. Keep it grounded enough to feel like a real tool.

### 7.2 Typography

Mike preference:
1. Courier New
2. Mondwest
3. Helvetica

Use:
- Courier New for UI labels, code, readouts
- Helvetica/system sans for longer readable descriptions if needed
- optional Mondwest later for logo/title if available

### 7.3 Color tokens

```css
--bg-void: #03050a;
--bg-panel: rgba(7, 12, 20, 0.78);
--bg-panel-solid: #080d14;
--ink-primary: #e8f7ff;
--ink-muted: #7f94a8;
--line-soft: rgba(120, 220, 255, 0.16);
--line-hot: rgba(85, 255, 210, 0.72);
--accent-cyan: #60f7ff;
--accent-green: #69ffb7;
--accent-violet: #9b7cff;
--accent-gold: #ffd36e;
--danger: #ff5d73;
--success: #69ffb7;
```

### 7.4 UI layout

Default screen:

```text
┌─────────────────────────────────────────────────────────────┐
│ tiny top toolbar: FPS, scene name, export, settings          │
│                                                             │
│                                                             │
│                 FULLSCREEN PARTICLE CANVAS                  │
│                                                             │
│                                                             │
│ bottom center: preset / variant constellation                │
│ left collapsed: Conjure Box                                 │
│ right collapsed: Formula Oracle / Dream Knobs                │
└─────────────────────────────────────────────────────────────┘
```

Panels are overlays, not permanent dashboards.
Artwork owns the screen.

### 7.5 Motion design

- UI fades and slides softly, never bounces cartoonishly.
- Hover states glow faintly.
- Buttons have tiny “instrument light” response.
- Variant switch crossfades/morphs particle positions where possible.
- Respect reduced-motion preference.

---

## 8. Preset Art Direction

V1 presets should be hand-authored, not AI-generated slop.

### 8.1 Geodesic Ghost Sphere

Mood: sacred green hologram.

Behavior:
- golden-angle sphere distribution
- subtle breathing radius
- rim brighter than core
- slow rotation
- small spectral shimmer

Controls:
- Radius
- Breath
- Rim Glow
- Spectral Drift

### 8.2 Aurora Jellyfish

Mood: underwater ghost, luminous tendrils.

Behavior:
- bell-shaped top
- dangling tendrils made of particles
- slow pulse
- cyan/violet/green palette

Controls:
- Bell Pulse
- Tendril Length
- Drift
- Biolume

### 8.3 Spiral Galaxy

Mood: deep-space cinematic.

Behavior:
- multi-arm spiral
- dense core
- dust halo
- slow orbital drift

Controls:
- Arms
- Core Gravity
- Dust Spread
- Orbit Speed

### 8.4 Smoke Cathedral

Mood: vertical sacred smoke, blue-gold embers.

Behavior:
- rising columns
- turbulent curl
- arch-like negative space
- ember flecks

Controls:
- Turbulence
- Lift
- Arch Width
- Ember Ratio

### 8.5 Neural Fireflies

Mood: living thought network.

Behavior:
- particles cluster in nodes
- faint connecting lines optional later
- pulses travel through clusters

Controls:
- Cluster Count
- Signal Speed
- Connection Glow
- Wander

### 8.6 Waveform Halo

Mood: audio ghost ring.

Behavior:
- circular waveform ribbon
- particles oscillate from a hidden signal
- great for Nous/Hermes visual identity later

Controls:
- Frequency
- Amplitude
- Halo Width
- Phase Drift

### 8.7 Black Hole Thread

Mood: gravity well with luminous accretion.

Behavior:
- particles orbit inward
- warped disk
- occasional ejection jets

Controls:
- Gravity
- Disk Tilt
- Jet Strength
- Accretion Glow

### 8.8 Crystal Rain

Mood: luminous rain falling through invisible geometry.

Behavior:
- vertical particle streaks
- depth parallax
- occasional crystalline bursts

Controls:
- Fall Speed
- Depth Spread
- Burst Rate
- Refraction Tint

---

## 9. Hermes Integration Plan

### 9.1 V1: Prompt bridge, not core tool

Do not modify Hermes core first.

Use Hermes from the normal agent workflow:
- user describes desired effect in app or chat
- Hermes generates formula code following the app API
- app validates and runs it
- project JSON saves prompt/code/params/history

### 9.2 V1.5: Local skill

Create local skill:

```text
~/.hermes/skills/creative/hermes-particle-forge/SKILL.md
```

The skill should teach Hermes:
- formula API
- linter constraints
- style recipes
- performance rules
- how to generate variants
- how to optimize formulas
- how to port p5 snippets to 3D swarm mode

### 9.3 V2: Optional Hermes tool/plugin

Only after API stabilizes.

Possible tool functions:

```python
particle_forge_validate(code) -> JSON
particle_forge_generate(prompt, mode, variant_count) -> JSON
particle_forge_save_project(project) -> path
particle_forge_export(project_path, format) -> path
```

This is not needed for V1.

---

## 10. Project JSON Schema

```json
{
  "schema_version": 1,
  "name": "aurora_jellyfish",
  "title": "Aurora Jellyfish",
  "created_at": "2026-05-08T00:00:00Z",
  "updated_at": "2026-05-08T00:00:00Z",
  "mode": "swarm",
  "seed": 12345,
  "prompt": "a ghost jellyfish made from emerald sparks, breathing slowly",
  "description": "A luminous jellyfish particle sculpture with tendrils and slow breathing motion.",
  "engine": {
    "particle_count": 20000,
    "renderer": "three-points-shader",
    "formula_mode": "js-init-update"
  },
  "params": {
    "scale": 1.0,
    "speed": 1.0,
    "bloom": 0.8
  },
  "formula": {
    "init": "...",
    "update": "..."
  },
  "linter": {
    "safety": "pass",
    "performance": "green",
    "determinism": "stable",
    "warnings": []
  },
  "history": [
    {
      "prompt": "make it more alien",
      "changed_at": "...",
      "notes": "increased tendril turbulence and violet highlights"
    }
  ]
}
```

---

## 11. Build Phases

### Phase 0: Taste Spike

Goal:
- Prove we can make one scene beautiful before building the whole app.

Deliverable:
- one standalone HTML or app page with the Geodesic Ghost Sphere
- 20K particles
- soft circular shader points
- smooth motion
- screenshot export

Success criteria:
- screenshot looks worth sharing
- 20K target performance is acceptable
- no console errors

Do not proceed until this feels good.

### Phase 1: Minimal Forge

Goal:
- Turn the taste spike into a usable local app.

Deliverable:
- Flask app
- Desktop launcher
- renderer module
- preset system
- controls panel
- save/load project JSON
- PNG export

Success criteria:
- launch via `.bat`
- switch between 3 presets
- save/load works
- export works

### Phase 2: Formula Engine + Linter

Goal:
- Safely run generated/custom formulas.

Deliverable:
- init/update formula API
- deterministic helpers
- linter
- dry-run validator
- runtime finite guard
- code editor panel

Success criteria:
- valid formula runs
- forbidden code blocked
- slow/flickery patterns warned
- no app crash from bad formula

### Phase 3: Hermes Conjure Loop

Goal:
- Make Hermes feel like the creative partner.

Deliverable:
- structured prompt template
- “Copy prompt for Hermes” or local invoke path
- import generated formula
- generate 4 named variants
- Formula Oracle explanation panel

Success criteria:
- prompt -> usable formula in under a few minutes
- variants feel meaningfully different
- Hermes can optimize a formula after linter warning

### Phase 4: Export Studio

Goal:
- Make outputs useful.

Deliverable:
- standalone HTML export
- high-res PNG options
- WebM/GIF loop if feasible
- project bundle export

Success criteria:
- exported HTML opens independently
- PNG is clean
- loop export is watchable

### Phase 5: Local Skill

Goal:
- Make the workflow reusable across sessions.

Deliverable:
- `hermes-particle-forge` skill
- formula examples
- performance rules
- style templates
- p5-to-3D port workflow

Success criteria:
- fresh Hermes session can generate good formulas using the skill

### Phase 6: V1 Polish

Goal:
- Make it feel magical.

Deliverable:
- refined visual design
- better presets
- variant thumbnails
- polish animations
- README
- demo exports

Success criteria:
- Mike can launch, play, generate, refine, export without hand-holding
- default state is beautiful
- no embarrassing UI slop

---

## 12. Detailed Task Plan

### Task 1: Create project scaffold

Files:
- Create: `/mnt/c/Users/Mike/Desktop/hermes-particle-forge/app.py`
- Create: `/mnt/c/Users/Mike/Desktop/hermes-particle-forge/requirements.txt`
- Create: `/mnt/c/Users/Mike/Desktop/hermes-particle-forge/static/index.html`
- Create: `/mnt/c/Users/Mike/Desktop/hermes-particle-forge/static/css/app.css`
- Create: `/mnt/c/Users/Mike/Desktop/hermes-particle-forge/static/js/main.js`
- Create: `/mnt/c/Users/Mike/Desktop/hermes-particle-forge/projects/.gitkeep`
- Create: `/mnt/c/Users/Mike/Desktop/hermes-particle-forge/exports/.gitkeep`
- Create: `/mnt/c/Users/Mike/Desktop/start_particle_forge.bat`

Verification:
- `python app.py`
- page opens at `http://127.0.0.1:7867`
- no console errors

### Task 2: Build renderer taste spike

Files:
- Create: `static/js/engine/renderer.js`
- Create: `static/js/engine/particles.js`
- Modify: `static/js/main.js`
- Modify: `static/css/app.css`

Implementation:
- Three.js scene/camera/renderer
- BufferGeometry point cloud
- ShaderMaterial circular particles
- 20K particle target
- Geodesic Ghost Sphere hardcoded first

Verification:
- canvas renders
- particles glow softly
- OrbitControls work
- FPS acceptable

### Task 3: Add deterministic helpers

Files:
- Create: `static/js/engine/helpers.js`

Implementation:
- hash/noise/palette/easing/rotate helpers

Verification:
- same seed produces same scene
- no Math.random required in preset update loops

### Task 4: Add preset system

Files:
- Create: `static/js/engine/presets.js`
- Create: `static/js/ui/controls.js`

Implementation:
- preset registry
- load preset by id
- expose params
- add first 3 presets: sphere, jellyfish, galaxy

Verification:
- switch presets without reload
- controls update scene

### Task 5: Add particle linter

Files:
- Create: `static/js/engine/linter.js`
- Create: `tests/test_linter.py` or browser-side linter test page

Implementation:
- forbidden pattern checks
- warning checks
- dry-run compile
- result object with safety/performance/determinism

Verification:
- `fetch` blocked
- `Math.random()` warned
- `new THREE.Vector3()` warned/blocked in update

### Task 6: Add formula editor

Files:
- Create: `static/js/ui/inspector.js`
- Modify: `static/index.html`
- Modify: `static/css/app.css`

Implementation:
- formula init/update text areas
- apply button
- linter result display
- Formula Oracle display placeholder

Verification:
- paste valid formula and run
- paste bad formula and block safely

### Task 7: Add Flask project API

Files:
- Modify: `app.py`
- Create: `tests/test_projects_api.py`

Implementation:
- list/save/load projects
- slug-safe filenames
- JSON validation

Verification:
- save project
- reload browser
- load project

### Task 8: Add export

Files:
- Create: `static/js/engine/export.js`
- Create: `templates/standalone_export.html`
- Modify: `app.py`

Implementation:
- PNG snapshot client-side
- standalone HTML export server-side
- project JSON download

Verification:
- PNG opens
- standalone HTML opens

### Task 9: Add Hermes prompt bridge

Files:
- Create: `static/js/ui/prompt.js`
- Create: `docs/hermes_prompt_template.md`

Implementation:
- Conjure Box
- structured prompt generator
- copy prompt
- import formula output

Verification:
- Hermes-generated formula passes linter and runs

### Task 10: Polish first-run experience

Files:
- Modify: `static/css/app.css`
- Modify: UI modules
- Create: `README.md`

Implementation:
- beautiful default state
- collapsed panels
- tasteful readouts
- keyboard shortcuts

Verification:
- first launch looks good
- user can operate without reading code

---

## 13. Acceptance Criteria

V1 is done only when all are true:

- Desktop `.bat` launches app.
- Default scene is beautiful.
- 20K particles run acceptably on Mike’s machine.
- At least 5 polished presets exist.
- Custom formula editor works.
- Linter blocks dangerous code.
- Linter warns about performance traps.
- Save/load projects works.
- PNG export works.
- Standalone HTML export works.
- Hermes prompt bridge can generate a usable new scene.
- No console errors during normal use.
- No browser API key entry needed.

---

## 14. What Makes It Better Than Existing Tools

Compared to Sanjays2402/ai-particle-simulator:
- local-first
- no browser API key
- performance-aware from the start
- deterministic formula API
- two-stage init/update model
- beautiful hand-authored presets before feature pileup
- linter and Formula Oracle

Compared to Casberry:
- private/local
- Hermes-native
- more transparent code workflow
- no cloud dependency
- stronger export/project ownership

Compared to Babylon/PlayCanvas/Nebula:
- prompt-native
- art-instrument-first
- simpler local creative workflow
- p5 tweet-code bridge

---

## 15. Risks

### Risk 1: It becomes a clone

Mitigation:
- avoid copying UI/code/branding
- focus on local-first Hermes workflow
- unique visual language
- Formula Oracle / linter / variants

### Risk 2: Performance collapses

Mitigation:
- vanilla Three.js
- no React/R3F in V1
- deterministic helpers
- linter
- measure FPS early
- do taste spike before full app

### Risk 3: Generated code is unsafe

Mitigation:
- strict linter
- dry-run validator
- sandbox later
- no file/network access from generated code

### Risk 4: It looks generic

Mitigation:
- hand-author presets
- beauty gate
- strong art direction
- no feature expansion until default scene is excellent

### Risk 5: Scope creep

Mitigation:
- V1 excludes cloud, gestures, audio, GPGPU, node editor
- phases must pass acceptance before moving on

---

## 16. Future Roadmap

### V1.5

- p5 Tweet Processing Lab
- p5 snippet explainer
- p5-to-3D swarm port
- WebM/GIF loop export
- variant thumbnails
- better prompt import/export

### V2

- GLSL vertex shader generation
- shader compiler feedback
- audio reactivity
- text-to-particles
- image-to-particles
- sprite sheet export

### V3

- GPGPU/FBO simulation
- 100K-1M particles
- node graph mode
- hand gesture controls
- community/preset sharing if explicitly wanted

---

## 17. Immediate Next Move

Do Phase 0 first.

### Phase 0A: Benchmark 001 — Yuruyurau Trig Bloom

Created benchmark harness:
- `/mnt/c/Users/Mike/Desktop/hermes-particle-forge-benchmark-001.html`

This is the canonical p5/tweet-code test subject. It proves the pipeline:
1. run the original tiny formula
2. expand it into readable controlled code
3. port it into a Particle Forge-style 3D formula
4. expose Dream Knobs
5. export PNG
6. inspect linter/performance notes

Use this benchmark to test future Forge features:
- p5 snippet import
- p5-to-Forge conversion
- formula explanation
- parameter extraction
- deterministic helper replacement
- linter warnings
- variant generation
- visual quality comparison between original 2D and Forge 3D port

### Phase 0B: Geodesic Ghost Sphere taste spike

Build one standalone Geodesic Ghost Sphere taste spike with:
- Three.js
- 20K particles
- black/neon aesthetic
- soft glowing shader points
- deterministic golden-angle sphere
- breathing motion
- PNG screenshot button
- FPS readout

Do not build the full app until at least one Phase 0 scene feels magical.

If the benchmark and taste spike are beautiful and fast enough, then scaffold the app around them.

---

## 18. North Star Quote

Hermes Particle Forge should feel like this:

“Mike types a sentence. Hermes turns it into a living constellation. The app tells him why it works, lets him bend it like an instrument, and exports something beautiful enough to keep.”
