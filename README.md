# Hermes Particle Forge

A local-first generative particle-art instrument for turning compact mathematical formulas into cinematic, interactive particle worlds.

Particle Forge is built as a single-file browser app: no account, no build step, no server requirement, and no cloud dependency. Open it, choose a curated scene, perform the system with camera/palette/trail controls, then export stills or exact recipe JSON.

Live demo: https://123mikeyd.github.io/particle-forge-v2-cool-pass/

## Preview

The repository includes a short motion capture at `assets/demo.mp4`.

## Highlights

- Curated scene presets: Moonflower Engine, Butterfly Reactor, Haunted Aquarium, Dog Star Field, Strange Orchid, and Solar Phyllotaxis.
- Formula families: Trig Bloom, Aurora Jellyfish, Rose/Rhodonea Bloom, Lissajous Orchid, Golden Phyllotaxis, De Jong Attractor, Biolume Fireflies, Polar Sine Orbitals, and Lorenz Spiral Attractor.
- Performance-oriented Canvas2D renderer with up to 40,000 particles.
- Director Mode auto-tours for ambient, slow, and chaotic presentations.
- Palette, trail-material, camera-motion, mouse-warp, formula-blending, and auto-morph controls.
- Gallery mode and fullscreen mode for clean demos/screen recordings.
- Recipe JSON copy/load plus local browser save/load.
- PNG export for still artwork.
- Keyboard and mouse navigation: `G` gallery, `F` Forge 3D Port, `Space` pause, `WASD`/arrows pan, mouse wheel zoom, drag pan.

## Quick start

Option 1: open `index.html` directly in a browser.

Option 2: serve locally.

```bash
python3 -m http.server 8765
```

Then visit:

```text
http://127.0.0.1:8765/
```

## Recommended first run

1. Pick a `Scene Preset`.
2. Switch to `Forge 3D Port`.
3. Try `Director Mode` → `Slow Tour`.
4. Toggle `Hide UI / Gallery` or press `G`.
5. Use `Formula Blend` + `Auto Morph` for hybrid math creatures.
6. Export a PNG or copy Recipe JSON when you find a good state.

## Repository contents

```text
index.html                              Standalone Particle Forge app
assets/demo.mp4                         Short V2 demo capture
docs/product-plan.md                    Product direction and long-range roadmap
docs/research-and-architecture.md       Research notes and architecture decisions
docs/formula-test-pack.md               Candidate formula scenes and benchmark ideas
docs/benchmark-001-yuruyurau-trig-bloom.html
                                        Original benchmark preserved as a standalone page
LICENSE                                 MIT license
```

## Design principles

- Beauty before feature count.
- Local-first and private by default.
- Mathematical structure should remain readable before bloom/trails/camera effects are layered on.
- Performance is part of the art: visible particle counts, tuned defaults, and no random-looking startup state.
- Export matters: stills, recipes, and standalone HTML should be easy to keep or share.

## Status

Prototype / art-tool workbench. The app is already usable as a standalone generative-art toy and as a benchmark bed for future Hermes-assisted formula generation.

Near-term roadmap:

- Compress the demo video into a lightweight README-friendly GIF/WebM.
- Split the single-file prototype into `static/js`, `static/css`, and reusable formula modules.
- Add project-file save/load beyond browser local storage.
- Add WebGL/Three.js renderer path for higher particle counts.
- Add Hermes prompt-to-formula workflow once the local API contract stabilizes.

## Credits and inspiration

Created by Mike with Hermes Agent.

Inspired by compact mathematical art, ShaderToy-style fragment formulas, tweet-sized p5 sketches, and strange-attractor / polar-orbital generative art traditions.

Specific inspirations preserved in the app and docs include:

- Yuruyurau-style compact p5 trig-bloom math art.
- User-provided polar sine orbital GLSL-style formula.
- User-provided Lorenz spiral p5 formula.
- Classic mathematical families: rose/rhodonea curves, Lissajous figures, phyllotaxis, De Jong attractors, and Lorenz-style attractors.

If a compact formula lineage needs more precise attribution, open an issue or PR and the credit trail can be improved.

## License

MIT. See `LICENSE`.
