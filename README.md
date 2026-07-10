# Ideagram

A Claude Code / Cursor / Windsurf skill that turns a concept into a **beautiful, on-brand unDraw-style illustration** — by matching it to a real unDraw illustration and recoloring it to your brand, so the output has genuine illustrator quality instead of a hand-drawn approximation.

## The realization behind it

The earlier version of this skill tried to *draw* illustrations from geometric primitives. It produced pictograms — no finesse, no scene, nothing like unDraw. The reason is simple: unDraw's figures are dozens of hand-placed bezier anchors drawn by a professional illustrator. An LLM generating path coordinates blind cannot reach that quality, and pretending otherwise wastes everyone's time.

So this skill doesn't draw. It **reuses the real path data**: match a concept to the right real unDraw illustration, then recolor it to your brand accent (swapping only unDraw's purple, leaving every skin tone, hair, and garment exactly as drawn). The output is genuinely illustrator-grade because it *is* the illustrator's work — just made yours. When no single illustration fits, it composes whole real components (a figure, a device, a background panel) into a new scene.

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
│   ├── undraw-anatomy.md             — dissection of real unDraw files: palette, structure, the reuse technique
│   ├── style-contract.md             — only for the no-library fallback (generated primitives)
│   └── metaphor-library.md           — only for the no-library fallback
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
