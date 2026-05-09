# Hermes Particle Forge Research + Implementation Plan

> For Hermes: this is a research-backed build plan for Mike's own AI particle-simulation tool inspired by Casberry's AI Particle Simulator and the p5/#つぶやきProcessing generative-art posts.

Goal: Build a local, Hermes-assisted AI particle-simulation studio: prompt -> generated particle logic -> live WebGL preview -> sliders -> save/export/share locally.

Recommended name: Hermes Particle Forge

Date: 2026-05-08

---

## 1. What kicked this off

User references:
- Sadao Tokuyama post: https://x.com/tokufxug/status/2052584301792031096
- Yuruyurau post: https://x.com/yuruyurau/status/2052410599662072049
- Casberry particle simulator: https://particles.casberry.in/

The Yuruyurau post is a classic compact p5.js/#つぶやきProcessing style sketch:

```js
a=(x,y,d=mag(k=4*cos(x/21),e=y/8-20))=>circle((q=3*sin(k*2)+.3/k+sin(y/19)*k*(9+2*sin(e*14-d*3+t*2)))+50*cos(c=d-t)+200,q*sin(c)+d*39-475,k*k>15?2:1)
t=0,draw=$=>{t||createCanvas(w=400,w);background(9).noStroke().fill(w,116);for(t+=PI/240,i=1e4;i--;)a(i,i/235)}
```

Gist: 10k points are bent through trigonometry, animated by time, then drawn as circles. It is not a physics engine; it is math-driven generative motion. That is actually good for us: it means a beautiful v1 can be much simpler than real particle physics.

Japanese hashtag note:
- つぶやきProcessing = “Tweet Processing” / “muttered Processing”: a creative-coding challenge where a complete Processing/p5.js sketch fits into a tweet.

---

## 2. Research findings

### 2.1 Casberry AI Particle Simulator visible feature set

Observed UI / extracted public page behavior:

Core:
- Fullscreen WebGL/Three.js canvas.
- Default 20,000 particle point-cloud simulation.
- Preset formations: Sphere, Cube, Helix, Donut.
- Particle count, sim speed, glow intensity sliders.
- Visual styles: Spark, Plasma, Ink, Paint, Steel, Glass, Vector, Cyber.
- Smart text engine: type text, select font, select animation mode, visualize text as particles.
- Media imports: image, video, 3D model; blueprint/CAD import.
- Custom editor: paste particle logic code, name it, save local, optionally publish.
- Community cloud backed by Firebase.
- Manual drawing pad.
- Export manager supports code/media/3D export.

Important API shape used by the custom-code model:

Read-only variables:
- i: particle index
- count: total particles
- time: simulation time
- THREE: limited Three.js access

Write targets:
- target: Vector3-like object to set particle position
- color: Color-like object to set particle color

Helpers:
- addControl(id, label, min, max, initialValue)
- setInfo(title, description)
- annotate(id, positionVector, labelText)

Security/performance gate:
- Blocks browser/network/storage APIs like document, window, fetch, localStorage, WebSocket, eval, dynamic import, require, process, crypto, setTimeout, setInterval.
- Dry-runs generated code before saving.
- Recommends no object allocation in the per-particle loop.
- Recommends math over branching.
- Validates finite numbers.

### 2.2 Three.js / WebGL implementation research

Good starting point:
- Three.js Points + BufferGeometry + ShaderMaterial.
- Use Float32Array buffers for particle positions/colors/sizes.
- For 20k-100k particles, CPU-updating typed arrays can work if simple and careful.
- For 100k+ or true physics/feedback, use GPGPU/FBO simulation: particle position/velocity stored in textures and updated by fragment shaders.

References researched:
- Three.js docs: Points, BufferGeometry, ShaderMaterial.
- Maxime Heckel particle/R3F article: attributes vs uniforms; vertex shaders for GPU animation; FBOs for advanced particle simulation.
- Codrops Dreamy Particles: GPGPU particle positions in textures, GPUComputationRenderer, additive blending, bloom, interaction.

Practical decision:
- V1 should NOT start with GPGPU. Use CPU-generated positions/colors in typed arrays and Three.js Points. It will be faster to build, easy for Hermes to generate logic for, and enough for 20k-50k particles.
- V2 can add GPU shader presets and/or GPGPU FBO mode.

