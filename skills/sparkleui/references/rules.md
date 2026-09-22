✅ **Locked — Sep 15, 2026 — v2**

---

# SparkleUI — Generation Rules

Hard operational rules for generating any block (Hero, About, Bento, Pricing,
Footer, etc.) and any landing page built from them. `design.md` governs
_creative direction_ — what it should look like, and the anatomy of each
block type, heroes included. This file governs _execution_ — how it gets
built and delivered. Both apply together, every time.

---

## 1. Source of Truth Priority

**The user's current prompt outranks everything below it.** Where it
conflicts with any of these files, follow the prompt for _that generation
only_ — and do not silently edit the underlying files to match.

Then read, in this order:

1. **design.md** — creative direction, structural DNA, mood mapping, and the
   per-block anatomy under **Block Types**. Heroes live there too; there is
   no separate hero file.
2. **media-skill.md** — if the block involves video, 3D, blending, masking,
   parallax or cursor interaction
3. **This rules.md** — technical execution rules
4. **../generated-log.md** — what has already been made, so the new block
   varies from it. Append to it after generating.

If something genuinely needed is missing — no brand mood, no reference for a
recreation — ask one short clarifying question rather than guessing and
generating anyway. **The stack is never one of those questions:** it is
always React + Next.js + Tailwind + Motion (§2).

**Exception — don't ask for what you can look at.** If the answer is in a
file, a URL or an image the user already supplied, check it. Questions are
for decisions only the user can make, not for facts sitting in front of you.

---

## 2. Stack

Every block is **React + Next.js + Tailwind + Motion**, always — there is no
other branch to choose. `design.md` holds the full stack rule.

- Motion is imported as `import { motion } from "motion/react"` — **never**
  `framer-motion`, which is the old package name for the same library and
  the import path is wrong.
- Delivered as standalone component files, PascalCase, **`.jsx`** unless the
  user says the project is TypeScript — then `.tsx`. Not wrapped in a
  preview page.
- Interactive state (a carousel, an auto-advancing rail, a cursor effect, a
  scroll parallax) is built the same way regardless — React state and
  effects, not a reason to second-guess the stack.

---

## 3. Knowledge Adherence

- Generate strictly from what's provided in design.md, media-skill.md,
  and the user's prompt — do not invent brand details, color meanings,
  or structural patterns that weren't specified anywhere.
- If the requested style isn't covered by the Reference Direction Library in
  design.md, extrapolate using the same _mood-matching logic_, not a random
  new pattern — stay inside the system's spirit even for novel moods.
- Never silently substitute a different layout/style because it's "easier" —
  if a request is genuinely not feasible in the current stack, say so and
  propose the closest feasible alternative instead of quietly downgrading it.
- **When recreating a supplied design, match it.** See design.md → Two Modes
  of Work. Variation rules do not apply to a recreation.

---

## 4. Diagnosis Discipline

This is the rule most often broken, and the one that costs the user the most
time. Every expensive detour in this project came from a confident wrong
diagnosis, not from a hard problem.

- **One guess is allowed. A second guess is not.** If a visual bug survives
  the first fix, stop patching and find the actual mechanism before writing
  any more code. Read the spec, check what the browser really does, reason
  about the stacking or layout model.
- **Say "I had the cause wrong" when that's what happened.** Silently trying
  something else reads as progress and isn't.
- **When the user reports the same symptom twice, the previous explanation
  was wrong.** Do not restate it in different words.
- **Fix the whole class, not the instance.** If one image was broken by a
  Tailwind CDN limitation, every image with the same pattern is broken too.
  Sweep the file before replying.
- **Never announce something is fixed when it is untested.** Say what was
  changed and what to look for.

Worked examples of causes that were missed the first time are in the
Appendix at the end of this file.

---

## 5. Page Architecture

For any output with more than one section.

**One alignment system, defined once.** Two rules, used by the nav and by
every panel:

```css
.frame { padding-left: 12px; padding-right: 12px; }   /* outer margin */
.inset { padding-left: 20px; padding-right: 20px; }   /* inner padding */
@media (min-width: 640px)  { .frame { … 16px } .inset { … 32px } }
@media (min-width: 1024px) { .frame { … 20px } .inset { … 56px } }
```

