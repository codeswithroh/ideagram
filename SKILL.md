---
name: ideagram
description: Turn a concept, feature description, blog post, or pitch into a single beautiful, on-brand illustration — either by GENERATING an original modern-flat editorial illustration with an image model (steered by a rigorous style system), or by recoloring a real unDraw illustration to the brand. Use whenever the user asks to "make an illustration," "create a graphic," "visualize this concept," "explain this visually," wants something "unDraw-style" or "Storyset-style," needs a hero/feature/blog illustration, or says an idea needs a picture. Trigger even if they don't name a style — "make something to explain X" or "I need a graphic for this tweet" both qualify.
---

# Ideagram

## Two ways to make an illustration — pick the right one

**Path A — Generate (the ceiling).** Create an original illustration with an image-generation tool, steered by a rigorous style system (Style DNA + composition patterns + a fresh-metaphor recipe + a prompt template + a QA pass). This is how you get a *beautiful, custom* illustration for any concept — not limited to a library. It's the higher-quality path and the one to reach for when the user wants something bespoke and premium. **Requires an image-generation tool** (a built-in image generator, or an image MCP). If none is available in the session, you can't run this path — fall back to Path B and say so.

**Path B — Recolor real unDraw (reliable, no image tool needed).** Match the concept to a real unDraw illustration in the user's local library and recolor it to the brand accent. Genuine illustrator quality (it *is* real unDraw art), exact and reliable, but bounded by what's in the library. This is the default when there's a good library match, or whenever no image tool is available.

**How to choose:** if an image tool is available and the user wants something custom/premium, or the library has no good match → **Path A**. If there's a strong library match, or no image tool exists → **Path B**. When unsure, say which you're using and why.

---

## Path A — Generate an original illustration

### A1. Digest the brief → one idea
Reduce the input to one sentence ("this is fundamentally about ___"). One image = one idea. If the brief holds two ideas, that's two images. For a longer piece, propose a short shot list (2–8 images, each with its own idea) before generating.

### A2. Design the image (this is where quality is won — don't skip to the prompt)
Work through `references/composition-patterns.md`:
- Pick **one structure** (hero scene / before-after / character state / concept metaphor / workflow / layers / journey).
- Invent a **fresh metaphor** with the 3-step recipe (abstract concept → physical action → low-key object → character performs it). Avoid the first cliché.
- Design it as a **scene with depth** (background → midground object → foreground figure → grounding), not a subject floating on white.
- Apply the **"earn its place" test**: if removing the character still leaves the metaphor intact, they're decoration — rewrite so they perform the core action.

### A3. Write the prompt
Fill `references/prompt-template.md` with the decisions above and the brand accent hex, keeping the whole Style DNA (`references/style-dna.md`) in the prompt — including the explicit anti-slop "never" list. Generate **one image per call**.

### A4. QA and iterate
Run `references/qa-checklist.md`. If it fails (busy, off-palette, decorative character, broken hands, baked-in gibberish text), regenerate or image-edit using the iteration prompts in `prompt-template.md` — don't ship a flawed image.

### A5. Deliver
Save to `design/`; name a set in order (`01-...`, `02-...`). Report which images are strongest vs. optional, and that they were generated (Path A).

> Honesty: this path needs an image-generation tool. If the session has none, say plainly "no image tool here — using the recolor path instead," and go to Path B.

---

## Path B — Recolor a real unDraw illustration

Reads real unDraw SVGs from the user's **local library** (`~/.ideagram/undraw/`, populated free from [undraw.co/illustrations](https://undraw.co/illustrations)) — it does not bundle or redistribute unDraw's files. Read `references/undraw-anatomy.md` once for how these files work.

1. **Distill** the concept to keywords.
2. **Match** to the best-fitting library SVG (filenames are descriptive; prefer one whose whole scene matches).
3. **Recolor** to the brand accent: `python3 scripts/recolor_undraw.py <matched>.svg --accent "#<brand>" --out design/<name>.svg` — swaps only unDraw's `#6c63ff` accent, preserving skin/hair/clothing/neutrals. Add `--also "#8ed16f:#<hex>"` for a secondary accent.
4. **Compose** (only if no single illustration fits): extract whole components with `scripts/extract_component.py` and assemble a real scene per `references/composition-patterns.md`. Reuse *whole* components; recombining sub-limbs is fragile.
5. **Validate & deliver:** `scripts/validate_assets.py <final>.svg`; deliver the SVG (crisp, recolorable), or PNG via `scripts/export_png.py`.

If the library has no usable match and there's no image tool for Path A, say so and offer to let the user grab a fitting illustration from undraw.co.

---

## Reference files

| File | Path | Read when |
|---|---|---|
| `references/style-dna.md` | A | The visual contract for every generated image — read before writing any prompt |
| `references/composition-patterns.md` | A | Designing the image: structure + fresh-metaphor recipe + scene-building |
| `references/prompt-template.md` | A | Writing the actual generation prompt + iteration/edit prompts |
| `references/qa-checklist.md` | A | Checking every generated image before delivery |
| `references/undraw-anatomy.md` | B | How real unDraw files are built + the reuse technique |
| `references/style-contract.md`, `references/metaphor-library.md` | — | Legacy no-image, no-library fallback (generated primitives). Lowest quality; last resort only. |

## Scripts

| Script | Purpose |
|---|---|
| `scripts/recolor_undraw.py` | Recolor a real unDraw illustration's accent to a brand color (Path B workhorse). |
| `scripts/extract_component.py` | Lift a whole figure/device/panel from a source unDraw SVG for composing a scene. |
| `scripts/validate_assets.py` | Verify an SVG is well-formed before delivering. |
| `scripts/export_png.py` | Flatten an SVG to PNG at standard sizes (needs cairosvg + system cairo). |

## Honesty

Always say which path produced the result (generated vs. recolored real unDraw vs. primitive fallback), and never present a flawed image (broken hands, gibberish text, off-brand color) as finished — fix it or flag it. Respect the sourcing model: reuse real unDraw art from the user's local library; don't redistribute unDraw's files into a public repo.