### 2.3 p5/#つぶやきProcessing lesson

The viral examples are compact math art, not necessarily realistic physics. A good Hermes Particle Forge should support two modes:

1. 2D p5 micro-sketch mode
   - Fast for tweet-sized snippets.
   - Uses Canvas2D or p5.js.
   - Great for short code-to-visual experiments.

2. 3D swarm mode
   - Three.js point clouds.
   - Prompt-generated per-particle formulas.
   - Sliders and export.

V1 can focus on 3D swarm mode, but keeping a 2D tab would be high-leverage because it captures the Yuruyurau vibe exactly.

---

## 3. Recommended architecture

### Product shape

Build this as a local web app first, not as a raw Hermes core tool first.

Why:
- The value is visual/interative; a web UI is the product.
- Hermes can generate code snippets and save project files using existing tools.
- A Hermes skill can encode the prompt contract and workflow without modifying Hermes core.
- A Hermes core tool can come later if we need a function like `particle_forge_generate(prompt)` exposed directly to agents.

Recommended V1 stack:
- Python Flask backend.
- Static frontend with Vite or simple ES modules.
- Three.js from npm or CDN.
- No account, no cloud, local-first.
- Project files stored under:
  - `/mnt/c/Users/Mike/Desktop/hermes-particle-forge/`
  - `projects/*.json`
  - `exports/*.png`
  - `exports/*.html`
  - `exports/*.js`

Backend responsibilities:
- Serve the app.
- Save/load local formations.
- Optional: call Hermes CLI or current agent workflow later for prompt-to-code generation.
- Export generated standalone HTML.

Frontend responsibilities:
- Three.js renderer.
- Particle engine.
- Code editor.
- Prompt box.
- Controls/sliders.
- Safety validation.
- Preview and export.

Hermes skill responsibilities:
- Give Hermes a reusable procedure for creating safe particle code.
- Enforce the mini-API contract.
- Include example formulas and style prompts.
- Include validation checklist.
- Warn against copying proprietary site code; use original implementation.

Optional future Hermes core tool:
- `particle_forge_create(prompt, style, count, out_dir)`
- `particle_forge_render(project_path, frame|video)`
- `particle_forge_validate(code)`

This should be a tool only after the local app exists and the API stabilizes.

---

## 4. V1 feature spec

### Must-have

1. Desktop-launchable local app
   - `start_particle_forge.bat` on Desktop.
   - Opens browser at `http://127.0.0.1:7867`.

2. Fullscreen Three.js canvas
   - Black/neon default style.
   - OrbitControls.
   - Bloom-like glow if easy; otherwise additive point sprites first.

3. Particle engine
   - Default count: 20,000.
   - User-adjustable count: 1,000-100,000.
   - Per-particle typed arrays for position/color/size.
   - Preset formations: sphere, cube, helix, donut, galaxy, smoke/jellyfish.

4. Safe custom code editor
   - User writes/generates code body with signature:
     - inputs: i, count, time, params, Math
     - outputs: position {x,y,z}, color {r,g,b}, size
   - No DOM/network/storage APIs inside user code.
   - Dry-run validator before apply.
   - Runtime guard against NaN/Infinity.

5. Prompt-to-code workflow
   - Prompt box: “make a blue jellyfish made of sparks, breathing slowly.”
   - In V1, Hermes generates code through the normal chat/agent path and writes it into a saved project or clipboard-style field.
   - App can include a “Generate prompt for Hermes” button that copies a structured prompt.
   - Later: backend can call configured LLM provider directly.

6. Sliders
   - Speed
   - Scale
   - Glow/opacity
   - Particle size
   - Custom sliders generated by code metadata or `addControl(...)` equivalent.

7. Save/load local formations
   - Name, prompt, code, params, visual style.
   - Save to local JSON via Flask endpoint.

8. Export
   - PNG snapshot.
   - Standalone HTML export containing the formation and renderer.
   - JSON project export.

### Nice-to-have V1.5

- Text-to-particles.
- Image-to-particles.
- 2D p5 micro-sketch tab.
- Video/GIF export via MediaRecorder.
- Gallery grid of local saved formations.
- “Remix this” button.

### Defer to V2

- Firebase/community cloud.
- 3D model import/export GLB/OBJ.
- GPGPU/FBO simulation.
- Hand gesture controls.
- Real physics engine integration.
- Multiplayer/collab.

