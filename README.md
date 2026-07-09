# Ideagram

A Claude Code / Cursor / Windsurf skill that turns a concept, feature description, or pitch into one clean, shareable, unDraw-style flat illustration — ready to post on social media, drop into a landing page, or show someone cold to explain an idea instantly.

Not affiliated with unDraw. This produces original artwork generated fresh per request in a similar flat, two-color, minimal-detail visual register — it does not use, redistribute, or derive from unDraw's actual illustration files.

## Why this works better than asking an LLM to "draw an unDraw-style illustration" cold

Two things reliably go wrong without a system behind it:

1. **It tries to depict everything in the input.** The unDraw/Storyset register works because each illustration commits to exactly one idea with one clear focal point. `references/metaphor-library.md` exists specifically to force that reduction — content → one sentence → one concrete object — instead of cramming a whole paragraph into one crowded scene.
2. **Freehand SVG human figures go wrong.** Hand-drawing a person's path data from scratch per request is where anatomy gets mangled. `assets/primitives/` bundles a small library of figures and props that are already built and verified (rendered and checked in a browser, not just written blind) — new illustrations compose from these instead of redrawing people every time.

## Install

Drop the `ideagram/` folder into your project's skills directory (Claude Code, Cursor, and Windsurf all support this). No API keys, no hosted service — everything runs locally.

**Optional, for raster export** (PNG for platforms that don't accept SVG): `pip install cairosvg` **and** the system cairo library (`brew install cairo` on macOS, `sudo apt install libcairo2` on Debian/Ubuntu — cairosvg's Python package alone isn't enough, a very common gotcha). Without it, the SVG itself is still fully usable directly — most modern platforms and every browser accept SVG natively.

## Structure

```
ideagram/
├── SKILL.md                          — the workflow, read this first
├── references/
│   ├── style-contract.md             — the two-color, flat-fill visual grammar every illustration follows
│   └── metaphor-library.md           — concept → concrete visual metaphor lookup
├── scripts/
│   ├── validate_assets.py            — SVG well-formedness validation
│   ├── recolor_svg.py                — recolor a composed illustration to a brand accent color
│   └── export_png.py                 — flatten to PNG at standard social/presentation sizes
└── assets/primitives/                — starter figure/prop library (2 figures, 7 props), composable via <g transform>
```

## What this style is, honestly

Clean, simple, geometric flat illustration — closer to a flat icon/illustration hybrid than to unDraw's own more detailed, organically-posed artwork. It composes and recolors reliably, which is the point, but it's not a pixel-for-pixel match to any specific illustrator's style. See `references/style-contract.md` for the full grammar this is built on.

## License

MIT for the skill, scripts, and bundled primitive illustrations in this repo (all original artwork). Not affiliated with, and does not redistribute assets from, unDraw.co.
