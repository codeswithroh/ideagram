---
name: ideagram
description: Turn a concept, feature description, blog post, or pitch into a single clean, shareable explanatory illustration in an unDraw-like flat style — ready to post on social media, drop into a landing page, or show someone to explain an idea instantly, without needing design skill or a separate design tool. Use this whenever the user asks to "make an illustration," "create a graphic," "visualize this concept," "explain this visually," wants something "unDraw-style," "Storyset-style," or a simple flat illustration for a feature/blog/announcement, or says an idea is hard to explain in words and needs a picture instead. Trigger even if they don't name a style — "make something to explain X" or "I need a graphic for this tweet about Y" both qualify.
---

# Ideagram

## What this is, and isn't

Given a piece of content — a feature description, a paragraph, a pitch — produce **one** clean, flat, two-color SVG illustration that explains the single core idea at a glance. Optimized for: posting on social media, embedding on a landing page, or showing someone cold with zero verbal explanation needed.

This is not a photorealistic image generator and not a general-purpose art tool. It's specifically about the "explain one idea, instantly, simply" register that sites like unDraw popularized — restraint is the entire point. If the request calls for something else (photoreal, highly detailed, brand-specific illustrated characters with a distinct hand-drawn style), say so rather than forcing this system to do it badly.

## Why this is built the way it is

Two things reliably go wrong when an LLM is asked to "draw an unDraw-style illustration" without a system behind it:

1. **It tries to depict too much.** Given a paragraph of content, the instinct is to cram in every detail mentioned. The actual unDraw/Storyset register works because it depicts exactly one idea, with one clear focal point — see `references/metaphor-library.md` for how to pick that one idea deliberately instead of by instinct.
2. **Freehand human figures go wrong.** Hand-authoring SVG path data for a person from scratch, per request, is where proportions get mangled — a limb detached from a torso, a head the wrong size. `assets/primitives/` bundles a small library of pre-built, already-verified figures and props (rendered and checked in a browser, not just written blind) — compose scenes from these instead of drawing new figures from raw paths each time.

## Workflow

### Step 1 — Distill the content to one concrete metaphor

Read `references/metaphor-library.md`. Reduce the input to one sentence ("this is fundamentally about ___"), then map that to one concrete, recognizable object or scene — not an attempt to depict the whole input. If the content genuinely contains two unrelated ideas, that's two illustrations, not one crowded scene; say so rather than cramming both in.

### Step 2 — Compose from primitives, don't freehand a new figure

Check `assets/primitives/` for an existing figure/prop that fits the chosen metaphor. Compose by placing primitives in a shared canvas using `<g transform="translate(x y)">` wrappers around each primitive's contents, not by editing their internal path coordinates — this keeps every primitive independently reusable for the next illustration.

If nothing in the library fits, build new shapes following `references/style-contract.md` exactly (two colors, flat fills, rounded human-made objects, circles/capsules for organic shapes) rather than freehand human anatomy — and consider adding a genuinely reusable new primitive to the library if this concept is likely to come up again.

### Step 3 — Apply the style contract

Read `references/style-contract.md` before finalizing. The short version: exactly two colors (one near-black base, one accent — default `#262631` / `#6C5CE7`, but the accent should become the caller's brand color when they have one), no gradients or shadows, one clear focal point carrying the accent.

If the user has a brand/accent color, use `scripts/recolor_svg.py --accent "#hex" --preserve-dark` on the composed SVG rather than hand-editing color values — it's already handled correctly, including leaving the base/outline color untouched.

### Step 4 — Validate before delivering

Run `scripts/validate_assets.py` against the finished SVG. This catches malformed XML that reads fine as text but renders as a broken image in strict browsers (the most common case: a `--` inside a `<!-- -->` comment) — invisible unless actually parsed, so don't skip it.

### Step 5 — Deliver in the format the use case actually needs

- **Web/landing page**: the SVG file directly — smallest, crispest, infinitely recolorable.
- **Social media / anywhere raster is required**: `scripts/export_png.py illustration.svg --out design/export/` renders flattened PNGs at standard social/presentation dimensions (1080×1080 square, 1200×630 landscape/OG-card, 1920×1080 presentation), centered without distorting the aspect ratio. Requires `cairosvg` (`pip install cairosvg`) *and* the system cairo library (`brew install cairo` on macOS, `sudo apt install libcairo2` on Debian/Ubuntu) — if either is missing, the script says so plainly rather than failing silently, and the SVG remains fully usable on its own in the meantime (most platforms and every browser accept SVG natively).

## Reference files

| File | Read when |
|---|---|
| `references/metaphor-library.md` | Step 1 — choosing what to actually depict |
| `references/style-contract.md` | Step 3 — applying color/shape/composition rules |

## Scripts

| Script | Purpose |
|---|---|
| `scripts/recolor_svg.py` | Recolor a composed illustration's accent color to a brand hex, preserving the base/outline. |
| `scripts/validate_assets.py` | Verify the finished SVG is well-formed before delivering it. |
| `scripts/export_png.py` | Export flattened PNGs at standard social/presentation sizes (needs cairosvg + system cairo). |

## Assets

`assets/primitives/` — the starter figure/prop library (2 figures, 7 props; see `references/metaphor-library.md` for the full mapping). This is meant to grow: if a concept keeps needing a new object that doesn't exist yet, build it once following the style contract and add it here rather than redrawing it from scratch on every future request.

## Honesty

If the requested concept doesn't map cleanly to anything in the metaphor library and the resulting illustration is a rougher first attempt at a new visual, say so rather than presenting it as equally polished as the pre-built primitives. And don't claim PNG export happened if cairo wasn't available and the deliverable is SVG-only — say what format was actually produced.