---

## 5. Safety and originality notes

Do not clone/copy Casberry source into our project. We can use it as product research, but our implementation should be original.

Safe to reuse as general ideas:
- The mini-API concept: particle index, count, time, target/color outputs.
- The UI concept: fullscreen canvas + side controls + preset carousel + export.
- Performance constraints: typed arrays, no allocations in hot loops, finite number guards.

Do not copy:
- Their exact code, assets, Firebase configuration, UI CSS, visual branding, or community backend.

Security for generated code:
- Treat user/AI-generated JS as untrusted.
- Best V1 approach: validate heavily and execute only a function body with restricted arguments.
- Better V1.5 approach: run user code inside a sandboxed iframe or Web Worker with no network permissions.
- Never allow generated code to access secrets, filesystem, shell, or Hermes config.

---

## 6. Hermes skill proposal

Skill name:
- `hermes-particle-forge`

Category:
- `creative`

Description:
- “Use when generating, validating, or iterating AI particle-simulation formulas for Hermes Particle Forge: prompt-to-Three.js point clouds, p5 micro-sketches, safe code constraints, presets, exports, and visual QA.”

Skill contents:
1. Trigger conditions.
2. The Particle Forge mini-API.
3. Safe code generation contract.
4. Formula recipes:
   - sphere
   - helix
   - torus/donut
   - galaxy spiral
   - jellyfish
   - smoke plume
   - waveform/audio-like ribbon
   - text cloud
5. Style prompt templates:
   - neon cyber
   - ink wash
   - plasma fire
   - glass hologram
   - vector CRT
6. Validation checklist:
   - no forbidden APIs
   - no allocations in per-particle loop
   - finite positions/colors/sizes
   - works at 1k, 20k, 50k particles
   - export works
7. QA workflow with screenshots/browser console.
8. How to save project JSON and standalone HTML.

This skill is worth making after the first prototype exists, so it can document the actual API instead of guessing.

---

## 7. Optional Hermes core tool proposal

Toolset name:
- `particle_forge`

Possible functions:

```python
particle_forge_validate(code: str) -> dict
particle_forge_template(kind: str, style: str) -> dict
particle_forge_project(prompt: str, code: str, name: str, out_dir: str) -> dict
particle_forge_export_html(project_path: str) -> dict
```

When to implement:
- After the web app has stable JSON project schema.
- After we know which operations are useful from agent chat.

Why not first:
- Hermes already has file, terminal, browser, and code tools.
- The main missing thing is the app itself, not an agent wrapper.

---

## 8. Concrete implementation plan

Suggested project path:
- `/mnt/c/Users/Mike/Desktop/hermes-particle-forge/`

### Task 1: Scaffold local Flask app

Files:
- Create: `app.py`
- Create: `requirements.txt`
- Create: `static/index.html`
- Create: `static/app.js`
- Create: `static/style.css`
- Create: `projects/.gitkeep`
- Create: `exports/.gitkeep`
- Create: `start_particle_forge.bat`

Implementation:
- Flask serves `/` and static files.
- API endpoints:
  - `GET /api/projects`
  - `POST /api/projects`
  - `GET /api/projects/<name>`
  - `POST /api/export/html`

Verification:
- `python app.py`
- Open `http://127.0.0.1:7867`
- Confirm page loads.

### Task 2: Build Three.js particle renderer

Files:
- Modify: `static/app.js`
- Modify: `static/index.html`
- Modify: `static/style.css`

Implementation:
- Use Three.js ES modules from CDN or npm build.
- Create scene/camera/renderer.
- Create BufferGeometry with position/color/size arrays.
- Render using PointsMaterial first, then ShaderMaterial for soft circles.
- Animation loop updates typed arrays.

Verification:
- 20k sphere renders smoothly.
- Browser console clean.

### Task 3: Add preset formations

Presets:
- sphere
- cube
- helix
- donut
- galaxy
- jellyfish

Implementation:
- Each preset is a JS function: `(i, count, time, params, out) => out`.
- UI buttons switch active preset.
- Info panel explains current preset.

Verification:
- Switching presets does not leak memory or break controls.

### Task 4: Add safe custom code editor

