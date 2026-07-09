# Concept → visual metaphor library

The single hardest part of this whole skill isn't drawing — it's picking *what to draw*. Given a paragraph of content, the temptation is to try to depict everything in it. Don't. Pick the one concrete object or scene that a viewer would recognize in under a second, even with zero context. This file is a starting lookup for that translation; treat it as scaffolding, not an exhaustive catalog — extend it as new concepts come up.

| Abstract concept | Concrete visual metaphor | Primitives to compose |
|---|---|---|
| Idea / innovation / brainstorming | A lit lightbulb | `prop-lightbulb.svg`, optionally + `figure-standing.svg` holding/looking at it |
| Security / privacy / protection | A shield with a checkmark | `prop-shield.svg` |
| Communication / feedback / conversation | A speech bubble | `prop-speech-bubble.svg`, optionally two figures facing each other |
| Growth / progress / analytics / success metrics | An upward bar chart | `prop-chart-bar.svg` |
| Email / messaging / notifications | An envelope | `prop-envelope.svg` |
| Speed / launch / momentum / going live | A rocket | `prop-rocket.svg` |
| Remote work / focus / desk work / building something | A figure at a desk with a laptop | `figure-sitting-desk.svg` |
| A single user / person / individual contributor | A standing figure | `figure-standing.svg` |
| Collaboration / teamwork | Two `figure-standing.svg` instances, mirrored toward each other (see `style-contract.md`'s mirroring note — a plain side-by-side placement doesn't read as "facing"), optionally with a shared object (chart, speech bubble) between them | multiple figures + one shared prop |
| Real-time co-editing / live presence / multiple people in one document | Two figures flanking a `prop-document.svg`, with two small cursor markers (a simple triangle + a small rounded-rect "nameplate") pointing at different lines of text — use the fill-vs-outline technique from `style-contract.md` to make the two cursors distinguishable without a third color | 2× `figure-standing.svg` (mirrored) + `prop-document.svg` + two small hand-built cursor markers |
| Writing / content / documents / notes | A document with visible text lines | `prop-document.svg` |
| Onboarding / getting started | A standing figure + an open envelope or a lit path forward — pick whichever the actual product does (welcome email vs. guided steps) | context-dependent |
| Data / cloud / storage | Not yet in the primitive kit — until a dedicated primitive exists, compose from simple shapes (rounded rectangle stack) following the style contract rather than skipping the concept |

## How to pick when nothing matches directly

1. Strip the input down to one sentence: "this is fundamentally about ___." If that sentence still has two unrelated nouns in it, the content needs two illustrations, not one overloaded scene.
2. Ask what a five-year-old would draw for this concept with three crayons. That instinct — the single, obvious, almost-too-simple image — is usually the right one. Sophistication in this style comes from restraint, not from adding detail.
3. If the concept is genuinely abstract (e.g. "reliability," "scale," "trust") and no object maps to it directly, pick the closest adjacent concrete thing (trust → shield or handshake; scale → upward chart or multiplying figures) rather than inventing a literal abstraction (there's no good visual noun for "trust" itself — the shield stands in for it).
4. When composing multiple primitives into one scene, keep the same one-focal-point rule from `style-contract.md` — one element (usually the prop, not the figure) should carry the accent color and be the thing the eye lands on first.

## When a new concept doesn't map to anything here

Don't force it into a bad-fit existing primitive. Compose a new simple flat shape following `style-contract.md`'s shape language (circles, rounded rects, capsules, flat fill, two colors only), validate it with `scripts/validate_assets.py`, and consider adding it to `assets/primitives/` if it's likely to recur — that's how this library is meant to grow.
