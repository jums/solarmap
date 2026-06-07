# SolarMap — Interactive Solar System Simulator

A real-time, interactive solar system simulator running entirely in a single HTML file. Explore the solar system with realistic orbital mechanics, adjustable scales, and a clean space-themed interface.

**Live Demo**: [https://jums.github.io/solarmap](https://jums.github.io/solarmap)

---

## Features

### Celestial Bodies
- **8 major planets** (Mercury through Neptune) with accurate orbital elements
- **5 dwarf planets** (Pluto, Ceres, Eris, Haumea, Makemake)
- **14 major moons** across all outer planets and Earth/Mars
- **Sun** with glow effect
- **Saturn's rings** with Cassini division detail
- **Asteroid belt** — 8,000 particles between 2.2–3.3 AU with correct Keplerian motion

### Orbital Mechanics
- Positions calculated from **J2000 epoch orbital elements**
- Kepler's equation solved iteratively for true anomaly
- Bodies use real semi-major axes, eccentricities, inclinations, and longitudes
- Current date positions at startup (Julian Date calculation)
- **2.5D visualization** — orbital inclinations visible when tilting the view

### Controls
| Action        | Mouse                    | Keyboard  |
| ------------- | ------------------------ | --------- |
| Rotate view   | Left-drag                | —         |
| Pan           | Right-drag / Middle-drag | —         |
| Zoom          | Scroll wheel             | `+` / `-` |
| Reset view    | —                        | `R`       |
| Top-down view | —                        | `T`       |
| Pause/Resume  | —                        | `Space`   |

### Display Modes
- **Body Size Scale** — from exaggerated (visible at overview) to astronomically realistic
- **Distance Scale** — from compact/logarithmic to full linear realism
- **Tilt (3D)** — adjust viewing elevation to see orbital plane differences
- **Presets**: Full Realism, Overview, Inner System

### Filtering
- Toggle visibility of: Planets, Dwarf Planets, Moons, Asteroid Belt, Orbit Lines, Labels
- All filter states persisted to `localStorage`

### Time Simulation
- Adjustable time speed: real-time to ±365 days/second
- Pause/resume
- Current simulation date displayed

### Body Info
- Click any body to see details: radius, semi-major axis, orbital period, eccentricity, inclination

### State Persistence
- All display settings, filter states, camera position, and time speed saved to `localStorage`
- **Reset All** button clears everything and reloads

---

## Implementation Status

| Feature                           | Status     |
| --------------------------------- | ---------- |
| Planetary orbits & positions      | ✅ Complete |
| Dwarf planets                     | ✅ Complete |
| Major moons                       | ✅ Complete |
| Asteroid belt                     | ✅ Complete |
| Orbit lines with distance scaling | ✅ Complete |
| Camera controls (rotate/pan/zoom) | ✅ Complete |
| Size & distance scaling           | ✅ Complete |
| 2.5D tilt view                    | ✅ Complete |
| Time simulation                   | ✅ Complete |
| Body info on click                | ✅ Complete |
| Saturn rings                      | ✅ Complete |
| Star field background             | ✅ Complete |
| localStorage persistence          | ✅ Complete |
| Label overlay                     | ✅ Complete |
| Body textures (when zoomed in)    | 🔲 Planned  |
| Touch/mobile controls             | 🔲 Planned  |
| Orbit trail animation             | 🔲 Planned  |
| Search/focus on body              | 🔲 Planned  |

---

## Technical Overview

### Architecture
- **Single file**: Everything in `index.html` — no build tools, frameworks, or external JS dependencies
- **WebGL 2.0**: All rendering via GPU shaders for smooth 60fps performance
- **~1100 lines** of vanilla JavaScript

### Rendering Pipeline
1. **Star field** — 2000 random points on a sphere, rendered as GL_POINTS
2. **Asteroid belt** — 8000 points with individual orbital velocities via vertex shader
3. **Orbit lines** — 256-segment parametric curves computed in vertex shader from orbital elements
4. **Bodies** — Billboarded quads with procedural sphere shading (Lambertian + edge smoothing)
5. **Saturn rings** — Billboarded quad with ring/gap masking in fragment shader
6. **Labels** — DOM overlay for crisp text at any zoom (not GPU-rendered)

### Orbital Mechanics
- Kepler's equation solved via Newton-Raphson iteration (30 iterations max, 1e-8 tolerance)
- 3D position computed using the standard rotation matrices from orbital elements (Ω, ω, i)
- Distance scaling applied consistently across orbit lines, asteroid belt, and body positions using a power function: `sign(au) * pow(abs(au), p) * m`

### Fonts
- **Orbitron** — geometric/futuristic, used for titles and labels
- **Exo 2** — clean sans-serif, used for UI controls and info text
- Both loaded from Google Fonts CDN

### Browser Support
- Requires **WebGL 2.0** (Chrome 56+, Firefox 51+, Edge 79+, Safari 15+)
- Graceful error message shown if WebGL 2.0 unavailable

---

## Running Locally

Just open `index.html` in a modern browser:

```bash
# Option 1: Direct open
open index.html          # macOS
start index.html         # Windows
xdg-open index.html      # Linux

# Option 2: Local server (avoids potential CORS issues with fonts)
npx serve .
# or
python -m http.server 8000
```

## Deployment

Hosted via GitHub Pages from the `main` branch root.

---

## License

MIT
