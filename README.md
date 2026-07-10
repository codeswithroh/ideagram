# Ideagram

A Claude Code / Cursor / Windsurf skill that turns a concept into a **beautiful, on-brand illustration** — either by **generating** an original modern-flat editorial illustration with an image model (steered by a rigorous style system), or by **recoloring** a real unDraw illustration to your brand.

## Two paths

**Path A — Generate (the ceiling).** With an image-generation tool available, ideagram creates an original illustration for *any* concept, steered by a full style system: a Style DNA (with an explicit anti-slop "never" list), composition patterns, a fresh-metaphor recipe, a prompt template, and a QA pass. This is the higher-quality, unbounded path — a custom, premium, on-brand image for whatever the brief is. It requires an image tool; without one, use Path B.

**Path B — Recolor real unDraw (reliable, no image tool).** Match a concept to a real unDraw illustration in your local library and recolor only its purple accent to your brand, leaving skin/hair/clothing exactly as the illustrator drew them. Genuine illustrator quality, exact and reliable, but bounded by what's in the library.

The generation path's craft was learned from studying the excellent [ian-xiaohei-illustrations](https://github.com/helloianneo/ian-xiaohei-illustrations) skill, whose quality comes from a rigorous style system driving an image model — not from drawing SVG. Ideagram applies that method to a clean modern-editorial aesthetic.

## How sourcing works (and the licensing)

unDraw illustrations are free to use commercially with no attribution — but their license bars *compiling their assets into a redistributed collection*. So this skill **does not bundle unDraw's files.** Instead it reads them from a local library you maintain:

- Create `~/.ideagram/undraw/` and drop unDraw `.svg` files there (download freely from [undraw.co/illustrations](https://undraw.co/illustrations)).
- The skill matches, recolors, and composes from whatever's in your library.

You get full unDraw quality on your own sites; the public repo stays clean of redistributed art. (The repo's `.gitignore` blocks `undraw_*.svg` as a guardrail.)

## Install

Drop the `ideagram/` folder into your skills directory. Python 3 only; no keys, no services. Optional PNG export needs `cairosvg` + system cairo (see the export script's message).

## Structure

```
ideagram/
├── SKILL.md                          — the workflow, read first
├── references/
│   ├── style-dna.md                  — Path A: the visual contract for every generated image (+ anti-slop list)
│   ├── composition-patterns.md       — Path A: structures + the fresh-metaphor recipe + scene-building
│   ├── prompt-template.md            — Path A: the image-gen prompt template + iteration/edit prompts
│   ├── qa-checklist.md               — Path A: post-generation quality gate
│   ├── undraw-anatomy.md             — Path B: dissection of real unDraw files + the reuse technique
│   ├── style-contract.md             — legacy no-image/no-library fallback (generated primitives)
│   └── metaphor-library.md           — legacy fallback
├── scripts/
│   ├── recolor_undraw.py             — recolor a real unDraw illo's accent to a brand color (the workhorse)
│   ├── extract_component.py          — lift a whole figure/device/panel out for composing a new scene
│   ├── validate_assets.py            — SVG well-formedness check
│   └── export_png.py                 — flatten to PNG at standard sizes
└── assets/primitives/                — the old generated-primitive kit, kept only as a last-resort fallback
```

## What this is, honestly

At its best (recoloring a real unDraw illustration to your brand) the output is indistinguishable from unDraw, because it is unDraw — that's the point, and it's a night-and-day improvement over hand-drawn primitives. The fallback path (generating from primitives when you have no library) produces simple pictograms and is clearly labeled as such. The skill's value is matching + recoloring + composing real art reliably, not out-drawing a human illustrator.

## License

MIT for the skill code, scripts, and docs (all original). Does not include or redistribute unDraw's illustrations — those you download yourself, free, from undraw.co.