Every left edge on the page then agrees by construction. Repeating padding
utilities per section is how alignment drifts, and it drifts invisibly until
someone screenshots two sections together.

**Panels join.** Consecutive sections read as one continuous surface: first
panel rounds its top corners only, last panel rounds its bottom only,
everything between is square, and no vertical padding sits between them.

**Navigation is page-level and sticky.**

- It must live **outside** every panel. A sticky element positions against
  the nearest ancestor with non-visible overflow, so a nav inside a panel
  that clips its media sticks only _within that panel_ and scrolls away with
  it — it looks broken rather than being obviously broken, which is worse.
- Transparent at rest, picking up a frosted background, hairline border and
  soft shadow once the page has moved. The mechanism: one scroll listener,
  `{ passive: true }`, toggling a single class past a ~12px threshold.
  `backdrop-filter` needs its `-webkit-` twin, and does not transition — so
  transition the background and border, and let the blur switch.
- Measure its real height into a CSS variable and let the hero fill the
  remainder:

  ```css
  min-height: calc(100dvh - var(--nav-space));
  ```

  Use `dvh`, not `vh`: on mobile `100vh` is the _large_ viewport, so a
  `vh`-sized hero overflows the visible screen by the height of the URL bar.

  **Measure the whole sticky wrapper, including its padding, and count any
  top padding on the panel below.** `--nav-space` has to cover everything
  above the panel's own box or the first screen overflows by that much.
  Re-measure on resize, on `ResizeObserver`, and on `document.fonts.ready` —
  a hard-coded number is wrong the moment the bar wraps or the font lands.

- Give sections `scroll-margin-top` equal to that variable, or anchor links
  park their target underneath the bar.

**Every section reveals on scroll**, using the same timing family as the
hero. That means Motion's `whileInView` (or an `IntersectionObserver` behind
a `useEffect` when the reveal needs more control than `whileInView` gives),
not a load-time delay — a delay fires while the section is still below the
fold, so by the time it is scrolled to, the reveal is already over:

```jsx
<motion.section
  initial={{ opacity: 0, y: 24 }}
  whileInView={{ opacity: 1, y: 0 }}
  viewport={{ once: true, margin: '0px 0px -8% 0px', amount: 0.15 }}
  transition={{ duration: 0.4 }}
>
```

The element is visible without JavaScript (its rest state is opacity 1
unless `initial` is applied), so nothing is hidden if motion is reduced —
gate the animated props behind `useReducedMotion` from `motion/react`.

---

## 6. Responsiveness (non-negotiable)

- Every block must work at three breakpoints minimum: mobile (~375px),
  tablet (~768px), desktop (~1280px+).
- Mobile-first Tailwind classes (`base → sm: → md: → lg:`), not desktop-first
  overrides.
- Text must never overflow, truncate awkwardly, or overlap on small screens —
  headline sizes should scale down, not just wrap.
- Background images/videos must use `object-cover` (or equivalent) to avoid
  distortion — **except** when the subject must sit at a designed size, in
  which case size by height only and let width follow. See media-skill.md §2.
- Touch targets (buttons, nav links) must be large enough for mobile tapping
  (minimum ~44px touch area).
- Anything sized against the viewport must be recomputed on resize, not set
  once at load.

---

## 7. Code Quality Rules

- Use semantic markup (`<header>`, `<nav>`, `<section>`, `<h1>`, etc.) —
  never generic `<div>` soup.
- Include `alt` text for all images. Decorative layers get `aria-hidden`.
- No inline `style={{}}` props — Tailwind classes only, unless a value is
  genuinely dynamic and computed at runtime.
- Keep naming consistent: PascalCase for React components (`HeroSplit.jsx` /
  `HeroSplit.tsx`).
- No unused code, commented-out blocks, or placeholder `// TODO` in final
  output. When a feature is removed, remove its CSS, its markup and its
  variables in the same pass.
- Minimize external dependencies — don't introduce a library for something
  Tailwind or vanilla JS can already do.
- **Comments explain why, not what.** A comment earns its place by recording
  the thing that would otherwise be rediscovered painfully: why this element
  is wrapped, why this blend mode, why this value is computed rather than
  guessed.

### Tailwind — known limits

