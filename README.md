# Particle Forge V2 Cool Pass

A standalone browser-based math-art particle forge: compact equations become art-directed particle worlds with scene recipes, palettes, trails, camera choreography, mouse interaction, formula blending, Director Mode, Gallery Mode, and recipe save/load.

Open `index.html` directly or serve it locally:

```bash
python3 -m http.server 8765
```

Then visit `http://127.0.0.1:8765/`.

## What it does

Particle Forge turns small mathematical sketches into playable visual worlds. Pick a curated scene, switch to Forge 3D Port, then use Palette, Trail, Camera, Mouse Warp, Formula Blend, and Director Mode to perform the artwork live.

## Features

- Formula worlds:
  - Trig Bloom
  - Aurora Jellyfish
  - Rose / Rhodonea Bloom
  - Lissajous Orchid
  - Golden Phyllotaxis
  - De Jong Attractor
  - Biolume Fireflies
  - Polar Sine Orbitals
  - Lorenz Spiral Attractor
- Curated Scene Presets:
  - Moonflower Engine
  - Butterfly Reactor
  - Haunted Aquarium
  - Dog Star Field
  - Strange Orchid
  - Solar Phyllotaxis
- Palette Worlds:
  - Aurora
  - Ghost Glass
  - Infrared
  - CRT Bloom
  - Oil Slick
  - Solar Flare
  - Deep Sea Biolume
  - Dog Star Gold
- Trail Materials:
  - Clean
  - Ghost Smear
  - Long Exposure
  - CRT Phosphor
  - Ink Bleed
  - Fire Ember
  - Vaporwave Persistence
  - Star-map Accumulation
- Camera Motion:
  - Slow Orbit
  - Locked Front
  - Breathing Zoom
  - Handheld Drift
  - Spiral Dive
  - Macro Inspection
  - Kaleidoscope Orbit
- Director Mode auto-tour:
  - Slow Tour
  - Chaos Tour
  - Ambient Tour
- Fullscreen and Hide UI / Gallery mode
- Mouse Warp interaction:
  - Attract
  - Repel
  - Swirl
- Formula Blend:
  - Blend target formula
  - Blend amount
  - Auto Morph animation
- Recipe tools:
  - Copy Recipe JSON
  - Load Recipe JSON
  - Save Local
  - Load Local
- PNG export
- Mouse wheel zoom, drag pan, WASD/arrows pan

Each curated scene starts from tuned safe parameters so the app does not load into ugly/random-looking defaults.

## Controls

- Pick `Scene Preset` first.
- Use `Forge 3D Port` for the full particle-world version.
- Use `Director Mode` for an automatic curated tour.
- Use `Hide UI / Gallery` or `Fullscreen` for presentation.
- Move the mouse over the canvas when Mouse Warp is enabled.
- Use Formula Blend + Auto Morph for hybrid math creatures.
- Use Recipe JSON to copy/share/save exact states.

Keyboard shortcuts:

- `G`: toggle Gallery Mode
- `F`: switch to Forge 3D Port
- `Space`: pause/play
- `WASD` / arrow keys: pan view

## Credits / inspiration

Made with Hermes Agent and Mike.

Inspired by compact mathematical art, shader-toy-style fragment formulas, tweet-sized p5 sketches, and strange-attractor / polar-orbital generative art traditions.

Specific inspirations preserved in the app include:

- Yuruyurau-style trig bloom compact p5 math-art.
- User-provided polar sine orbital GLSL-style formula.
- User-provided Lorenz spiral p5 formula.
- Classic mathematical families including rose/rhodonea curves, Lissajous figures, phyllotaxis, De Jong attractors, and Lorenz-style attractors.

If you recognize a compact formula lineage here and want a more precise attribution, open an issue/PR and we will gladly improve the credit trail.

## Status

This is an early public art-tool prototype. It is already pretty frickin' cool, and it will get better.

## License

MIT
