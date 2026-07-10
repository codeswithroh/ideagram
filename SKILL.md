---
name: ideagram
description: Turn a concept, feature description, blog post, or pitch into a single beautiful, on-brand unDraw-style illustration — by matching it to a real unDraw illustration and recoloring it to the brand accent, so the output has genuine illustrator quality, not a hand-drawn approximation. Use whenever the user asks to "make an illustration," "create a graphic," "visualize this concept," "explain this visually," wants something "unDraw-style" or "Storyset-style," needs a hero/feature/blog illustration, or says an idea needs a picture. Trigger even if they don't name a style — "make something to explain X" or "I need a graphic for this tweet" both qualify.
---

# Ideagram

## The core idea

Produce **one** beautiful unDraw-style illustration for a concept, on-brand. The key realization behind this skill: an LLM cannot hand-draw illustration at unDraw's quality — those figures are dozens of hand-placed bezier anchors drawn by a professional illustrator, and freehand-generating path coordinates produces crude pictograms, not art. The quality is *in the real path data*.

So this skill does **not draw**. It **reuses real unDraw illustrations** — matching a concept to the right one and recoloring it to the brand accent (and, when needed, composing whole real components into a new scene). The output is genuinely illustrator-grade because it *is* the illustrator's work, just made on-brand. Read `references/undraw-anatomy.md` once — it's the dissection of how these files are built and why this approach works.

## Setup: a local unDraw library

This skill reads real unDraw SVGs from a **local library folder** the user maintains — it does not bundle or redistribute unDraw's files (their license bars compiling their assets into a redistributed collection; downloading them for your own use is free and needs no attribution).

- Default library location: `~/.ideagram/undraw/` — create it and drop `.svg` files there.
- The user populates it by downloading illustrations from [undraw.co/illustrations](https://undraw.co/illustrations) (free, no attribution, unlimited). A folder like `~/Downloads/undraw` also works — just point the workflow at wherever the SVGs are.
- If **no library exists**, tell the user plainly: the best output needs a few real unDraw SVGs in the library, and point them at undraw.co to grab some (30 seconds). Only fall back to the crude generated-primitive path (`references/style-contract.md`) if they can't, and be honest that it's a large quality drop.

## Workflow

### Step 1 — Distill the concept

Reduce the input to one sentence — "this is fundamentally about ___" — and a few keywords (e.g. "team collaboration," "security," "analytics dashboard," "mobile app"). This is what you'll match against the library. If the content is really two ideas, that's two illustrations.

### Step 2 — Match to the best illustration in the library

List the library's SVGs (their filenames are descriptive, e.g. `undraw_tech-keynote_ytf3.svg`, `undraw_content-team_1p7b.svg`). Pick the one whose subject best fits the concept's keywords. If several fit, prefer the one whose *scene* matches (a dashboard concept → an illustration with a screen/chart; a teamwork concept → one with multiple figures). If nothing in the library is close, say so and either ask the user to grab a fitting one from undraw.co or fall back (Step 5).

### Step 3 — Recolor to the brand

If the user has a brand/accent color, run:

```
python3 scripts/recolor_undraw.py <matched>.svg --accent "#<brand>" --out design/<name>.svg
```

This swaps only unDraw's `#6c63ff` accent family for the brand color, leaving every skin tone, hair, clothing, and neutral exactly as drawn — verified on real multi-figure scenes. If the illustration uses a secondary accent (green `#8ed16f`, pink `#ff6584`) that should also change, add `--also "#8ed16f:#<hex>"`. If the user has no brand color, skip this — unDraw's purple is a fine default.

### Step 4 — (Optional) Compose a custom scene from real components

Only when no single library illustration fits and you need to build one:

- Extract whole components (a figure, a device, a background panel) from source illustrations with `scripts/extract_component.py source.svg --list` then `--match "translate(...)" --out part.svg` (see `references/undraw-anatomy.md`).
- Assemble them into one `<svg>` as a real **scene**, not a floating figure: a background element (panel/screen), the concept's midground device, then the figure interacting with it, plus a ground shadow. This scene-building is what the old primitive approach missed.
- Place each component with a `<g transform="translate() scale()">`; render and use `getBBox()` to size/position accurately. Reuse *whole* components — recombining sub-limbs is fragile (see anatomy doc).
- Recolor the finished composition (Step 3).

### Step 5 — Fallback only if there's no usable library

If the user genuinely has no unDraw library and won't create one, the generated-primitive path (`references/style-contract.md` + `assets/primitives/`) still exists — but it produces simple flat pictograms, not illustrator-grade art. Use it only as a last resort and say plainly that grabbing a couple of real unDraw SVGs would look dramatically better.

### Step 6 — Validate and deliver

- `python3 scripts/validate_assets.py <final>.svg` — catches malformed SVG (e.g. a `--` inside a comment) that renders as a broken image.
- Deliver the SVG directly for web (crisp, tiny, recolorable). For social/raster, `scripts/export_png.py <final>.svg --out design/export/` renders standard sizes (needs cairosvg + system cairo; degrades honestly if absent — the SVG works everywhere regardless).

## Reference files

| File | Read when |
|---|---|
| `references/undraw-anatomy.md` | Always, first — the palette, structure, and the reuse technique this skill is built on |
| `references/style-contract.md` | Only for the Step 5 fallback (generated primitives, no library) |
| `references/metaphor-library.md` | Only for the Step 5 fallback — concept → simple shape mapping |

## Scripts

| Script | Purpose |
|---|---|
| `scripts/recolor_undraw.py` | Recolor a real unDraw illustration's accent to a brand color, preserving skin/ink/clothing/neutrals. The workhorse. |
| `scripts/extract_component.py` | Lift a whole figure/device/panel out of a source unDraw SVG for composing a new scene. |
| `scripts/validate_assets.py` | Verify the finished SVG is well-formed before delivering. |
| `scripts/export_png.py` | Flatten to PNG at standard social/presentation sizes (needs cairosvg + system cairo). |

## Honesty

Say what actually happened. If you recolored a real unDraw illustration, that's the honest and best case — say so. If you fell back to generated primitives because there was no library, say that plainly and note the quality gap. Never present a hand-drawn pictogram as if it were unDraw-quality. And respect the sourcing model: reuse real unDraw art from the user's local library for their own sites; don't redistribute unDraw's files into a public repo.