Implementation:
- Textarea/code editor.
- Forbidden keyword scan.
- Compile with Function constructor only after validation.
- Dry-run with sample particles.
- Runtime finite guard.

Important: This is acceptable only because it is local and heavily restricted. For stronger isolation later, move custom code to a sandboxed Worker/iframe.

Verification:
- Valid formula applies.
- `fetch(...)`, `document`, `window`, `localStorage`, `eval` are blocked.
- Bad math producing NaN is clamped or rejected.

### Task 5: Add prompt-to-Hermes helper

Implementation:
- Prompt box.
- Button “Copy Hermes generation prompt.”
- The copied prompt includes:
  - user visual request
  - mini-API
  - forbidden APIs
  - output-only JS function body
  - performance rules

Verification:
- Pasted prompt into Hermes produces a usable function body.

### Task 6: Add local save/load

Implementation:
- JSON schema:
```json
{
  "name": "blue_jellyfish",
  "prompt": "blue jellyfish made of sparks",
  "code": "...",
  "params": {"speed":1,"scale":1},
  "style": "spark",
  "created_at": "..."
}
```

Verification:
- Save project.
- Reload page.
- Load project.
- Formation returns.

### Task 7: Add exports

Implementation:
- PNG snapshot via canvas `toDataURL`.
- JSON download.
- Standalone HTML export from backend template.

Verification:
- Exported PNG opens.
- Exported HTML opens offline or from file/server.

### Task 8: Create Hermes skill after API stabilizes

Files:
- Create local skill: `~/.hermes/skills/creative/hermes-particle-forge/SKILL.md`

Contents:
- Actual API, examples, validation rules, QA steps, style recipes.

Verification:
- New session can load `hermes-particle-forge` skill.
- Skill can generate at least 3 good custom formulas.

---

## 9. First formulas to ship

### Geodesic-ish sphere

```js
const u = i / count;
const phi = Math.acos(1 - 2 * u);
const theta = Math.PI * (1 + Math.sqrt(5)) * i + time * 0.15;
const r = params.scale * 22;
out.x = r * Math.sin(phi) * Math.cos(theta);
out.y = r * Math.cos(phi);
out.z = r * Math.sin(phi) * Math.sin(theta);
out.r = 0.2 + 0.4 * Math.sin(u * 12 + time);
out.g = 0.9;
out.b = 0.45;
out.size = 1.5;
```

### Yuruyurau-inspired 2D trig smoke, adapted to 3D

```js
const x = i;
const y = i / 235;
const k = 4 * Math.cos(x / 21);
const e = y / 8 - 20;
const d = Math.sqrt(k*k + e*e) + 0.0001;
const q = 3*Math.sin(k*2) + 0.3/k + Math.sin(y/19)*k*(9 + 2*Math.sin(e*14 - d*3 + time*2));
const c = d - time;
out.x = (q + 50*Math.cos(c)) * 0.15;
out.y = (q*Math.sin(c) + d*39 - 475) * 0.08;
out.z = Math.sin(i * 0.013 + time) * 8;
out.r = 0.7;
out.g = 0.9;
out.b = 1.0;
out.size = k*k > 15 ? 2.0 : 1.0;
```

### Galaxy spiral

```js
const u = i / count;
const arm = i % 5;
const angle = u * 80 + arm * Math.PI * 2 / 5 + time * 0.1;
const radius = Math.sqrt(u) * 35 * params.scale;
const jitter = Math.sin(i * 12.989) * 0.8;
out.x = Math.cos(angle) * radius + jitter;
out.y = (Math.sin(i * 0.137 + time) * 2.5) * (1-u);
out.z = Math.sin(angle) * radius + Math.cos(i * 7.31) * 0.8;
out.r = 0.4 + u * 0.6;
out.g = 0.55 + 0.25 * Math.sin(angle);
out.b = 1.0;
out.size = 1.2 + (1-u) * 2.2;
```

---

## 10. Recommendation

Yes, we can make one.

Best path:
1. Build the local web app first: `Hermes Particle Forge`.
2. Use Hermes as the AI generator through a structured prompt in V1.
3. Add a real local Hermes skill once the mini-API stabilizes.
4. Add a Hermes core tool only if we want direct agent-callable project generation/export.

I recommend starting with a two-day-ish V1:
- Day 1: Flask + Three.js renderer + presets + editor.
- Day 2: prompt helper + save/load + export + polish.

