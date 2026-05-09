# Hermes Particle Forge — Formula Test Pack

Purpose: candidate math/formula subjects for Benchmark 002+ after Benchmark 001: Yuruyurau Trig Bloom.

Goal: more life-like, artistic, organic, and magical particle systems. These should test whether Hermes Particle Forge can turn pure math into beautiful living motion.

---

## What is next?

Immediate next step:

1. Keep Benchmark 001 as the p5/tweet-code benchmark.
2. Build a Benchmark Gallery page that can host multiple formula subjects.
3. Add 3-5 new formula scenes, each testing a different class of living motion.
4. Pick the strongest one as the Phase 0B taste spike for the real WebGL/Three.js engine.

Recommended Benchmark 002:
- Aurora Jellyfish / Medusa Field

Why:
- Life-like.
- Beautiful.
- Tests tendrils, pulsing, depth, drift, and translucency.
- Very aligned with the “magical and beautiful” product direction.

---

## Benchmark 002: Aurora Jellyfish / Medusa Field

Visual:
- A luminous jellyfish made of particles.
- Bell pulses slowly.
- Tendrils trail below it like living threads.
- Cyan, emerald, violet, and ghost-white palette.

Math ingredients:
- Bell surface: spherical cap / paraboloid.
- Tendrils: sine waves along vertical strands.
- Pulse: slow sinusoidal scale.
- Drift: low-frequency noise / Lissajous offset.
- Depth: phase-shifted z motion.

Core formula sketch:

```js
const u = i / count;
const strandCount = params.strands;
const strand = i % strandCount;
const v = Math.floor(i / strandCount) / (count / strandCount);
const angle = strand / strandCount * Math.PI * 2;
const pulse = 1 + 0.08 * Math.sin(time * params.pulse);

if (u < params.bellRatio) {
  // bell particles
  const r = Math.sqrt(u / params.bellRatio) * params.bellRadius * pulse;
  const theta = angle + 0.2 * Math.sin(time + r);
  out.x = Math.cos(theta) * r;
  out.z = Math.sin(theta) * r;
  out.y = -Math.pow(r / params.bellRadius, 2) * params.bellDepth;
} else {
  // tendril particles
  const t = (u - params.bellRatio) / (1 - params.bellRatio);
  const rootR = params.bellRadius * (0.25 + 0.75 * ((strand % 7) / 7));
  const wave = Math.sin(t * params.waveFreq + time * params.waveSpeed + strand) * params.waveAmp;
  out.x = Math.cos(angle) * rootR + Math.cos(angle + Math.PI/2) * wave;
  out.z = Math.sin(angle) * rootR + Math.sin(angle + Math.PI/2) * wave;
  out.y = t * params.tendrilLength;
}
```

Dream Knobs:
- Bell Pulse
- Tendril Length
- Strand Count
- Biolume
- Drift
- Wave Speed
- Ghost Trails

What it tests:
- segmented formula logic
- organic motion
- translucent/luminous rendering
- depth layering
- scene-specific controls

---

## Benchmark 003: Murmuration / Flocking Ghost Birds

Visual:
- A living cloud of particles that moves like birds or fish.
- Not literal birds; more like a murmuration made of sparks.
- Shape alternates between a cloud, ribbon, and vortex.

Math ingredients:
- Boids-inspired fields but simplified.
- Curl noise / vector field following.
- Attractor/repulsor points.
- Phase offsets per particle.

Core formula ideas:
- Use deterministic hash per particle for base orbit.
- Use flow-field vector angle from fbm/noise.
- Use sinusoidal attractor path.

Pseudo formula:

```js
const u = i / count;
const h = helpers.hash(i + seed);
const h2 = helpers.hash(i * 17 + seed);
const attractX = Math.sin(time * 0.37) * 20;
const attractY = Math.cos(time * 0.23) * 10;
const attractZ = Math.sin(time * 0.19) * 18;
const field = Math.sin(out.x * 0.04 + time) + Math.cos(out.z * 0.05 - time * 0.7);

out.x = Math.cos(u * Math.PI * 80 + field) * (10 + h * 30) + attractX;
out.y = Math.sin(u * Math.PI * 43 + time + h) * 12 + attractY;
out.z = Math.sin(u * Math.PI * 80 + field) * (10 + h2 * 30) + attractZ;
```

