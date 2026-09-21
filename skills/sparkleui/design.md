✅ **Locked — Sep 15, 2026 — v2**

---

# SparkleUI — Design Generation Framework

The creative direction for every block SparkleUI produces, Hero included —
a hero is a block, so it lives here rather than in a file of its own.

This file says **what a block should be**. Its companions:

- `rules.md` — how it gets built and delivered
- `media-skill.md` — video, 3D, blending, masking, cursor and scroll effects
- `generated-log.md` — what has already been made, so the next one differs

---

## Core Philosophy

SparkleUI does **not** use a fixed color palette, font pairing, or visual
style. Every block should be a **unique creative execution**, generated fresh
from the brand and mood the user describes.

What stays fixed is not the *look*, but the **structural DNA** — the layout
logic that makes a block feel premium and complete, whatever visual style is
applied to it.

A director's brief, not a style guide: the story beats are fixed, the
cinematography changes every time.

---

## Two Modes of Work

SparkleUI produces both, and they follow different rules. **State which mode
is in play in the opening line of the response.**

### Mode A — Original generation

The user describes a brand and a mood. Everything below about varying layout,
palette and type applies in full. Check `generated-log.md` first and pick a
different combination than the last block of that type.

`generated-log.md` is the project's memory of what has already been made. It
lives alongside these files, and it is appended to — never rewritten — at the
end of every generation, with one row for the block and a short "avoid
repeating" line. Without that step the variation rule cannot be enforced.

### Mode B — Recreation

The user supplies a design and asks for it back as working code. **The
variation rules are suspended.** Match the supplied design exactly — the same
layout, colours, spacing, type and copy — and only add what was asked for on
top. Do not "improve" it, do not substitute a different arrangement because it
would be easier, and do not swap a colour because it sits better with the
palette. If something in the reference is genuinely not achievable, say so and
propose the nearest thing rather than quietly changing it.

---

## Structural DNA (always present, style-agnostic)

Every block must carry these, even when their visual treatment changes
completely:

1. **One dominant headline** — short, punchy, max 2 lines in Mode A. A
   recreation follows the supplied design's line breaks, however many.
2. **One supporting line** — 1–2 sentences, secondary weight/size
3. **One primary action** — pill or rounded button, high contrast against
   the background
4. **A background that carries the mood** — never plain white or empty
5. **Readability layer** — gradient overlay, scrim, backdrop blur, or
   `text-shadow` on the text itself, wherever text sits on visual complexity.
   On a light panel, `text-shadow` is the right tool: darkening an off-white
   surface to make dark text readable is backwards.

A Hero adds one more: a **navigation bar** — logo/mark left, links centre or
right, one action item right. On a landing page that nav belongs to the page,
not to the hero (see Landing Pages).

### Optional elements (use when they fit the mood, not by default)

- Eyebrow/badge tag above the headline (a small pill or tracked caps line)
- Secondary CTA (often a "watch/play" circular button)
- Social proof cluster (small overlapping avatars + short line)
- Carousel/next arrow control (side or bottom-right)
- Numbered progress indicator (`01 / 04`) for multi-slide blocks
- Oversized decorative background typography (a word rendered huge behind the
  content, mostly editorial and luxury moods)
- Hand-drawn marginalia — marker notes, arrows, orbit arcs, dots. Playful and
  character-forward moods only, and anchored to the subject rather than to a
  column (`media-skill.md` §6)
- Footer strip — trust logos, stats or spec rows along the bottom of a panel

---

## What Varies Every Generation (the creative surface)

For each new block, choose **one coherent direction** across all of these —
not mix-and-match:

| Dimension | Range of options |
|---|---|
| Background treatment | cinematic photography, 3D render, illustration, abstract gradient, video/motion loop |
| Typography mood | bold condensed display, elegant serif editorial, clean geometric sans, playful rounded |
| Layout | centered, left-aligned with right visual, asymmetric/oversized text, split-screen |
| Color mood | derived from the background itself, not a preset palette |
| Tone of copy | punchy/gaming, calm/luxury, technical/SaaS, warm/wellness |