The fastest useful first milestone is a Desktop `.bat` that launches a local app where Mike can click presets, paste Hermes-generated code, and export a PNG/standalone HTML.

---

## 11. Competitive research: has anyone made this already?

Short answer: yes, pieces of this exist. The closest direct clone exists. But there is still a strong opening for a better local-first, Hermes-native version.

### 11.1 Closest direct match: Sanjays2402/ai-particle-simulator

URL:
- https://github.com/Sanjays2402/ai-particle-simulator
- Live demo: https://sanjays2402.github.io/ai-particle-simulator/

What it is:
- React + Vite + Three.js / React Three Fiber particle app.
- OpenAI-compatible prompt-to-particle-code flow.
- 25+ presets.
- Command palette.
- Dynamic controls generated from `addControl(...)` calls.
- Themes/styles.
- Screenshot, GIF, HTML export.
- MIT license.

Important overlap with our idea:
- It uses nearly the same mini-API concept:
  - `i`, `count`, `target`, `color`, `time`, `THREE`, `addControl`, `setInfo`, `controls`.
- It compiles user/AI code with `new Function(...)`.
- Its system prompt is basically a concise version of the prompt contract we were planning.

Observed weakness:
- In browser testing, the live demo dropped from 20K to 18K, then to 2K particles, while showing only ~4 FPS in the session.
- That means the implementation is probably CPU-bound: it calls generated JS for every particle every frame, mutates BufferGeometry, and relies on React/R3F overhead/postprocessing.
- Some built-in example code uses `Math.random()` in per-frame particle code, which creates flicker/non-determinism and costs performance.
- AI generation requires user-provided API key in the web UI. This is convenient but not ideal for a local Hermes workflow; browser-stored API keys are a trust/UX concern.

How we can beat it:
- Make V1 local-first and Hermes-native: no browser API key entry required.
- Use deterministic per-particle seeded noise instead of `Math.random()` per frame.
- Separate “shape sampling” from “animation update” so static expensive work is precomputed once.
- Offer a performance ladder:
  1. CPU typed-array mode for easy formulas.
  2. Generated GLSL vertex-shader mode for fast formulas.
  3. Later GPGPU/FBO mode for 100k-1M particles.
- Provide a formula linter/performance grader before code runs.
- Make export/deploy cleaner: standalone HTML, local project folders, maybe “send this to Hermes to improve” loop.

Verdict:
- This proves the idea is viable and open-source.
- It also shows the danger: a feature-rich React/R3F implementation can become slow fast.
- Hermes Particle Forge should be less “UI clone” and more “fast local creative-coding instrument.”

### 11.2 Casberry AI Particle Simulator

URL:
- https://particles.casberry.in/

What it does well:
- Strong product polish and visual direction.
- Fullscreen immersive black/neon interface.
- 20K particle default.
- Custom editor.
- Smart text engine.
- Image/video/model/blueprint imports.
- Local formations plus community cloud.
- Code/media/3D export claims.
- Strong safety messaging around forbidden APIs and zero-garbage hot loops.

Limitations / unknowns:
- Not clearly open source from the site surface.
- Uses Firebase/community backend and public app config.
- The “AI” flow seems partly prompt-template oriented: copy a prompt to Gemini/Claude, paste code back, or use a built-in smart engine depending on mode.
- We should not copy source/assets/branding.

How we can beat it:
- Local-first / private by default, matching Mike’s preference.
- Desktop `.bat` launch, no account, no cloud dependency.
- Hermes integration: ask Hermes for a simulation, get code directly into the project.
- Better dev export: standalone HTML plus React/Three.js modules plus project JSON.
- Better transparency: show generated code, linter score, performance estimate, and exactly why code was blocked.

### 11.3 Babylon.js Node Particle Editor / Particle Editor

URLs:
- https://npe.babylonjs.com/
- https://doc.babylonjs.com/features/featuresDeepDive/particles/particle_system/node_particle_editor/
- https://doc.babylonjs.com/legacy/inspector/particleEditor/

What it is:
- Serious particle authoring tool inside the Babylon ecosystem.
- Visual node graph for particle systems.
- Creation phase, update phase, system block.
- Randomization and gradients.
- Multi-system triggers.
- Exports/loads particle system snippets/JSON.