Dream Knobs:
- Flock Cohesion
- Wander
- Vortex Pull
- Wingbeat Pulse
- Cloud Spread

What it tests:
- lifelike collective behavior
- flow fields
- emergent-looking motion without a full simulation

---

## Benchmark 004: Coral Growth / Reef Bloom

Visual:
- A branching coral organism slowly growing and breathing.
- Looks like reef, fungus, neurons, or frost.
- Great still image and slow animation.

Math ingredients:
- Phyllotaxis / golden angle.
- Branch curves.
- Reaction-diffusion-inspired color bands.
- Domain warping.

Core formula ideas:
- Map particle index to branch id and growth position.
- Use golden angle for branch directions.
- Use sin/noise to curl branch tips.

Pseudo formula:

```js
const branchCount = params.branches;
const branch = i % branchCount;
const t = Math.floor(i / branchCount) / (count / branchCount);
const golden = Math.PI * (3 - Math.sqrt(5));
const angle = branch * golden;
const curl = Math.sin(t * params.curlFreq + time * 0.4 + branch) * params.curl;
const r = Math.pow(t, 0.72) * params.radius;

out.x = Math.cos(angle + curl) * r;
out.z = Math.sin(angle + curl) * r;
out.y = (t - 0.5) * params.height + Math.sin(t * 14 + branch) * params.ripple;
```

Dream Knobs:
- Branch Count
- Growth
- Curl
- Reef Height
- Tip Glow
- Color Banding

What it tests:
- botanical/organic structure
- growth parameters
- still-frame beauty

---

## Benchmark 005: Heartbeat / Breathing Organism

Visual:
- Abstract living organ or heart-like field, not anatomical gore.
- Pulses with systole/diastole rhythm.
- Particles contract and expand with secondary ripples.

Math ingredients:
- Parametric heart curve.
- Pulse envelope using sin or eased periodic function.
- Shockwave rings.
- Color temperature changes with pulse.

2D heart curve:

```js
x = 16 * Math.pow(Math.sin(a), 3);
y = 13 * Math.cos(a) - 5 * Math.cos(2*a) - 2 * Math.cos(3*a) - Math.cos(4*a);
```

3D expansion:
- Use multiple shells with z from sin phase.
- Pulse scale from eased sine.

Dream Knobs:
- Pulse Rate
- Shockwave
- Organ Glow
- Shell Depth
- Tenderness / Chaos

What it tests:
- recognizable parametric form
- emotion/mood
- non-uniform rhythmic motion

---

## Benchmark 006: Mycelium / Neural Root Network

Visual:
- Fine glowing root/fungal network spreading through darkness.
- Looks like mycelium, lightning, neurons, or city lights.

Math ingredients:
- Branching walks.
- L-system-like recursive branching.
- Voronoi-ish cellular distance fields.
- Attractor points.

Core approach:
- Precompute base particles as paths/branches in init.
- Animate light pulses along branch parameter t.

Dream Knobs:
- Branching
- Pulse Speed
- Root Density
- Glow Decay
- Growth Direction

What it tests:
- init-time precomputation
- path-based particle systems
- line/particle hybrid rendering

---

## Benchmark 007: Firefly Field / Bioluminescent Swarm

Visual:
- Floating fireflies in deep forest darkness.
- Individual particles blink with asynchronous rhythms.
- Motion is calm, life-like, and atmospheric.

Math ingredients:
- Deterministic per-particle blink phase.
- Smooth random drift.
- Depth parallax.
- Low alpha trails.

Pseudo formula:

```js
const h = helpers.hash(i + seed);
const phase = h * Math.PI * 2;
const blink = Math.pow(0.5 + 0.5 * Math.sin(time * (0.8 + h) + phase), 8);
out.x = (helpers.hash(i*3) - 0.5) * params.spread + Math.sin(time * 0.2 + phase) * params.drift;
out.y = (helpers.hash(i*5) - 0.5) * params.height + Math.cos(time * 0.17 + phase) * params.drift;
out.z = (helpers.hash(i*7) - 0.5) * params.depth;
out.size = 0.5 + blink * params.sparkSize;
out.a = 0.1 + blink;
```

