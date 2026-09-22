✅ **Locked — Sep 15, 2026 — v1**

---

# SparkleUI — Media & Motion Skill

The playbook for anything that isn't flat markup: video, 3D, image sets,
blending, masking, parallax and cursor interaction.

Read this whenever a block involves a supplied render, a clip, a model, or
any effect that responds to the pointer or the scroll. It sits under
`design.md` and `rules.md` and covers the mechanics those two don't.

Everything here was learned by getting it wrong first. The failure table in
the `rules.md` Appendix is the short version; this is the working detail.

---

## What's in here

Eight techniques, plus the dial block and a self-check.

| §   | Technique                      | Reach for it when                                                                               | Came from                |
| --- | ------------------------------ | ----------------------------------------------------------------------------------------------- | ------------------------ |
| 1   | **Medium choice**              | Deciding between video, 3D, a transparent PNG or stacked layers before building anything        | —                        |
| 2   | **Subject sizing**             | A render has to sit at a designed size and stay sharp — not be cropped to fill                  | H5 Dexpress              |
| 3   | **Cursor scrub**               | A character should turn to follow the viewer's mouse                                            | H3 AI ROBO → H5 Dexpress |
| 4   | **Backdrop removal**           | A supplied clip arrives on a flat background that has to disappear against the page             | H5 Dexpress              |
| 5   | **Cross-fade & per-item tint** | Switching between large renders, and letting the active one colour its surroundings             | S1 Pick Your Deck        |
| 6   | **Anchored decoration**        | Marker notes, arcs, dots and arrows have to hold their position around a subject at every width | H5 Dexpress              |
| 7   | **Scroll parallax**            | Depth on scroll — layers at different speeds, occlusion rather than fade                        | H4 Traveltrip            |
| 8   | **Three.js**                   | The brief needs true perspective or a camera that moves                                         | H2 MADAR                 |
| 9   | Dial block                     | Every media block — the values the user tunes                                                   | —                        |
| 10  | Self-check                     | Before delivering anything with media in it                                                     | —                        |

**Two related techniques live elsewhere**, because they aren't media:

