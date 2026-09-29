# Black Hole

A real-time 3D black hole simulation rendered in the browser with WebGL.

![Three.js](https://img.shields.io/badge/three.js-r170-black?logo=three.js) ![GLSL](https://img.shields.io/badge/GLSL-shaders-blue)

## Overview

Interactive visualization of a black hole with a glowing accretion disk:

- 5,000 instanced streak particles orbiting the singularity
- Doppler-shifted coloring (approaching side shifts hot, receding side shifts cool)
- Orbital compression pulled inward by the event horizon
- Procedural flow noise deforming the disk plane
- Additive rim glow around the event horizon sphere

## Controls

| Input            | Action            |
| ---------------- | ----------------- |
| Drag             | Rotate camera     |
| Scroll           | Zoom in / out     |

## How to run

The project has no build step. Open `index.html` directly in a modern browser, or serve the folder with any static file server:

```bash
python -m http.server 8000
```

Then visit `http://localhost:8000`.

Three.js is loaded from a CDN via import map, so an internet connection is required on first load.

## Implementation notes

The disk is a single `InstancedMesh` of tapered cylinders. Each instance is transformed on the GPU inside a custom vertex shader:

1. The initial random orbit is read from `instanceMatrix`.
2. A per-instance orbital velocity is computed as `1.5 / sqrt(r)` (Keplerian falloff).
3. `uCompression` scales orbital radii toward the event horizon.
4. A noise field on the `x/z` plane adds vertical turbulence that evolves over time.
5. The look vector is projected into the orbit tangent to compute the doppler factor.

All animation state is passed through shader uniforms (`uTime`, `uMorph`, `uCompression`, `uIntensity`, `uOrbitScale`), so no per-frame CPU work happens beyond rendering.

## Structure

```
Black-Hole/
├── index.html   # entire simulation (HTML, CSS, shaders, JS)
└── README.md
```

## License

[MIT](LICENSE)