**Rule of thumb:** background, typography and copy tone all reinforce the
*same* feeling. A playful 3D-character background does not take a formal serif
headline. Pick one emotional register and commit to it everywhere.

---

## Mood → Treatment Mapping

Pick the row that matches the brand mood. Don't blend rows. If the mood
doesn't match a row exactly, extend the *closest* row's logic rather than
inventing an unrelated direction.

| Mood | Background | Typography | Layout | CTA style | Color approach |
|---|---|---|---|---|---|
| **Gaming / Character** | 3D render or illustrated character, centered or off-center | Bold rounded or condensed sans, ALL CAPS ok | Centered subject, text top-left, social proof top-right | Rounded pill, dark bg + bright accent | Cool tones, high saturation on accent only |
| **Adventure / Energetic** | Dynamic photo/illustration with implied motion | Bold condensed sans, ALL CAPS | Subject fills frame, headline right-aligned or overlapping | Solid pill, white or bright accent | Warm/saturated, high-contrast |
| **Moody / Atmospheric** | Dark cinematic render, desaturated, high contrast | Elegant serif or thin sans, sentence case | Left text block, oversized translucent word behind subject | Ghost/outline button | Near-monochrome + one accent |
| **Lifestyle / Organic** | Warm natural imagery (light, water, nature) | Clean geometric sans, sentence case | Text left, overlapping rounded cards right | Pill, warm neutral | Warm neutrals, soft gradients |
| **Editorial / Mystical** | Painterly, centered wordmark treatment | Large elegant serif as focal element | Centered, minimal nav, slide counter bottom-left | Text link or thin outline | Deep jewel or muted earth |
| **SaaS / Corporate** | Abstract gradient, product shot, subtle 3D | Clean geometric sans, sentence case | Centered or left text, product visual right | Solid pill, brand primary | Brand primary + neutral greys |
| **Bright Commerce** | Cut-out product or character on a warm off-white panel, hand-drawn marginalia | Heavy geometric sans display + a marker face for notes | Three columns: copy left, subject centre, spec rows right | Solid pill with arrow | Off-white surface, one strong brand colour, greys for support |
| **Luxury / Premium** | High-end photography, muted, generous negative space | Refined serif or thin sans | Left-aligned, lots of whitespace | Minimal outline, no bright colours | Black/white/gold or monochrome + metallic |
| **Wellness / Calm** | Soft nature or abstract, light and airy | Rounded sans or soft serif, sentence case | Centered, generous spacing | Soft pill, pastel | Pastels, low saturation |

### Reference Direction Library

Structural patterns to calibrate range — not to be copied directly:

- **Character/product-forward** — large centered subject with text arranged
  around it; playful rounded CTA; avatar cluster; circular next-arrow
- **Adventure/energetic** — dynamic angle, motion blur or implied movement,
  bold condensed all-caps headline, single pill CTA, small stat counter
- **Moody/atmospheric** — dark, cinematic, desaturated; oversized translucent
  word behind the subject; numbered slide indicator; side-icon navigation
- **Lifestyle/organic** — warm natural background, soft rounded cards
  overlapping the image, pill-shaped nav CTA
- **Editorial/mystical** — elegant serif wordmark as centerpiece, painterly
  background, minimal top nav, slide counter bottom-left

The goal is range: SparkleUI should be able to produce any of these moods, and
moods beyond them, on demand.

---

## Layout Variants

Choose one per generation; vary from the previous block of that type in
`generated-log.md`.

- **Centered** — headline + subtext + CTA stacked over the background
- **Split** — text block one side, visual subject the other
- **Asymmetric / oversized text** — a huge decorative word as a background
  layer, smaller real content on top
- **Character-forward** — subject dominates the frame, text arranged in the
  negative space around it