Dream Knobs:
- Blink Sync
- Drift
- Density
- Spark Size
- Forest Depth

What it tests:
- calm lifelike ambient scene
- alpha/size modulation
- beauty with simple math

---

## Benchmark 008: Fluid Smoke / Curl Field Spirit

Visual:
- Smoke creature or spirit rising and folding into itself.
- A more life-like version of Benchmark 001.

Math ingredients:
- Curl-noise-like vector field.
- Domain warping.
- Vertical lift.
- Particle trails.

Dream Knobs:
- Lift
- Curl
- Dissipation
- Spirit Shape
- Ember Ratio

What it tests:
- fluid illusion without full fluid simulation
- trails
- field-following motion

---

## Benchmark 009: Lissajous Orchid / Harmonic Flower

Visual:
- A flower-like harmonic sculpture made from orbital curves.
- Petals breathe and twist.

Math ingredients:
- Lissajous curves.
- Rose curves.
- Torus projection.

Useful formulas:

```js
x = Math.sin(a * nx + phase);
y = Math.sin(a * ny);
z = Math.sin(a * nz + time);

// Rose curve radius
r = Math.cos(k * a);
```

Dream Knobs:
- Petals
- Harmonic Ratio
- Twist
- Bloom
- Phase Drift

What it tests:
- mathematical elegance
- periodic loops
- beautiful stills

---

## Benchmark 010: Strange Attractor Garden

Visual:
- Chaotic attractor sculpture that looks like butterfly wings, smoke, or alien plant life.

Math ingredients:
- Lorenz attractor
- Clifford attractor
- De Jong attractor
- Aizawa attractor

Clifford attractor:

```js
x1 = Math.sin(a * y) + c * Math.cos(a * x);
y1 = Math.sin(b * x) + d * Math.cos(b * y);
```

De Jong attractor:

```js
x1 = Math.sin(a * y) - Math.cos(b * x);
y1 = Math.sin(c * x) - Math.cos(d * y);
```

Dream Knobs:
- Chaos A/B/C/D
- Orbit Count
- Trace Length
- Color Drift
- Symmetry

What it tests:
- iterative formulas
- init-time path generation
- chaotic but stable beauty

---

## Recommended next build order

1. Benchmark 002: Aurora Jellyfish
2. Benchmark 007: Firefly Field
3. Benchmark 004: Coral Growth
4. Benchmark 010: Strange Attractor Garden
5. Benchmark 008: Fluid Smoke

Why this order:
- Jellyfish is the strongest magical/life-like visual.
- Firefly Field proves calm ambient beauty.
- Coral Growth proves organic structure.
- Strange Attractor proves deep math beauty.
- Fluid Smoke pushes toward true particle-fluid aesthetics.

---

## Numbers/constants worth testing

Golden angle:
```js
Math.PI * (3 - Math.sqrt(5)) // ~2.399963
```
Use for natural distributions: sunflower seeds, sphere points, coral branches.

Tau:
```js
Math.PI * 2
```
Use for clean periodic motion.

Irrational ratios:
```js
Math.sqrt(2)
Math.sqrt(3)
Math.sqrt(5)
(1 + Math.sqrt(5)) / 2 // golden ratio
```
Use for non-repeating phase offsets and organic quasi-periodic movement.

Easing pulse:
```js
const pulse = Math.pow(0.5 + 0.5 * Math.sin(time), 2.5);
```
Use for heartbeat, jellyfish bell, breathing organisms.

Smoothstep:
```js
function smoothstep(a,b,x){ x = clamp((x-a)/(b-a),0,1); return x*x*(3-2*x); }
```
Use for life-like transitions.

Deterministic hash:
```js
function hash(n){ const x = Math.sin(n * 12.9898) * 43758.5453123; return x - Math.floor(x); }
```
Use instead of Math.random inside formulas.

---

## The artistic test standard

A candidate formula is good if:

1. It looks good paused.
2. It looks alive in motion.
3. It has 3-7 meaningful Dream Knobs.
4. It can be explained by the Formula Oracle.
5. It can produce at least four strong variants.
6. It does not require assets.
7. It can eventually port to the Forge init/update formula API.

