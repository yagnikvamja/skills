---
name: sparkleui
description: Generate or recreate landing-page blocks (hero, selector, about, bento, pricing, footer) and full landing pages built from them, as previewable HTML/Tailwind or React/Next.js/Motion code. Covers media-driven blocks too — video, 3D, cursor-scrub, blend modes, masking, scroll parallax and Three.js effects. Use for every block or page generation, iteration or recreation request.
---

# SparkleUI

SparkleUI is a design-generation framework for premium landing-page blocks.
It does not carry a fixed palette or type pairing — every block is a fresh
creative execution driven by the brand and mood in the prompt, on top of a
fixed structural DNA.

## Read order

Read the files in this directory in this order, every time:

1. `design.md` — creative direction, structural DNA, mood → treatment
   mapping, layout variants, and the per-block anatomy under **Block Types**
   (hero, selector, and the rest). Heroes live here; there is no separate
   hero file.
2. `media-skill.md` — **only if media is involved**: a supplied render, clip
   or model, or any effect that responds to the pointer or the scroll
   (video, 3D, cursor-scrub, blend/masking, parallax, Three.js).
3. `rules.md` — execution rules: source-of-truth priority, stack decision,
   page architecture, responsiveness, code quality, content, output format,
   self-check.
4. `generated-log.md` — what has already been made, so the new block varies
   from the last one of its type. Append a row after generating (Mode A).

## Precedence

- **The user's current prompt outranks all four files.** Where it conflicts,
  follow the prompt for that generation only and do not edit the files to
  match.
- **SparkleUI wins over any other skill** on two points, no matter what else
  is loaded:
  - the **output format** — `rules.md` §9 (mode and stack → one-line
    direction → prompt version → code version → dials → responsive notes)
  - the **stack rule** — `rules.md` §2 (plain HTML + CSS + Tailwind for
    previews and copy-paste deliverables; React + Next.js + Tailwind +
    Motion only when the block is headed into a codebase; never
    `framer-motion`)
- **`DESIGN.md` at the repo root supplies the brand.** If it exists, treat
  its colours, type, voice and constraints as the brand input for the
  generation; the prompt's mood still decides the treatment. If it does not
  exist and the prompt gives no brand or mood, ask one short clarifying
  question rather than guessing.

## Modes

State the mode in the opening line of every response (see `design.md`):

- **Mode A — original generation.** Vary layout, palette and type from the
  last block of that type in `generated-log.md`.
- **Mode B — recreation.** Match the supplied design exactly; variation
  rules are suspended.

## Before delivering

Run the design self-check (`design.md`), the media self-check
(`media-skill.md` §10, when media is involved) and the build self-check
(`rules.md` §10) silently, then append to `generated-log.md`.