Tailwind runs through the project's real build (PostCSS/Next.js), not a
browser CDN, so most class-extraction surprises don't apply. Two still do:

- **A class that isn't a real utility generates nothing, silently.** There is
  no error, no warning, no visible difference from a class that simply
  inherits — which is exactly what makes it expensive. The one that bit us:
  `font-700` is not Tailwind. The scale is
  `font-medium` / `font-semibold` / `font-bold` / `font-extrabold`, and the
  arbitrary form is `font-[700]`. **If a property looks like it isn't
  applying, check the class exists before checking anything else.**
- **Complex arbitrary values are fragile.** Commas are legal
  (`bg-[rgb(255,0,0)]`), but spaces are not — they must be written as `_`
  (`w-[max(1500px,_120vw)]`) — and any value the extractor can't read as one
  complete token produces no rule. Negative arbitrary values need the leading
  dash outside the bracket (`-mt-[10%]`). Anything load-bearing for layout
  belongs in a real CSS rule, where it either works or visibly doesn't.

### CSS animations vs JavaScript

**A running CSS animation overrides inline styles on the properties it
animates** — fill-mode has nothing to do with it. `forwards` only extends
that override past the animation's end, which is what turns a transient
glitch into a permanent one. So a non-`forwards` animation is _not_ safe to
combine with JS either; it just breaks for a shorter time.

An element whose transform or opacity is written by script must therefore not
carry an entrance animation that touches **the same property**. Either:

- put the reveal on a wrapper element — this is what the deck render does,
  because JS drives its opacity for the cross-fade; or
- use an opacity-only reveal and drive only transform from JS — this is what
  the hero video does, since its placement transform and the `draw` fade
  never touch the same property.

Both are correct. What is not correct is one element with a `forwards`
animation on a property a script also writes.

---

## 8. Content Rules

- Never use "Lorem ipsum" — always write real, mood-appropriate placeholder
  copy that matches the brand tone described (see design.md tone options).
- Headlines: max 2 lines. Subtext: max 2 sentences.
- Don't fabricate specific stats, testimonials, or client names — use clearly
  generic placeholders ("10,000+ users", not a fake precise number presented
  as fact) unless the user supplies real data.
- **Copy that names a supplied asset must match that asset.** Open it, or
  read the screenshot, before writing the label. See design.md → Asset
  Handling.

---

## 9. Output Format (every generation)

Structure the response in this order:

1. **Mode** — original generation or recreation, in one sentence
2. **One-line summary** of the creative direction (mood → background
   treatment → typography choice)
3. **Prompt version** — copy-paste-ready natural language prompt
4. **Code version** — full working code block(s), or the file itself
5. **The dials** — the two or three values in the config block the user is
   most likely to want, in plain language, with the direction to move them
6. **Responsive notes** — anything they should know about how it adapts

On an iteration rather than a first generation, replace items 2 and 3 with:
what was wrong, what the actual cause was, and what changed. Keep it short,
and still deliver the file and the dials.

Do not skip the prompt version on a first generation — SparkleUI serves both
audiences.

---

## 10. Self-Check Before Delivering (run through silently)

**Design**

- [ ] Follows design.md's Structural DNA (nav, headline, subtext, CTA,
      full-bleed background, readability layer)?
- [ ] Background, typography and copy tone all reinforcing one mood?
- [ ] Contrast and readability solved over the busiest part of the background?
- [ ] Layout different from the last block generated (Mode A only)?

**Build**

- [ ] Works at mobile, tablet and desktop?
- [ ] Every panel using the shared `.frame` / `.inset` rules?
- [ ] Nothing load-bearing relying on a Tailwind arbitrary value with a comma?
- [ ] No element with an animation on the same property a script writes?
- [ ] Every utility class actually a real utility — no `font-700`?
- [ ] Scroll reveals on `whileInView` (or an `IntersectionObserver` behind
      `useEffect`), not a load-time delay?
- [ ] Anything JS-built styled with real CSS?
- [ ] Removed features fully removed — CSS, markup and variables?
- [ ] Every sized-from-viewport value recomputed on resize?
- [ ] Motion imported from `motion/react`, never `framer-motion`?

**Assets and copy**

