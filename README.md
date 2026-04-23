# Pinna's Intertwining Illusion — Parameter Tuner

An interactive browser tool for exploring and tuning **Pinna's Intertwining Illusion** — an optical illusion where concentric rings of tilted squares appear to spiral and intertwine. Discovered by Baingio Pinna (University of Sassari, Italy).

[**Try it live →**](https://uxrrr.github.io/pinna-interlocking/)

## What is the illusion?

Concentric circles of small squares look like interlocked spirals even though every circle is perfectly round. The effect arises from two interactions:

1. Squares alternate dark/light within each ring.
2. Tilt direction reverses between adjacent rings.

This aligns centre-surround enhancement fringes to mimic the visual signature of a spiral. The effect is strongest at tilt angles near 45° or 75°. Stopping the animation after watching it rotate produces a striking motion aftereffect.

## Features

- **Real-time parameter tuning** — tweak rings, radius, spacing, square size, tilt, density, stroke, colors, and fill
- **+/– step buttons** on every slider for fine-grained control
- **Animation** — rotate the illusion at adjustable speed; stop to observe the motion aftereffect
- **Ouchi illusion tab** — a second interactive illusion (the Ouchi checkerboard)
- **Presets** — built-in examples plus user-saved presets via `localStorage`
- **SVG export** — download the current view as a crisp 800×800 SVG suitable for printing

## Usage

No install, no build step. Just open `index.html` in any modern browser.

```
git clone https://github.com/<your-username>/pinna-interlocking.git
open pinna-interlocking/index.html
```

## Parameters

| Control | Range | Default | Effect |
|---------|-------|---------|--------|
| Rings | 2–10 | 5 | Number of concentric rings |
| Inner radius | 30–120 px | 55 | Radius of the innermost ring |
| Spacing | 30–100 px | 70 | Distance between rings |
| Square size | 6–30 px | 19 | Size of each square |
| Tilt | 5–85° | 76 | Tilt angle — drives the illusion |
| Density | 0.5–1.5× | 1.2 | Square packing density |
| Stroke width | 0.5–5 px | 1 | Square outline thickness |
| Background | color | #8878a0 | Canvas background |
| Dark color | color | #1a1a1a | Dark square color |
| Light color | color | #e8e8e8 | Light square color |
| Dark filled | checkbox | off | Fill dark squares |
| Light filled | checkbox | off | Fill light squares |
| Rotation speed | 5–180°/s | 5 | Animation speed |

## Architecture

Single-file vanilla JS + Canvas 2D — no frameworks, no build tooling, no dependencies.

**Geometry pipeline** (shared by canvas drawing and SVG export):
1. `readParams()` — reads control values into a plain object
2. `computeRings(p)` — computes `[{ r, n }]` ring data from params
3. `forEachSquare(p, rings, cx, cy, fn)` — iterates every square, calling `fn(x, y, rotRad, color, fill)`

## License

MIT — Copyright (c) 2026 Mark Safire