- **Three-column** — copy left, subject centre, spec/detail rows right. Reads
  as a product page rather than a poster; suits commerce.
- **Corner-anchored** — headline one corner, copy panel the opposite one, rig
  or render filling the middle. Suits technical and 3D-led moods.
- **Rail + stage** — a column of options down one side, a large view
  answering the selection. See Interaction Patterns.

---

## Block Types

### Hero

The top-of-page, first-viewport block.

**Anatomy**

| Element | Rule |
|---|---|
| Nav bar | Logo left, links centre/right, one action item right. **Page-level and sticky — outside the hero panel, not inside it.** |
| Eyebrow/badge (optional) | Small pill or tracked caps line, max 4 words |
| Headline | 1 element, max 2 lines and ~8 words per line in Mode A; a recreation follows the supplied design |
| Subtext | 1 element, 1–2 sentences, max ~25 words |
| Primary CTA | 1 pill/rounded button, verb-first label ("Shop Decks", "Start Now") |
| Secondary CTA (optional) | Circular icon button or ghost text link |
| Background | Fills the first screen below the nav |
| Readability layer | Scrim, gradient or `text-shadow` wherever text meets complexity |
| Footer strip (optional) | Trust logos, stats or spec rows along the bottom |

**The hero is a panel, not the page.** Two consequences:

*The nav is not part of the hero.* It sits above every panel, stays sticky
through the whole page, and is transparent over the hero until the page moves.
A nav inside the hero panel sticks only within that panel — the panel needs
`overflow: hidden` to clip its media, which makes it the sticky scrollport —
so it scrolls away with the hero. `rules.md` §5 has the implementation.

*The hero fills what the nav leaves,* not the whole viewport:

```css
.hero-panel { min-height: calc(100dvh - var(--nav-space)); }
```

with `--nav-space` measured from the real bar *including its wrapper padding
and any top padding on the panel itself*, re-measured on resize and on
`document.fonts.ready`. `dvh` rather than `vh`, or the hero overflows a mobile
screen by the height of the URL bar.

*The hero rounds its top corners only.* The section below butts straight onto
it with no gap, so the page reads as one continuous surface.

**Readability patterns**

```html
<!-- Bottom-to-top gradient, for centered/bottom-heavy text -->
<div class="absolute inset-0 bg-gradient-to-t from-black/70 via-black/20 to-transparent"></div>

<!-- Left-to-right gradient, for split/left-aligned text -->
<div class="absolute inset-0 bg-gradient-to-r from-black/70 via-black/10 to-transparent"></div>

<!-- Full scrim, for busy backgrounds needing centered text -->
<div class="absolute inset-0 bg-black/40"></div>

<!-- Backdrop blur card, for text in a contained box -->
<div class="backdrop-blur-md bg-black/30 rounded-2xl p-8"></div>
```

Match the pattern to the layout. Don't put a full scrim over a bright
character-forward hero where the character should stay vibrant. **On a light
hero, don't reach for a scrim at all** — use `text-shadow` on the text.

**Copy**

- Capitalisation follows the mood row (ALL CAPS for Gaming/Adventure,
  sentence case elsewhere).
- The headline expresses an outcome or a feeling, not a feature list.
- Subtext adds context — it doesn't restate the headline.
- CTA labels are verbs: "Shop Decks", "Explore", "Get Started". Never "Click
  Here" or "Submit".
- Marker notes and marginalia are asides, not information. Three or four
  words, never anything the reader needs.

**Responsive**

- Headline scales down at least 2 steps from desktop to mobile. Prefer a
  single `clamp()` over a ladder of breakpoint classes.
- Split and three-column layouts stack vertically below 1024px, with the
  subject given an explicit `min-height` so it doesn't collapse.
- Nav collapses to logo + menu icon below 1024px.
- Backgrounds use `object-cover`; **subjects don't** — size those by height
  (`media-skill.md` §2).
