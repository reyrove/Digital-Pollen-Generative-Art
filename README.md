# Digital Pollen

**A seed-based generative system for noise-driven organic textures.**

A catalogue of computational textile compositions for fashion, textile and surface design — algorithmically drawn, seed-documented, and ready for production.

---

## Overview

Digital Pollen is a generative design system rather than a single artwork. Each composition is built from a grid of potential anchors, each one a starting point for a short organic curve. The curve's direction is driven by three layers of value noise at different frequencies — the result reads as pollen, scattering across a dark, quiet field.

The system is designed for:

- **Fashion houses** adapting organic texture for apparel and accessories
- **Textile studios** developing repeat patterns and yardage
- **Surface designers** working across print, wallpaper, and interior applications

Every composition can be licensed, adapted, or commissioned to a brief.

---

## Concept

A curve, when it is *driven by noise*, becomes a field of pollen — organic, quiet, and yours.

The organic curve — irregular, harmonic, endlessly variable — has always carried the mark of living systems. From the veins of a leaf to the flight of a bee, natural motion is never straight, always soft. Digital Pollen translates that motion into code. Each composition begins with a scatter of anchors and unfolds through layered value noise, producing hundreds of short strokes that together read as a texture — as pollen on a dark surface, as a quiet organic field.

The palette, the noise frequencies, the curve geometry, and the grid density are all derived from a single numeric seed.

Like the other still volumes in this series (Girih, Arachne, Celestial Grove, ChaotiColor, Citrus Mosaic, Crazy Knight Curve, Crazy Knight Line, Crazy Letter, cyPollock), **Digital Pollen is a static composition.** The plate, the framed plate, the surfaces, and the archive are all static frames. A pollen field is something you read; its character is stillness, not motion.

---

## Features

- **Seed-based generation** — every composition is defined by a numeric seed and can be regenerated exactly
- **Deterministic output** — the same seed always produces the same composition
- **Noise-driven curves** — three layers of value noise at different frequencies guide every step
- **Eight base palettes** — from blues and violets, through reds and greens, to warm golds
- **Goth mode** — one of the eight palettes triggers a desaturated, moody variant
- **Layered glow** — every curve carries a soft shadow blur that ties the field together
- **Nested frame** — three decorative rectangles wrap the composition, echoing the concentric logic
- **Adaptive grid** — roughly 50 anchors per axis, culled to leave a sparse scatter
- **Adaptive surfaces** — one seed applied across print, scarf, textile, and wall formats
- **Archive** — eight curated seeds available for immediate loading
- **Download** — export the composition as a high-resolution PNG
- **Keyboard shortcuts** — `R` for new seed, `S` to save

---

## Project Structure

```
.
├── index.html          # Main catalogue page
├── images/
│   ├── fav.svg         # Favicon
│   ├── tote.png        # Mockup: tote bag
│   ├── tee.png         # Mockup: t-shirt
│   └── cushion.png     # Mockup: cushion
└── README.md
```

---

## How It Works

### The Seed

A numeric seed (a large integer) initializes a deterministic pseudo-random generator. From this seed, the system derives:

- Base palette (from a set of 8 hue families)
- Three noise frequencies (f1, f2, f3)
- Three noise phases (φ1, φ2, φ3)
- An angle multiplier (2 through 6)
- A 16 + 8 = 24-colour palette, split between the base hue and its complement
- Whether the composition runs in **Goth mode** (desaturated, muted) or **Vibrant mode** (saturated)

Because the generator is deterministic, the same seed always produces the same composition — on any device, at any time.

### The Noise

The system uses three-layer value noise — a 3D noise field sampled at three different frequencies:

- **Frequency 1** (f1) — the medium-frequency layer, driving the primary direction change
- **Frequency 2** (f2) — a second medium-frequency layer, adding secondary wobble
- **Frequency 3** (f3) — the low-frequency layer, adding broad, slow drift

At every step of every curve, the direction is computed as:

```
angle = noise3D(cx · f1, cy · f1, φ1) · 2π · multiplier
      + noise3D(cx · f2, cy · f2, φ2) · 2π
      + noise3D(cx · f3, cy · f3, φ3) · π
```

The `angleMultiplier` (2 through 6) controls how chaotic the curves become: a value of 2 produces gentle, near-circular strokes; a value of 6 produces wild, spiralling curves.

The result: hundreds of short, organic strokes that never travel straight, always drift, and always feel alive.

### The Grid

Anchors are placed on a regular grid — one anchor roughly every 2% of the canvas. However, only about 30% of them are actually drawn:

```
if (random() > 0.3) continue;
```

This leaves the composition sparse, breathing, and organic. A denser grid would read as wallpaper; a sparser one would read as a scattered drawing.

### The Palette

The 24-colour palette is built from two halves:

- **16 base hues** — variations around the seed's primary hue, spanning roughly ±80° of the colour wheel
- **8 complement hues** — variations around the hue + 180°, providing visual contrast

Each colour is stored in HSL space with an alpha value, so the whole field has soft transparency and layering.

**Goth mode** — when the seed selects palette index 4, the composition shifts into Goth mode: saturation and lightness both drop, producing muted, desaturated, moody compositions.

### The Frame

Three nested rectangles are drawn around the field, at 5%, 6%, and 7% offset from the edges. Each is stroked in a colour from the palette, at increasing line width. They form a quiet border — concentric like a print plate.

### The Surfaces

The same seed is rendered across four surface formats. These are static frames — they represent the print-ready composition.

| Surface  | Aspect | Material          |
|----------|--------|-------------------|
| Print    | 1 : 1  | Cotton rag        |
| Scarf    | 3 : 1  | Twill silk        |
| Textile  | 4 : 3  | Fabric yardage    |
| Wall     | 2 : 3  | Wallpaper         |