| Technique                                                      | File                               |
| -------------------------------------------------------------- | ---------------------------------- |
| **Auto-advancing selector** (the rail that steps on by itself) | `design.md` → Interaction Patterns |
| **Scroll reveals** (sections animating in as they're reached)  | `rules.md` §5                      |

---

## 1. Choosing the medium

| The brief needs                                       | Use                                 | Not                                                                |
| ----------------------------------------------------- | ----------------------------------- | ------------------------------------------------------------------ |
| A character that reacts to the viewer                 | video, scrubbed by cursor           | a still image tilted with CSS — it reads as a rotating sticker     |
| True perspective, or a camera that moves              | Three.js                            | hand-plotted SVG perspective; it reads flat and cannot re-converge |
| A subject at a designed size on a designed background | transparent PNG                     | a clip or photo with a baked backdrop                              |
| Depth on scroll                                       | separate layers at different speeds | one image with a zoom                                              |
| A product in five colourways                          | five supplied renders               | one render recoloured by filter                                    |

**Don't build elaborate stand-in artwork.** A hand-drawn SVG character that
looks wrong is worse than empty space, and it will be asked for twice: once
to build, once to remove. Ask for the asset.

---

## 2. Sizing media

**`object-cover` fills a box by cropping.** Correct for a background that
should reach every edge — a sky, an ocean, a texture. Wrong for a subject
that has a designed size, because "fill the window" and "be this big" are
different instructions, and cover will happily scale a character to three
times its intended size to satisfy the first.

**For a subject: size by one axis and let the other follow.**

```js
el.style.height = (pct / 100) * container.clientHeight + "px";
el.style.width = "auto";
```

Nothing is stretched, nothing is cropped, and every pixel draws once — which
is also why it stays sharp. Upscaling is the usual reason a supplied render
looks soft, and no amount of sharpening gets it back.

**Anything sized from the container is recomputed on resize.** Use a
`ResizeObserver` on the container, not just a window resize listener — the
container can change without the window doing so.

**Masking to crop costs resolution.** A mask that hides the bottom 20% forces
the image to scale up by 1.25 to compensate. Keep masks shallow, or crop the
source.

---

## 3. Cursor-scrubbed video

The effect: the pointer's horizontal position maps onto a clip's playhead, so
a character turns to follow the viewer. Frames, not transforms — it looks
real because it _is_ the artwork, seen from another angle.

**Mapping.** Absolute position, never accumulated movement. Deltas drift over
a session and have no neutral to return to.

```js
var nx = clamp((e.clientX / window.innerWidth - 0.5) / SCRUB.range + 0.5, 0, 1);
if (SCRUB.invert) nx = 1 - nx;
targetTime = nx * vid.duration;
```

**Always expose `invert`.** Which end of the clip corresponds to "looking
left" depends on how the clip was rendered, and it is a coin flip. A switch
costs one line; guessing costs a round trip.

**Queue the seeks.** Writing `currentTime` faster than the decoder can answer
makes the picture stutter and freeze. One seek at a time, each waiting for
the last to land — **with a timeout**, because a stall, a decode error or a
failed range request means `seeked` never fires, and without a way out the
effect is dead for the rest of the session:

```js
var FRAME_T = 1 / 30; // don't chase sub-frame differences

function release() {
  seeking = false;
  clearTimeout(guard);
}
vid.addEventListener("seeked", release);

function tick() {
  smoothTime += (targetTime - smoothTime) * SCRUB.ease;

  if (!seeking && Math.abs(vid.currentTime - smoothTime) > FRAME_T) {
    seeking = true;
    guard = setTimeout(release, 400); // the seek never landed; carry on
    vid.currentTime = smoothTime;
  }

  if (Math.abs(targetTime - smoothTime) > FRAME_T / 4)
    raf = requestAnimationFrame(tick);
  else raf = null;
}
```

**Set the threshold to about one frame.** Chasing a difference smaller than
the frame interval issues seeks that resolve to the frame already on screen —
work for no visible change.

**The clip has to be encoded for scrubbing.** A normal 2-second GOP means
`currentTime` writes land on keyframes and the character jumps between poses.
Ask for short-GOP or all-intra, and keep it short — two or three seconds of
rotation is plenty.

**Rest at the middle of the clip**, so there is room to turn both ways.

**Returning to rest when the pointer leaves the page needs care.**
`pointerleave` does not bubble, so a listener on `window` never fires. Bind it
to `document.documentElement`, or use `pointerout` and check
`relatedTarget === null`. Also return to rest on `blur`.

**Never autoplay or loop** a clip that is being scrubbed. `muted`,
`playsinline`, `preload="auto"`, no `autoplay`.

**Gate it on a real pointer**: `(hover: hover) and (pointer: fine)`, and off
entirely under `prefers-reduced-motion`.

**Show something while it loads.** Until `loadedmetadata` there is no
duration and no frame — decide what occupies that space rather than leaving
a hole.

---

## 4. Removing a clip's backdrop

A supplied clip usually arrives on a flat backdrop that has to disappear
against the page.

### A blend is confined to its isolated group

**This is the rule that cost the most, and it is easy to state backwards.**

`mix-blend-mode` blends an element with everything painted behind it _within
the same group_. With no isolating ancestor, that group is the page, and the
element blends happily with the page background. What confines it is
**isolation**: a stacking context (a non-`static` position with a
non-`auto` `z-index`), a mask, a filter, `opacity` below 1, or an explicit
`isolation: isolate`. Once an ancestor forms that group, the blend can only
see what is painted _inside_ it — and if that is nothing, the clip's backdrop
survives as a visible rectangle no matter which mode you set.

The trap is that the obvious way to hold media — an absolutely positioned
layer with `z-index: 0` — creates the group by itself, without anyone
deciding to.

**So: whatever layer forms the group must paint the surface colour.**

```jsx
{
  /* z-0 already forms the group; isolation makes that intent explicit,
    and bg-card is what the blend actually has to work against. */
}
<div className="absolute inset-0 z-0 bg-card [isolation:isolate]">
  <video style={{ mixBlendMode: "darken" }} />
</div>;
```

### Which mode

- **`darken`** — takes the **minimum of each channel** independently. Where
  the clip's backdrop is lighter than the surface it disappears; that's the
  win. But note the two consequences: a backdrop lighter in one channel and
  darker in another comes through **tinted**, and anything in the _subject_
  lighter than the surface — white highlights, teeth, eye whites, a pale
  sole — is flattened to the surface colour. On an off-white page that
  flattening is usually invisible. On a mid-tone page it is not, and
  `darken` is then the wrong tool.
- **`multiply`** — multiplies rather than replaces, so it always darkens.
  Leaves a faint tinted rectangle unless the backdrop is genuinely white.
- **`normal`** — no removal. Honest when the backdrop is meant to be seen.

### Feather every edge anyway

The two colours are rarely an exact match in all three channels, so the
_outline_ of the clip can still be picked out as a hairline even when the
interior looks right. Fading all four edges means there is no boundary to see.

Two gradients, intersected:

```js
function band(dir, a, b) {
  return (
    "linear-gradient(to " +
    dir +
    ", rgba(0,0,0,0) 0%, rgba(0,0,0,1) " +
    a +
    "%, rgba(0,0,0,1) " +
    (100 - b) +
    "%, rgba(0,0,0,0) 100%)"
  );
}
var m = band("bottom", top, bottom) + "," + band("right", side, side);

el.style.maskImage = m;
el.style.webkitMaskImage = m; // required, or older Safari masks nothing
el.style.maskComposite = "intersect";
el.style.webkitMaskComposite = "source-in";
```

Each gradient masks one pair of opposite edges; only the overlap stays opaque.

**Set both `maskImage` and `webkitMaskImage`.** Without the prefixed
property, Safari before 15.4 applies no mask at all, and the composite
property then has nothing to act on.

**Know the failure mode.** If `mask-composite` isn't understood, the initial
value is `add` — the _union_ of the two alphas, not a sum to opaque. The
union is fully opaque along all four edge midpoints and only fades at the
corners, so the picture survives intact but the hairline comes straight
back. Safe, but not silent: if you still see an edge, check whether the
compositing applied before re-tuning the numbers.

**Don't reach for a radial mask to feather all four sides at once.** To fade
the edge midpoints, its opaque radius has to be under 50%, which eats the
subject's head and feet. Radial masks are for vignettes, not feathering.

**Prefer a transparent PNG when one is available.** All of the above is a
workaround for a backdrop that shouldn't be there. A cut-out render makes the
rings, halos and colours behind it actually readable.

---

## 5. Image sets and cross-fading

When a rail or carousel switches between large renders:

- **Preload the whole set** once the section exists. A 1.7 MB render fetched
  on click means the transition waits on the network.
- **Decode the next image before touching the current one.** Load it into a
  detached `Image()`, and only start the fade in its `onload`. Otherwise the
  fade lands on a half-loaded frame.
- **First paint is not a cross-fade.** With nothing to fade from, set the
  source immediately rather than holding an empty frame for the fade duration.
- **Keep the fade on the image and the reveal on a wrapper.** Script writes
  the image's opacity for the cross-fade, so an entrance animation on that
  same property would fight it — see `rules.md` §7.

### Per-item theming

A selector feels built rather than assembled when the surroundings answer the
choice, not just the picture. Carry one colour per item and drive the ring,
glow and swatch from it:

```js
function tint(hex, a) {
  var n = parseInt(hex.slice(1), 16);
  return (
    "rgba(" +
    ((n >> 16) & 255) +
    "," +
    ((n >> 8) & 255) +
    "," +
    (n & 255) +
    "," +
    a +
    ")"
  );
}
ring.style.borderColor = tint(item.tint, 0.22);
glow.style.backgroundColor = tint(item.tint, 0.1);
```

Keep the alphas low — 0.1 to 0.25. The colour should be felt, not seen; at
full strength it stops being a page with an accent and becomes a page that
changes colour.

---

## 6. Anchoring decoration to a subject

Marker notes, orbit arcs, dots and arrows must keep their position _relative
to the subject_, not to the column they happen to sit in. A column stretches
with the window; the subject doesn't. Position marks against the column and
they drift apart at every width except the one they were tuned at.

**Put every mark in one layer, pinned to the subject's centre and sized from
it**, in a normalised coordinate system. Using the subject's halo radius as
100 units:

```jsx
<svg viewBox="-160 -120 320 240">{/* 3.2 radii wide, 2.4 tall */}</svg>
```

JS then sets that layer's size and transform from the same centre and radius
as the subject. Every mark now holds its position at any width, and one dial
(`marks`) pushes them all in or out together.

**Arcs that belong to one circle must be drawn as one circle.** Two hand-drawn
béziers cannot be aligned with each other — the eye reads the mismatch
instantly. Use real SVG arc commands on a shared radius:

```
M 21 -106 A 108 108 0 0 1 107 -14
M -64 -87 A 108 108 0 0 0 -103 -32
```

A shared radius cannot be out of alignment with itself. Dots then sit exactly
on that radius at the arcs' open ends.

Keep scattered strokes deliberate: two clean marks read as a sparkle, five
random ones read as dirt.

---

## 7. Scroll parallax

- **Separate design height from frame height.** The frame fills the screen so
  nothing leaks; parallax measures travel against a fixed `DESIGN_H`, so the
  movement is identical on every display.
- **Every moving layer needs slack in its direction of travel.** Size it from
  `distance to cover + travel + margin`, computed, not guessed. Every
  "mystery strip" is a layer that ran out.
- **A pinned element shorter than the viewport always gaps.** No mask or
  overflow fixes it. Either fill the viewport or don't pin.
- **Occlusion beats fade.** A wordmark that physically rides up behind a
  mountain is more convincing than one that dissolves.
- **Don't darken a whole section to make text readable over imagery** — you
  lose the image you paid for. A light tint plus `text-shadow` on the text
  keeps both.
- **A horizontal scroll container clips vertically too.** Card shadows and
  reveal animations need internal padding or they get sliced.

---

## 8. Three.js notes

Only when the brief needs true perspective or a camera move.

- Use the UMD build (r128 / r147), not a module, so the file opens directly.
- **WebGL lines are locked to 1px** — `linewidth` is ignored everywhere. For
  thick glowing lines build a **ribbon mesh** (two verts per curve point,
  offset perpendicular). Bonus: correct perspective taper for free.
- **Depth-only occluders** turn a see-through wireframe into a readable
  object: `colorWrite:false`, `depthWrite:true`, plus `polygonOffset`. Needed
  for _every_ enclosed volume — one missing under-body occluder lets road
  lanes draw straight through a chassis.
- **`UnrealBloomPass` destroys canvas transparency.** Its final copy pass
  writes alpha 1, so the canvas goes opaque over any CSS background. Stacked
  CSS `drop-shadow` on the canvas is the transparent-safe substitute.
- **Extruded 2D profiles** beat primitives for anything vehicle-shaped. Boxes
  can only ever look like boxes.
- **Draw flank detail once** when the camera stays in one quadrant. Far-side
  copies show through as ghost lines and double the geometry.

---

## 9. The dial block for media

Media is the part users always want to tune, so the config block matters most
here. Name the values for what they do, in units a designer thinks in.

```js
var FRAME = {
  height: 74, // % of the panel's height — how big the subject is
  x: 2, // nudge right (+) / left (−), % of panel width
  y: 3, // nudge down (+) / up (−), % of panel height
  blend: "darken", // how the clip's own backdrop is removed
  fadeTop: 8, // % of height the top edge dissolves over
  fadeBottom: 9,
  fadeSides: 8,
  halo: 56, // pale circle behind the subject, % of panel height
  marks: 1, // >1 pushes the drawn notes further out
};

var SCRUB = { range: 1, ease: 0.14, invert: true };
```

Then name the two or three that matter in the reply, and say which way to
move them if the thing they just complained about is still wrong.

---

## 10. Self-Check (media-specific)

- [ ] Is the subject sized by one axis, not cropped to fill?
- [ ] Does the layer that isolates a blended element paint the surface colour?
- [ ] Would `darken` flatten anything in the subject lighter than the surface?
- [ ] Are all four edges of any blended media feathered?
- [ ] Is the scrub mapped from absolute position, with an `invert` switch?
- [ ] Are seeks queued on `seeked`, with a timeout so a missed event can't
      kill the effect, and a threshold of about one frame?
- [ ] Is the rest-on-leave listener on the document element rather than
      `window`, where `pointerleave` never fires?
- [ ] Is the whole image set preloaded, and the next frame decoded before the fade?
- [ ] Are decorative marks anchored to the subject, not to a column?
- [ ] Are arcs that share a circle drawn as real arcs on one radius?
- [ ] Does every moving layer have slack in its direction of travel?
- [ ] Is every effect gated on a fine pointer and off under reduced motion?
- [ ] Does anything sized from the container recompute on resize?

---

## Changelog

- **Sep 15, 2026** — v1.2. Added a **What's in here** index at the top: the
  eight techniques by name, when to reach for each, and which block it was
  learned on — plus a pointer to the two related techniques that live in the
  other files.
- **Sep 15, 2026** — v1.1. Corrections from an audit against the Dexpress
  build. The blend-mode section was stated backwards — isolation confines a
  blend, not transparency — and `darken` is per-channel, which means it can
  tint and can flatten light pixels in the subject. Added the missing
  `-webkit-mask-image` to the mask snippet and corrected the `mask-composite`
  fallback (union, so the hairline returns; the picture survives). Seek
  queueing gained a timeout, a one-frame threshold and a note on GOP
  structure; `pointerleave` corrected from `window` to the document element.
  Added per-item theming to §5.
- **Sep 15, 2026** — v1 created. Medium selection, sizing, cursor-scrubbed
  video, blend-mode backdrop removal and the stacking-context rule, edge
  feathering, image sets and cross-fading, subject-anchored decoration,
  scroll parallax, Three.js notes, and the media dial block.