- [ ] Every asset a CDN URL, never a local path?
- [ ] Every label checked against the asset it names?
- [ ] Large image sets preloaded so a transition never waits on the network?

**Delivery**

- [ ] Config block present, in plain language, at the top of the script?
- [ ] Both prompt and code versions included (first generation)?
- [ ] If a previous diagnosis was wrong, said so plainly?

If any box fails, fix it before sending — don't deliver and ask the user to
catch the issue.

---

## Appendix — Failures worth not repeating

Each of these was shipped, reported, and only then understood.

| Symptom                                              | Wrong guess                                       | Actual cause                                                                                                                                            |
| ---------------------------------------------------- | ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Background glow disappeared a few seconds after load | easing value; heavy overlays                      | `UnrealBloomPass`'s final copy pass writes alpha 1, turning the transparent canvas into an opaque rectangle                                             |
| Vertical seam beside a full-bleed image              | image too small                                   | Tailwind CDN silently dropped `w-[max(…,…)]` — the rule never existed                                                                                   |
| Image still wrong after that was fixed               | —                                                 | Preflight's `img { height:auto }` loads after the head and re-clamped it                                                                                |
| "Mystery strips" during parallax                     | mask needed                                       | a moving layer ran out of image; size from `distance + travel + margin`                                                                                 |
| Blue band under a pinned hero                        | fixable with a mask                               | a pinned element shorter than the viewport always gaps; fill it or don't pin                                                                            |
| Video backdrop visible as a rectangle                | the mask was wrong; then the blend mode was wrong | the blend was confined to an isolated group, and the layer forming that group painted no colour — so the only thing available to blend with was nothing |
| Every font weight looked slightly too light          | the font hadn't loaded                            | `font-700` is not a Tailwind class; it generated no rule and every weight was inherited                                                                 |
| Decorative marks looked crooked                      | needed nudging                                    | they were positioned against a column that stretches; and the two arcs weren't parts of the same circle                                                 |
| Deck named "Forest Green" showed a red board         | —                                                 | the list was written before looking at the files                                                                                                        |

---

## Changelog

- **Sep 22, 2026** — v2.2. Removed the HTML/Tailwind branch — every block is
  now delivered as React + Next.js + Tailwind + Motion, since that is where
  almost all output was actually headed. §2 rewritten from a stack decision
  to a single stack description. Scroll reveals moved from a vanilla
  `IntersectionObserver` recipe to Motion's `whileInView`. Dropped the
  Tailwind Play-CDN-specific limits (Preflight load order, JS-built-element
  guidance) that only applied to a browser-compiled stylesheet; kept the
  class-extraction gotchas that still apply under a real build. Output
  format and self-check no longer ask which branch was picked.
- **Sep 15, 2026** — v2.1. Corrections from an audit of the four files against
  the Dexpress build. Fixed the stated mechanisms for `position: sticky` and
  `overflow` (scrollport, not cancellation), for blend modes (isolation, not
  transparency), for CSS animations vs inline styles (a _running_ animation
  wins; `forwards` only extends it), for Tailwind arbitrary values (whitespace
  and token extraction, not commas), and for Preflight (specificity, not load
  order). Added the `font-700` class of failure, `dvh` for mobile viewport
  height, the `IntersectionObserver` recipe for scroll reveals, the nav-height
  padding arithmetic, and the frosted-nav mechanism. Reordered §1 so the
  user's prompt is stated as outranking the files, added `../generated-log.md` to
  it, and removed the stack from the list of things to ask about.
- **Sep 15, 2026** — v2. Added **§4 Diagnosis Discipline** and the failure
  appendix. Added **§5 Page Architecture** (shared alignment system, joined
  panels, sticky page-level nav, measured nav height). Added the Tailwind CDN
  limits and the CSS-animation-vs-JS conflict to §7. Added the config/dial
  block to the output format, and asset-verification to §8. Stack logic
  rewritten so interactive state alone is no longer an escalation trigger.
  Added media-skill.md to the source-of-truth order, and the
  "don't ask for what you can look at" exception to §1.
- **Aug 18, 2026** — v1 created. Source-of-truth priority, stack decision
  logic, knowledge adherence, responsiveness, code quality, content rules,
  output format, and self-check checklist established.