Each surface uses the same underlying seed and structural logic — only the repeat, orientation, and scale change.

### Stillness

Like the rest of the still volumes, Digital Pollen does not animate. The plate is a single frozen frame — the composition is complete the moment it is generated.

This is a deliberate design choice. A pollen field is not a swarm. It is not a rotation. It is a texture, drawn once and left. Its stillness is what makes it print-ready in the strictest sense: what you see is what you get.

---

## Usage

### In the browser

1. Open `index.html` in any modern browser.
2. Click **New Seed** to generate a new composition.
3. Click **Download** to save the composition as a PNG.
4. Scroll to the **Archive** section and click any plate to load it into Plate 001.

### Keyboard shortcuts

| Key | Action          |
|-----|-----------------|
| `R` | New seed        |
| `S` | Save as PNG     |

### Reproducing a composition

Each composition is identified by an 8-digit seed label displayed in the metadata panel. To reproduce a specific composition, note the seed and regenerate it programmatically:

```js
const rng = new RandomGenerator(seed);
const features = buildFeatures(rng);
renderComposition(canvas, features, rng);
```

Because the generator is deterministic, this will produce the identical composition on any device.

---

## Technical Notes

- **No build step.** The system is a single HTML file with inline CSS and JavaScript.
- **No dependencies.** All drawing is done with the native Canvas 2D API. No p5.js, no external noise library.
- **Deterministic.** The `RandomGenerator` class uses a xorshift-based PRNG seeded by an integer, so identical seeds produce identical outputs.
- **Deterministic noise.** The 3D value noise function uses an integer hash — no `Math.random()` in the noise itself — so the same seed always produces the same noise field. This is essential for reproducibility.
- **Static rendering.** Every canvas renders a single frame. There is no animation loop.
- **Feature isolation.** Cover, framed plate, surfaces, and archive thumbnails each derive their own feature set from their own local RNG, without disturbing the main plate's state.
- **Pollen scatter.** Roughly 70% of grid anchors are culled, so the composition reads as scatter rather than grid.
- **Adaptive curve geometry.** Curve length, step size, and maximum deviation all scale with the canvas size, so the composition reads consistently at any resolution.
- **Goth mode.** One of the eight base palettes shifts the composition into a desaturated variant; the metadata panel reports "Goth" or "Vibrant" accordingly.
- **Responsive.** The layout adapts from large desktop down to very small mobile devices (tested at 360px viewport width).
- **Accessible.** Supports `prefers-reduced-motion`. Pinch-zoom is enabled.

### Browser support

Tested in current versions of:

- Chrome / Edge
- Firefox
- Safari (desktop and iOS)

---

## Licensing

All Digital Pollen compositions are **seed-documented** and available for licensing across textile, surface, and print applications.

- **Standard licenses** cover single-product production runs.
- **Commercial use, custom editions, or exclusive rights** are available on request.

Each license is issued against a specific seed ID. Regeneration of the same seed produces the identical composition — ensuring reproducibility between artist, studio, and manufacturer.

For licensing enquiries: [reyhanehdaneshdoost@gmail.com](mailto:reyhanehdaneshdoost@gmail.com)

---

## Commission

Digital Pollen is a generative design system, not a fixed artwork. It can be adapted for specific briefs:

| Service     | Description                                                       |
|-------------|-------------------------------------------------------------------|
| Licensing   | Existing seeds from the archive, licensed for production use      |
| Commission  | New compositions designed to your palette, repeat, and product    |
| Systems     | A private generative tool built for your studio's ongoing use     |

To begin a conversation: [reyhanehdaneshdoost@gmail.com](mailto:reyhanehdaneshdoost@gmail.com)

---

## Series

Digital Pollen is part of a computational textile series. Each volume approaches ornament from a different structural angle:

| Volume                | Structure                    | Motion                     |
|-----------------------|------------------------------|----------------------------|
| Girih 1               | Islamic geometric            | Static                     |
| Arachne               | Rotating rings               | Static                     |
| Baroque Me Baby       | Baroque frames               | Static                     |
| Bezier 1              | Concentric curves            | Static                     |
| Bezier 2              | Single rotating curve        | Animated (plate)           |
| Brownian Graphe       | Graph networks               | Animated + interactive     |
| Celestial Grove       | Recursive branch trees       | Static                     |
| ChaotiColor           | Cellular automata            | Static                     |
| Citrus Mosaic         | Arc-and-triangle tiles       | Static                     |
| Crazy Knight Curve    | Knight's-tour smooth path    | Static                     |
| Crazy Knight Line     | Knight's-tour gradient       | Static                     |
| Crazy Letter          | Framed wavy lines            | Static                     |
| cyPollock             | Scattered branch field       | Static                     |
| **Digital Pollen**    | **Noise-driven texture**     | **Static**                 |

The series is designed as a coherent whole — same page structure, same seed logic, same licensing and commission terms — so that each volume can be presented individually or as part of a larger body of work.

---

## Credits

- **Design & Generative System** — Reyhaneh Daneshdoost
- **Typefaces** — Cormorant Garamond · DM Mono
- **Platform** — Reyrove Studio
- **Edition** — Digital Pollen, Autumn 2026

### On AI tools

Where technical obstacles were encountered, AI tools were used for debugging and code optimization. Every structural, aesthetic, and conceptual decision remained the artist's own.

---

## Links

- Website — [reyrove.github.io](https://reyrove.github.io/)
- Instagram — [@rey._.rove](https://www.instagram.com/rey._.rove/)
- LinkedIn — [Reyhaneh Daneshdoost](https://www.linkedin.com/in/reyhaneh-daneshdoost-730481160/)
- X — [@reyrove](https://x.com/reyrove)

---

© Digital Pollen · All compositions reproducible by seed · Computational Textile Design