- Decorative marks hide below `lg`. There is no room, and they fight the
  stacked content.

### Selector (rail + stage)

A column of options down one side, a large view answering the selection.
Built once, for Dexpress "Pick Your Deck".

- **Centre the group.** Rail hard left and stage hard right leaves dead space
  in the middle; a detail panel between them fills it and gives the
  selection somewhere to speak.
- **The active state is a shape, not a card.** A ring that closes around the
  chosen option reads better than a box behind it.
- Behaviour is specified under Interaction Patterns below.

### Not yet built

About, Bento, Pricing, Footer, Feature, Testimonials, FAQ, CTA. When the
first of each is made, add its anatomy here rather than starting a new file.

---

## Landing Pages (multi-section output)

A hero is often the start of a page rather than the whole delivery. When a
block grows into a page:

- **One shared alignment system, defined once.** Every panel uses the same
  outer margin and the same inner padding, expressed as two CSS rules the
  whole page shares. Per-section padding values are how alignment drifts.
  `rules.md` §5 has the implementation.
- **Panels join, they don't float.** Consecutive sections read as one
  continuous surface: the first rounds its top corners, the last rounds its
  bottom, everything between is square, and there is no vertical gap.
- **The navigation is part of the page, not part of the hero.** It lives
  above every panel and stays with the user as they scroll.
- **Every section reveals on scroll**, using the same timing family as the
  hero's entrance. A page where only the first screen is animated feels
  half-finished.
- **Vary section rhythm.** Don't stack three identical left-text/right-image
  sections. Alternate the weight — full-bleed, centred, split, rail.

---

## Interaction Patterns

Interactivity earns its place when it lets someone see something they
couldn't otherwise. Two patterns are established:

**Cursor-driven subject.** The pointer's horizontal position maps onto a
video's playhead so a character turns to follow the viewer. Absolute
position, never accumulated movement. See `media-skill.md` §3.

**Auto-advancing selector.** A rail of options steps on by itself while a
larger view answers each step. Non-negotiable behaviours: it pauses on hover
and on focus; it pauses while off screen; **a click stops the cycle for
good** — someone who has chosen should not have their choice moved on a
timer; arrow keys work; and with reduced motion it never advances on its own
and waits to be driven.

One colour pulled from the active option should drive the surrounding
treatment — a ring, a glow, a swatch — so the whole view answers the
selection rather than just the picture changing.

---

## Output Requirements

Every generated block produces **two deliverables**, since SparkleUI serves
both audiences:

1. **Prompt version** — a clean, copy-paste-ready natural-language prompt
   that could be pasted into an AI coding tool to regenerate this design.
2. **Code version** — see the stack rule below.

### Stack — decided by where the block is going

There is no single default. Pick the branch that matches the destination and
**say which branch was picked, and why, in one plain sentence** at the top of
the response.

**Branch A — React + Next.js + Tailwind + Motion.** For blocks headed into a
real codebase.

- React components using the **`motion`** package, imported as
  `import { motion } from "motion/react"`.
- **Never** `framer-motion` — that is the old package name for the same
  library and the import path is wrong.
- Delivered as standalone component files, PascalCase, **`.jsx`** unless the
  user says the project is TypeScript — then `.tsx`. Not wrapped in a
  preview page.
- Note any imports the user needs to install.

**Branch B — HTML + Tailwind.** For standalone blocks: previews, copy-paste
distribution, anything the user wants to open and look at immediately.

- A single self-contained HTML file with the Tailwind CDN and fonts linked
  in the head — save it, open it, it renders with no setup.
- Animation in CSS keyframes, no libraries. Scroll reveals use an
  `IntersectionObserver` (`rules.md` §5).
- **A theme block is required**, in a plain `<script>` *after* the CDN
  `<script src>`. Tokens are named for their role, not their colour —
  `page`, `card`, `ink`, `grey`, plus one or two brand names — so the same
  markup survives a palette change.
