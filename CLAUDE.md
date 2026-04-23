# Pinna's Intertwining Illusion — Parameter Tuner

## Overview

A self-contained interactive tool for exploring and tuning Pinna's Intertwining Illusion — an optical illusion where concentric circles appear to spiral and intertwine. Discovered by Baingio Pinna (University of Sassari, Italy).

## File

- **pinna_tuner_3.html** — single-file app; open directly in any browser, no build step.

## Architecture

Vanilla JS + Canvas 2D API. No dependencies, no build step. All logic, styles, and markup in one file.

**Key sections:**
- CSS — dark theme via `:root` design tokens, responsive mobile layout via `@media (max-width: 639px)`, `vh` fallback before `dvh`
- HTML — two-tab layout: `#tuner-page` (controls + canvas) and `#about-page`
- JS — drawing engine, animation loop, presets (built-in + user), SVG export

## Code Structure (JS)

### Constants
Top-of-file named constants: `CANVAS_PADDING`, `SVG_EXPORT_SIZE`, `SQUARE_SPACING_FACTOR`, `MIN_SQUARES_PER_RING`, `MIN_STROKE_WIDTH`.

### Cached DOM
`$()` helper caches `getElementById`. Key elements (`canvas`, `ctx`, `wrap`, buttons) are resolved once at load.

### Geometry Pipeline
Three functions form a shared pipeline used by both canvas drawing and SVG export:

1. `readParams()` — reads all control values into a plain object
2. `computeRings(p)` — returns `[{ r, n }]` array from params
3. `forEachSquare(p, rings, cx, cy, fn)` — iterates every square, calling `fn(x, y, rotRad, color, fill)`

`draw()` and `exportSVG()` both call this pipeline — no duplicated geometry logic.

### Animation
`animLoop(ts)` drives rotation via `requestAnimationFrame`. A `pendingResize` flag ensures canvas dimensions update on the next frame if the window resizes mid-animation.

### Presets
- `BUILTIN_PRESETS` — hardcoded, always shown first, no delete button
- User presets — stored in `localStorage` (key: `pinna_presets`), shown below built-ins with delete
- `addPresetItem()` — shared renderer for both types

## Built-in Presets

| Name | Description |
|------|-------------|
| Small Three B&W | 3 rings, small filled squares, warm background, tilt 74 |

## Controls & Parameters

| ID | Range | Default | Effect |
|----|-------|---------|--------|
| `rings` | 2–10 | 5 | Number of concentric rings |
| `r0` | 30–120px | 55 | Innermost ring radius |
| `spacing` | 30–100px | 70 | Distance between rings |
| `sq` | 6–30px | 19 | Square size |
| `tilt` | 5–85 deg | 76 | Tilt angle of squares (drives the illusion) |
| `density` | 0.5–1.5x | 1.2 | Square packing density |
| `sw` | 0.5–5px | 1 | Stroke width |
| `bg` | color | #8878a0 | Background color |
| `dark` | color | #1a1a1a | Dark square color |
| `light` | color | #e8e8e8 | Light square color |
| `darkFilled` | checkbox | false | Fill dark squares |
| `lightFilled` | checkbox | false | Fill light squares |
| `rotspeed` | 5–180 deg/s | 5 | Animation rotation speed |

## Features

- **+/- buttons** on every slider for fine-grained single-step increments
- **Animate** — rotate the illusion; stop to observe the motion aftereffect
- **Presets** — built-in + user-saved parameter sets via `localStorage`
- **SVG export** — exports current view as a static 800x800 SVG; blob URL revoked after download

## Illusion Mechanics (from About tab)

The illusion arises from two interactions:
1. Squares alternate dark/light within each ring
2. Tilt direction reverses between adjacent rings

This aligns centre-surround enhancement fringes to mimic the visual signature of a spiral. Strongest at tilt near 45 deg or 75 deg.

## Development Notes

- CSS uses `:root` custom properties for all theme colors (prefix `--c-`)
- Canvas resizes to fit `#canvas-wrap`; `pendingResize` flag handles resize during animation
- `ResizeObserver` on `#canvas-wrap` triggers redraws on layout change
- SVG export size is `SVG_EXPORT_SIZE` constant (800), independent of canvas display size
- MIT Licensed — Mark Safire, 2026
