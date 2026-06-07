---
description: Solar system simulator development agent
applyTo: "**/*"
---

# SolarMap Development Rules

## Architecture
- Single `index.html` file contains all HTML, CSS, and JavaScript
- No build tools, bundlers, or package managers — vanilla web technologies only
- WebGL 2.0 for all rendering (no Canvas 2D fallback)
- All state managed in a single global `state` object

## Code Style
- Use ES2020+ features (modules not needed since single-file)
- `const` by default, `let` only when reassignment is needed, never `var`
- Descriptive function and variable names — no abbreviations except well-known ones (e.g., `ctx`, `gl`, `dt`)
- Group code into clearly commented sections: Data, Shaders, Rendering, UI, State, Input
- Keep shader code in template literals, clearly labeled

## Rendering
- All astronomical distances in AU internally, convert for display
- All body sizes in km internally
- Use J2000 epoch orbital elements for position calculations
- Frame time in seconds, use `requestAnimationFrame` loop
- Camera uses spherical coordinates (distance, azimuth, elevation)

## Performance
- Shader uniform locations cached at init, never looked up per-frame
- Minimize GPU state changes — batch similar draws
- Asteroid belt uses instanced rendering
- Avoid allocations in the render loop — reuse vectors/matrices

## UI / UX
- All interactive controls must have keyboard equivalents
- Filter/display state persists in `localStorage` under key `solarmap_state`
- Reset button clears `localStorage` and reloads
- Tooltips or labels for all controls

## Quality
- No `console.log` in committed code except error handling
- Validate WebGL context creation with user-friendly error message
- Graceful degradation if textures fail to load