- Fonts: `preconnect` to both Google Font origins and `display=swap`. Late
  font load is why any measured height must be re-measured on
  `document.fonts.ready`.

**Why the split matters:** React and Motion cannot be previewed inline in
chat — there is no build step. HTML can. So exploration and visual review
happen fastest in Branch B, while Branch A is the deliverable for real work.
When a block will need both, build and review in HTML first, then convert.

**A long-running page stays in one branch.** Once a page is being built and
refined in Branch B, don't convert it mid-flight because a new section needs
state — a rail, a carousel and a cursor effect are all a few lines of plain
JavaScript. Convert when the user asks for the codebase version, not before.

### Animation style (both branches)

- Smooth and subtle, never flashy or distracting.
- Entrances: fades, slides, gentle scale.
- Scroll-triggered reveals for sections as the user scrolls.
- Small hover and tap feedback on buttons and interactive elements.
- Timing **200–500ms** unless a slower feel is specifically requested.
- Always guard with `prefers-reduced-motion` — content visible by default,
  motion layered on only when welcome.

### Asset Handling

- **CSS/SVG first** — build backgrounds from gradients, mesh, noise and
  geometric SVG wherever the mood allows, so the block renders with zero
  external assets.
- **CDN URLs** for supplied artwork, video or 3D models. Never local file
  paths. A page opened from disk *will* show a relative `<img>` or `<video>`,
  but anything loaded through `fetch`/XHR or a module script — which is how
  every `.glb` loader works — is blocked by the opaque `file:` origin and
  fails silently.
- **A small asset with no CDN URL gets embedded** as a base64 data URI, so
  the file stays self-contained. Sensible up to roughly 10 KB; past that,
  ask for a URL.
- **User-supplied assets always take priority** over generated stand-ins.
- **Don't build elaborate placeholder artwork.** A stand-in that looks wrong
  is worse than an empty space — ask for the asset instead.
- **Never write copy that describes an asset you haven't looked at.** When a
  numbered set arrives (`sb-01…sb-05`), confirm what each file actually
  contains before naming it. Guessing the order produced a rail where the
  red board was labelled "Forest Green".
- **Expose the asset list as a named block** at the top of the script, so the
  user can swap a URL without reading the code around it.

### The dial block (mandatory)

The user is a designer, not a developer. Every generated block ends up being
tuned, and tuning must not mean reading code.

So every block exposes a **named configuration object at the top of its
script**, holding every value the user is likely to want to change — size,
position, speed, strength, colour, timing — with a plain-language comment
per line saying what it does and which direction to move it. Values are in
units a designer thinks in: percentages of the container, milliseconds,
named blend modes. Not internal maths.

Then say in the reply which two or three dials matter most, and what to
change if the specific thing they just complained about is still wrong.

---

## Worked Example

**User prompt:** "Generate a hero for a mystical fantasy game website."

**Step 1 — Mode:** original generation.
**Step 2 — Mood:** Editorial/Mystical, extending toward Moody's dark
cinematic quality.

**Step 3 — Choices:**

- Background: dark atmospheric render
- Typography: elegant serif headline, sentence case
- Layout: centered, oversized translucent word behind the subject
- CTA: ghost/outline button
- Readability: bottom-to-top gradient

**Step 4 — Response structure:**

```
## Mode & stack
Original generation. HTML + Tailwind — static layout, no build step, so you
can open it straight away.

## Creative direction
Moody/Mystical — dark atmospheric background, elegant serif headline,
centred with decorative oversized text behind the subject.

## Prompt version
"A full-viewport hero for a mystical fantasy game. Dark atmospheric
background (misty forest or ancient ruins at night). Centred layout with an
elegant serif headline reading '[Game Name]', a short supporting line below,
and a single outline CTA reading 'Enter the Realm'. Subtle oversized
translucent word behind the subject for depth. Bottom-to-top gradient for
readability. Minimal sticky top nav with logo and four links."

## Code version
[the file]

## The dials
FRAME.height — how large the subject stands, % of the panel
FRAME.y      — nudge him up or down
WORD.opacity — strength of the decorative word behind him

## Responsive notes
Headline clamps between 2.6rem and 4.9rem. Nav collapses below 1024px.
Decorative word hides below lg.
```