Important note from docs:
- NPE currently generates CPU-based particle systems.

How it differs from our goal:
- It is a visual node editor, not prompt-to-generative-art-first.
- It targets Babylon engine workflows, not Hermes/Three.js/local creative-coding workflows.
- It is better for conventional VFX emitters; less aimed at tweet-sized mathematical swarm art.

What to borrow conceptually:
- Node/phase separation: creation vs update vs system.
- Gradients over particle lifetime.
- Trigger/event chains for multi-system effects.
- JSON export/import discipline.

### 11.4 Nebula / three-nebula

URL:
- https://discourse.threejs.org/t/nebula-a-fully-featured-particle-system-designer-for-three/21854

What it is:
- Three.js-focused particle system designer and engine.
- Desktop visual designer plus open-source renderer (`three-nebula`).
- Saves systems as JSON and renders them in Three.js/WebGL.
- Described as aiming for AAA-style particle workflows.

How it differs:
- More classic VFX particle designer than AI prompt-to-formula tool.
- Likely better for emitters/fire/smoke/explosions than for mathematical particle sculptures.

What to borrow:
- JSON as the core interchange format.
- “Designer app + renderer library” separation.
- Effects can be loaded into any Three.js app.

### 11.5 PlayCanvas Particle System Editor

URLs:
- https://developer.playcanvas.com/user-manual/graphics/particles/
- https://developer.playcanvas.com/user-manual/editor/scenes/components/particlesystem/

What it is:
- Mature game-engine particle system editor.
- Supports emission, lifetime, rate, textures, sprite sheets, mesh particles, blending, sorting, soft particles, curves over lifetime, local/world/screen space.

How it differs:
- Great traditional particle emitter editor.
- Not prompt-native.
- Not focused on generative mathematical art or Hermes agent workflows.

What to borrow:
- Curves over lifetime: scale/color/opacity/velocity.
- Sprite sheet support later.
- Soft-particle/depth concepts later.

### 11.6 Gesture-controlled AI particle demos

Examples:
- https://github.com/zeeshan020dev/Interactive-3D-Particle-System
- https://github.com/ux-utkarsh/Gesture-Particle

What they do:
- Three.js + MediaPipe/TensorFlow hand tracking.
- Webcam gesture controls: rotate, morph, implode, pinch, audio reactivity.
- Some were generated from Google AI Studio/Gemini prompts.

How they differ:
- More demo than authoring environment.
- Interaction gimmick is strong, but prompt-to-save/export workflow is usually weak.

What to borrow later:
- Gesture controls as a V2 feature.
- Audio reactivity as V1.5/V2.
- Keep it optional; do not let webcam/audio dependencies complicate V1.

### 11.7 p5.js AI/tutorial/skill ecosystem

Examples:
- https://p5js.ai/tutorials/random
- https://www.aiuxplayground.com/skills/algorithmic-art
- OpenProcessing: https://openprocessing.org/browse/

What exists:
- AI-assisted p5.js education.
- Skills/prompts for algorithmic art with seeded randomness and philosophy-first creation.
- Huge ecosystem of p5/OpenProcessing sketches.

How it differs:
- Mostly 2D creative coding, not 3D particle-swarm authoring.
- Good for learning and small sketches, not necessarily production export.

What to borrow:
- Seeded randomness.
- “Algorithmic philosophy” metadata: name the movement/style and explain the system.
- p5 micro-sketch mode as a side tab.

### 11.8 Shadertoy / AI shader workflows

Example:
- Qt Design Studio docs show AI-generated Shadertoy-style shader code adapted into its Effect Composer.

What exists:
- AI can generate GLSL fragment shaders.
- Shader editors are mature, but they are mostly pixel shaders, not particle authoring systems.

What to borrow:
- A second generation target: GLSL shader mode.
- “Copy only the body” style code-contract enforcement.
- Compilation/error feedback loop.

---

## 12. How Hermes Particle Forge can be better

The better version is not “Casberry clone” or “Sanjay clone.” It should be a local-first, agent-native, performance-aware particle instrument.

### 12.1 Differentiator: local-first and private

Most tools are web-hosted or require entering an API key in a browser UI. Hermes Particle Forge should:
- Run from Mike’s Desktop with a `.bat`.
- Store projects locally.
- Use Hermes’ existing model/provider config instead of asking for browser API keys.
- Work offline for presets and saved formulas.
- Export standalone HTML with no backend dependency.

