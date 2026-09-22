# Third-Party Skills & AI Resources

> Collection of third-party skills and AI resources that I mostly use in my projects.
>
> Each entry's `Install` command is the one to run — most third-party skills
> are plain GitHub repos the `skills` CLI can add directly, but a few ship
> their own installer instead, and those must use it.
>
> **Default target is `.claude` + `.agents` only.** Every command below
> already pins that pair explicitly — extend it only if you also want
> another coding agent (Cursor, Gemini, Copilot's `.github`, etc.).
>
> For a `skills` CLI command, always add `-a`/`--agent` explicitly (repeat
> the flag once per agent — comma-separating agents is read as one invalid
> name). Run without a TTY, the CLI auto-detects the running agent and
> installs for it only, skipping `.agents/skills/` (`universal`) entirely.
> When a source repo bundles more skills than are wanted, pass `-s`/`--skill`
> once per skill to install (comma-separating is read as one invalid name,
> same as `--agent`) — the whole repo is only installed without `-s` when
> every skill in it is wanted.
>
> Entries are grouped into categories (**Design skills**, **Code skills**,
> …), and within each category split into **Core** (installed for everyone,
> used on every section) and **Situational** (installed, but only invoked
> when the task calls for it).

## Design skills

### Core skills

Design-direction, taste and motion skills used on every section.

- [anthropics/skills](https://github.com/anthropics/skills) — **frontend-design** (Anthropic)
  - Why: forces a clear design direction before coding — purpose, tone,
    constraints and differentiation — so the result avoids generic/default
    AI styling.
  - When: use it to define the visual direction, before any code is written.
  - Install: `npx skills@latest add anthropics/skills -s frontend-design --agent claude-code --agent universal -y`

- [pbakaus/impeccable](https://github.com/pbakaus/impeccable) — **Impeccable** (Paul Bakaus)
  - Why: the primary anti-slop engine — 24 commands plus deterministic
    detector rules that run without an LLM; the taste layer.
  - When: `critique` on the first React build · `bolder` / `quieter` /
    `distill` / `delight` to adjust · `audit` + `polish` before catalog
    entry · detector rules in CI.
  - Its own installer, not the `skills` CLI. Run with no TTY it skips the
    "keep detected set or customize providers" prompt and falls back to
    whatever it auto-detected — which can include providers you don't use
    (e.g. `github`, for GitHub Copilot, writing `.github/skills/impeccable/`
    and `.github/hooks/impeccable.json`). Always pin `--providers` instead.
  - Install: `npx impeccable install --providers=claude,codex --scope=project -y`
    (valid providers: `claude`, `codex` → `.agents/`, `cursor`, `gemini`,
    `github`, `grok`, `hermes`, `opencode`, `pi`, `qoder`, `trae`,
    `trae-cn`, `rovo-dev`, `vibe`, `veto` — list only the ones you use)

- [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) — Vercel
  - **web-design-guidelines**
    - Why: the correctness layer — Vercel's interface rules for spacing,
      typography, interaction and accessibility.
    - When: every section, after the React build and before the audit.
  - **vercel-composition-patterns** (repo folder name is `composition-patterns`;
    "vercel-composition-patterns" is the catalog/display name)
    - Why: keeps React components flexible and maintainable by favoring
      composition over boolean-prop sprawl — compound components, lifted
      state, context, clear component APIs.
    - When: building or refactoring reusable React components, designing
      component APIs, working with compound components/context, or
      reviewing component architecture.
  - Install: `npx skills@latest add vercel-labs/agent-skills -s web-design-guidelines -s composition-patterns --agent claude-code --agent universal -y`

- [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) — **output-skill** ("Taste Skill", Leon Lin)
  - Why: blocks placeholders, skipped sections and half-finished files —
    enforces "complete runnable files".
  - When: always on during authoring, porting and prompt generation.
  - ⚠️ Unverified: no standalone repo literally named `output-skill` was
    found. It appears to be a sub-skill inside this repo (alongside
    soft-skill, minimalist-skill, brutalist-skill) rather than the repo
    root — confirm the exact skill name before installing.
  - Install (best guess, verify skill name first): `npx skills@latest add Leonxlnx/taste-skill -s output-skill --agent claude-code --agent universal -y`

- [emilkowalski/skill](https://github.com/emilkowalski/skill) — Emil Kowalski
  - **emil-design-eng**
    - Why: helps make better UI, interaction and motion decisions with a
      strong design-engineering approach.
    - When: while designing or refining a section, component or interaction.
  - **find-animation-opportunities**
    - Why: identifies where motion can improve clarity or delight and where
      animation should be avoided.
    - When: after the layout is ready, before deciding what to animate.
  - **animate**
    - Why: picks the curve, duration and properties for an animation from
      scratch.
    - When: when implementing a specific animation or transition.
  - **animation-vocabulary**
    - Why: helps describe motion clearly using the correct animation terms
      and effect names.
    - When: when writing prompts, specs, or explaining an animation idea.
  - **review-animations**
    - Why: reviews finished animations and finds issues with timing,
      easing, usability or overall polish.
    - When: after animation is implemented, before final approval.
  - Install: `npx skills@latest add emilkowalski/skill -s emil-design-eng -s find-animation-opportunities -s animate -s animation-vocabulary -s review-animations --agent claude-code --agent universal -y`

- [jakubkrehel/skills](https://github.com/jakubkrehel/skills) — **better-ui** (Jakub Krehel)
  - Why: improves the small visual details that make UI feel polished —
    radius, shadows, alignment, icons, hover states, surfaces and
    micro-interactions.
  - When: final visual pass on every section, before `/impeccable polish`.
  - Install: `npx skills@latest add jakubkrehel/skills -s better-ui --agent claude-code --agent universal -y`

- [owenob1/web-shaders-agent-kit](https://github.com/owenob1/web-shaders-agent-kit) — **web-shaders-agent-kit**
  - Why: prevents shaders that render black, redeclared uniforms, and
    color-space or aspect-ratio bugs.
  - When: every WebGL background (ogl or three.js), always together with
    the token-to-uniform pattern.
  - Not the `skills` CLI — no `npx` installer found. README instructs a
    manual copy instead (also ships Cursor rules, AGENTS.md, llms.txt, MCP
    config, and a system prompt for manual copy):
  - Install:

    ```bash
    mkdir -p .claude/skills
    cp -r skills/web-shaders .claude/skills/
    ```

    (path is best-effort from the README's install snippet — verify exact
    casing/path in the repo before relying on it)

### Situational skills

Installed, but only invoked when the task actually calls for them.

- [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) — **ui-ux-pro-max**
  - Why: a large library of styles, palettes and font pairings for
    exploring directions.
  - When: direction exploration and demo-theme design only — its palettes
    never enter section code.
  - Not the `skills` CLI — bespoke installer, and the maintainer says to
    always use it so you get the latest bundled templates (a static
    `SKILL.md` also exists in-repo but is not kept current).
  - Install:

    ```bash
    npm install -g ui-ux-pro-max-cli
    uipro init --ai claude
    ```

- [ibelick/ui-skills](https://github.com/ibelick/ui-skills) — **improve-ui**
  - Why: a second, read-only audit that writes fix plans for another agent.
  - When: when an Impeccable audit and a human review disagree; monthly
    catalog sweeps.
  - Not the `skills` CLI — bespoke `ui-skills` installer (site:
    ui-skills.com).
  - Install: `npx ui-skills get improve-ui` (inferred from the README's
    documented pattern `npx ui-skills get <skill>` — not seen verbatim for
    this exact skill, confirm before relying on it)

- Motion AI Kit — [motion.dev](https://motion.dev/docs/ai-kit) (Motion)
  - Why: gives the agent current Motion docs, examples, animation best
    practices, performance checks, and tools for tuning springs and
    transitions.
  - When: building, refining, debugging or optimizing animations with
    Motion.
  - Its own installer, not the `skills` CLI and not a plain GitHub repo —
    prompts for project vs. global and which agents to set up.
  - Install: `npx motion-ai`

- [better-auth/better-icons](https://github.com/better-auth/better-icons) — **Better Icons**
  - Why: search and fetch SVGs from 150+ Iconify collections from the
    agent.
  - When: a section needs an icon — inline the SVG with `currentColor`;
    never add an icon package.
  - Install: `npx skills@latest add better-auth/better-icons --agent claude-code --agent universal -y`

- [CloudAI-X/threejs-skills](https://github.com/CloudAI-X/threejs-skills) — **threejs-skills** (10 skills)
  - Why: accurate Three.js API references and patterns, audited against
    r160+.
  - When: only for sections that use three.js rather than ogl —
    `threejs-shaders` and `threejs-postprocessing` are the ones most
    commonly relevant.
  - Skills: `threejs-fundamentals`, `threejs-geometry`, `threejs-materials`,
    `threejs-lighting`, `threejs-textures`, `threejs-animation`,
    `threejs-loaders`, `threejs-shaders`, `threejs-postprocessing`,
    `threejs-interaction`.
  - Install (all ten): `npx skills@latest add CloudAI-X/threejs-skills --agent claude-code --agent universal -y`
  - Install (only shaders/postprocessing): `npx skills@latest add CloudAI-X/threejs-skills -s threejs-shaders -s threejs-postprocessing --agent claude-code --agent universal -y`

- [emilkowalski/skill](https://github.com/emilkowalski/skill) — Emil Kowalski (situational subset)
  - **improve-animations**
    - Why: audits all motion across a codebase and writes prioritized
      plans.
    - When: monthly catalog-wide motion sweep, not per section.
  - **prototype**
    - Why: builds several versions of a UI piece behind a switcher.
    - When: choosing between two or three directions for a flagship
      section, before the spec is final.
  - **apple-design**
    - Why: Apple's interface and fluid-motion principles, translated for
      the web.
    - When: reference for premium, restrained sections.
  - Install: `npx skills@latest add emilkowalski/skill -s improve-animations -s prototype -s apple-design --agent claude-code --agent universal -y`

- [jakubkrehel/skills](https://github.com/jakubkrehel/skills) — Jakub Krehel (situational subset)
  - **better-typography**, **better-layout**, **better-interface**
    - Why: focused passes on type, layout and interface detail from the
      same set as better-ui.
    - When: `critique` flags hierarchy, spacing or type-scale issues.
      Sections inherit host fonts, so tune scale and leading, not font
      choice.
  - **better-colors**
    - Why: color reasoning (OKLCH, neutrals, contrast).
    - When: only for designing demo themes and token mappings — never for
      section code.
  - **better-writing**
    - Why: interface copy quality.
    - When: writing named placeholder copy for each section.
  - **interface-review**
    - Why: a structured interface review.
    - When: second opinion when a section feels off but the audit passes.
  - Install: `npx skills@latest add jakubkrehel/skills -s better-typography -s better-layout -s better-interface -s better-colors -s better-writing -s interface-review --agent claude-code --agent universal -y`

## Code skills

### Core skills

React/code-quality skills used on every section, not just visual review.

- [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) — Vercel
  - **vercel-react-best-practices** (repo folder name is `react-best-practices`)
    - Why: 70 performance rules in 8 categories, ranked by impact
      (waterfalls and bundle size first, micro-optimizations last), each
      with incorrect vs correct code. Passed the Gen Agent Trust Hub,
      Socket and Snyk audits on skills.sh.
    - When: writing and reviewing platform code (RSC, data fetching,
      bundle size). For section code, only the client-side categories
      apply: re-renders, rendering performance and JS micro-optimizations.
  - **vercel-composition-patterns** (repo folder name is `composition-patterns`
    — same skill already listed under Design → Core; installing it once
    covers both uses)
    - Why: keeps React components flexible and maintainable by favoring
      composition over boolean-prop sprawl — compound components, lifted
      state, context, clear component APIs.
    - When: building or refactoring reusable React components, designing
      component APIs, working with compound components/context, or
      reviewing component architecture.
  - Install: `npx skills@latest add vercel-labs/agent-skills -s react-best-practices -s composition-patterns --agent claude-code --agent universal -y`

- [millionco/react-doctor](https://github.com/millionco/react-doctor) — **improve-react** (Million)
  - Why: read-only React audit. Runs React Doctor as evidence, ranks
    findings by leverage across bugs, performance, accessibility, security
    and maintainability, then writes self-contained fix plans into
    `plans/`. It never edits source.
  - When: before merging any React reference; before each platform
    release; `improve-react quick` for hot paths, `deep` for a quarterly
    sweep. `execute <plan>` runs a plan in an isolated worktree and checks
    the diff.
  - The skill itself installs via the generic `skills` CLI; at run time it
    separately shells out to `npx react-doctor@latest` for the underlying
    scan — no manual install needed for that, the skill invokes it on
    demand.
  - Install: `npx skills@latest add millionco/react-doctor -s improve-react --agent claude-code --agent universal -y`

## QA skills

### Core skills

Browser-verified checks run against real, rendered UI — used on every PR
that touches a section.

- [microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp) — **Playwright MCP** (Microsoft)
  - Why: gives the agent a real browser. It reads the page as an
    accessibility tree rather than pixels, resizes the viewport, takes
    screenshots, reads console errors and runs JS on the page.
    `--snapshot-boxes` adds each element's bounding box, which makes
    touch-target sizes measurable.
  - When: engine for every browser check below. Run in `--isolated` mode
    so tests never share state.
  - Not a skill — it's an MCP server, registered with `claude mcp add`,
    not installed with the `skills` CLI.
  - Install: `claude mcp add playwright npx @playwright/mcp@latest`
    (registers the server; append flags after a bare `--` to pass them
    through to the server itself, e.g. `-- --isolated --snapshot-boxes`.
    The README's own JSON-config example shows `--isolated` as a launch
    arg rather than the CLI form, so verify the `--` passthrough works
    before relying on it.)

- [OneRedOak/claude-code-workflows](https://github.com/OneRedOak/claude-code-workflows/tree/main/design-review) — **design-review workflow** (OneRedOak)
  - Why: a `/design-review` slash command and design-review subagent that
    look at the git diff, open the live UI through Playwright MCP, and
    grade it against principles stored in CLAUDE.md (hierarchy, WCAG AA+,
    responsive, interaction states), modeled on Stripe, Airbnb and Linear.
  - When: every PR that touches a section, after the design pass. Also
    before catalog entry.
  - Depends on Playwright MCP being registered first (README: "Uses
    Playwright MCP server integration to interact with and test actual UI
    components in real-time").
  - Not the `skills` CLI and no scripted installer — the `design-review/`
    folder is template files to copy in by hand:
    - `design-review-slash-command.md` → `.claude/commands/design-review.md`
    - `design-review-agent.md` → `.claude/agents/design-review.md`
    - `design-review-claude-md-snippet.md` → append into the project's
      `CLAUDE.md`
    - `design-principles-example.md` → reference/starting point for the
      principles section of that snippet
    (target paths are the standard Claude Code convention inferred from
    the filenames, not spelled out verbatim in the README — open each
    file and confirm before copying.)
  - Install: no CLI command; clone or download the repo and copy the four
    files above into place manually.