---

## Non-Negotiables

- Never ship a block with a plain or empty background — a background that
  carries the mood is mandatory.
- Never sacrifice text readability for aesthetics — always solve contrast,
  with a scrim on dark imagery or `text-shadow` on a light panel.
- Never reuse the exact same layout+background+type combination twice in a row
  (Mode A only). Check `generated-log.md`.
- Always keep headline + subtext + CTA as the non-negotiable content trio.
- Never silently substitute a different layout, colour or asset because it is
  easier. Name the problem and offer the nearest alternative.

---

## Self-Check (design side; `rules.md` §10 covers the build)

- [ ] Is the mode stated, and are the variation rules applied or suspended
      accordingly?
- [ ] Does the mood match one row of the mapping, or clearly extend the
      closest one?
- [ ] Do background, typography and copy tone reinforce one feeling?
- [ ] Is the layout different from the previous block of this type in
      `generated-log.md`? (Mode A)
- [ ] Is contrast solved over the busiest part of the background — and with a
      scrim only where a scrim belongs?
- [ ] Is the CTA label a verb?
- [ ] Headline within 2 lines / ~8 words per line? (Mode A)
- [ ] For a hero: is the nav outside the panel and sticky, is the panel's
      height measured from it rather than hard-coded, and does the panel
      round only its top corners with no gap below?
- [ ] Are decorative marks anchored to the subject rather than a column?
- [ ] Is the config block present and in plain language?
- [ ] Has `generated-log.md` been appended to?

---

## Changelog

- **Sep 15, 2026** — v2. **`hero-skill.md` merged in and retired** — a hero
  is a block, so a separate file only split the rules across two places. Its
  contents now live here: the Mood → Treatment mapping (9 rows, including the
  new **Bright Commerce**), the layout variants, the Hero anatomy and
  panel rules, the readability patterns, hero copy and responsive rules, and
  the worked example. Added a **Block Types** section so future blocks are
  documented in place, with the Selector pattern recorded and a list of block
  types still unbuilt. Structural DNA generalised from "every hero" to "every
  block", with the nav called out as hero-only.
- **Sep 15, 2026** — v1.3.1. Audit corrections: "full-bleed" replaced with
  "fills the first screen below the nav"; `text-shadow` added as a readability
  option; the two-line headline rule scoped to Mode A; Branch A aligned with
  rules.md (Next.js, `.jsx` unless TypeScript); theme block, font loading and
  scroll reveals specified for Branch B; data-URI embedding given a rule; the
  marginalia anchoring reference pointed at media-skill.md §6;
  `generated-log.md` defined; and the auto-advancing selector corrected so a
  click stops the cycle rather than restarting it.
- **Sep 15, 2026** — v1.3. Added **Two Modes of Work**; a **Landing Pages**
  section; an **Interaction Patterns** section; a mandatory **dial block**
  convention; two new Asset Handling rules; and the rule that a page in flight
  stays in one branch.
- **Aug 25, 2026** — v1.2. Stack rule replaced with a destination-based split:
  React + `motion/react` for real codebases, HTML + Tailwind for standalone
  previews, with the chosen branch stated in every response. Added the shared
  Animation Style section. Asset Handling tightened.
- **Aug 18, 2026** — v1.1. Output Requirements rewritten to match `rules.md`;
  added Delivery Format and Asset Handling sections.
- **Aug 18, 2026** — v1 created.

### Retired files

- **`hero-skill.md`** (v1 Aug 18 → v2.1 Sep 15) — merged into this file on
  Sep 15, 2026. Nothing was dropped. Delete it from project knowledge so
  there is one source of truth.