### 12.2 Differentiator: performance-aware generation

Add a “Particle Linter” that scores generated code before it runs:

Checks:
- Forbidden APIs.
- `new` allocations inside hot loop.
- `Math.random()` inside hot loop.
- unbounded loops.
- NaN/Infinity risks.
- too many trig calls.
- branch-heavy code.
- use of unsupported globals.

Output:
- Safety: pass/fail.
- Performance: green/yellow/red.
- Determinism: stable/flickery.
- Suggested fix: “replace Math.random with hash(i)” etc.

This is a major way to beat the existing direct clone.

### 12.3 Differentiator: deterministic creative coding

Provide built-in deterministic helpers:

```js
hash(i)
noise1(i)
noise2(i, seed)
palette(name, t)
ease(t)
rotateY(x, z, angle)
```

Then generated formulas can be stable across frames and export reproducibly.

### 12.4 Differentiator: two-stage engine

Instead of one JS function that recomputes everything every frame, use two optional functions:

```js
initParticle(i, count, seed, base)
updateParticle(i, count, time, base, out, params)
```

`initParticle` precomputes stable values once.
`updateParticle` only animates.

This makes complex scenes faster and easier to reason about.

### 12.5 Differentiator: multiple generation targets

Support three modes over time:

1. JS formula mode
   - Easiest and most debuggable.
   - Good for V1.

2. GLSL vertex shader mode
   - Much faster for large counts.
   - Good for V1.5.

3. GPGPU/FBO mode
   - True particle state simulation.
   - Good for V2 / 100k-1M particles.

Existing AI particle demos usually stop at JS formula mode.

### 12.6 Differentiator: Hermes review loop

Buttons/workflows:
- “Ask Hermes to improve this.”
- “Ask Hermes to make this faster.”
- “Ask Hermes to add sliders.”
- “Ask Hermes to make it more like jellyfish/smoke/galaxy.”
- “Ask Hermes to explain the math.”

Because Hermes has file access, it can update project JSON, generate variants, and preserve a history.

### 12.7 Differentiator: variant gallery

When given a prompt, generate 4 variants:
- elegant/simple
- chaotic/high-energy
- cinematic/glowy
- pixel/vector/minimal

Render thumbnails or preview buttons. This is better than a single prompt result.

### 12.8 Differentiator: asset/export focus

Since Mike often wants practical creative outputs, make exports first-class:
- PNG at chosen resolution.
- Transparent PNG if possible.
- Short WebM/GIF loop.
- Standalone HTML.
- React/Three component export.
- JSON project.
- Later: sprite sheet export for games.

### 12.9 Differentiator: “tweet-sized formula” mode

Support p5/#つぶやきProcessing-inspired micro-code:
- paste a tweet-sized p5 snippet
- run it safely in 2D canvas mode
- ask Hermes to expand it into readable code
- ask Hermes to port it to 3D swarm mode

This bridges the Yuruyurau inspiration directly.

### 12.10 Differentiator: honest quality gates

Before calling it good, the app should auto-test:
- loads cleanly
- no console errors
- 20K particles at target FPS on Mike’s machine
- screenshot export works
- standalone HTML export opens
- generated formula passes linter
- no browser API key required

This would already make it more dependable than the closest live demo observed.

---

## 13. Revised recommendation after competitive research

Yes, someone has made close versions already. The closest is `Sanjays2402/ai-particle-simulator`, and it is useful proof-of-concept / inspiration.

But yes, we can make a better one by focusing on:
1. Local-first Hermes integration.
2. Performance/linting/determinism.
3. Two-stage init/update formulas.
4. Export-quality assets.
5. Prompt-to-variants workflow.
6. p5 tweet-code-to-3D-port workflow.

Recommended V1 adjustment:
- Do not start with React/R3F. Use vanilla Three.js or very lightweight Vite + vanilla modules to reduce overhead.
- Build a stable renderer first, then add AI code generation.
- Use deterministic helper functions from day one.
- Add the linter before letting arbitrary generated code animate.

V1 success target:
- 20K particles at smooth FPS on Mike’s machine.
- If 20K is not smooth, we fix architecture before adding more features.
- No silent quality downgrade to 2K without making it obvious and actionable.
