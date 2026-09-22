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

- [emilkowalski/skill](https://github.com/emilkowalski/skill)
  - animation-vocabulary
  - emil-design-eng
  - find-animation-opportunities
  - prototype
  - review-animations
  - Install: `npx skills@latest add emilkowalski/skill --agent claude-code --agent universal -y`
- [better-icons](https://github.com/better-auth/better-icons)
  - Install: `npx skills@latest add better-auth/better-icons --agent claude-code --agent universal -y`
- [impeccable](https://github.com/pbakaus/impeccable)
  - Its own installer, not the `skills` CLI. Run with no TTY it skips the
    "keep detected set or customize providers" prompt and falls back to
    whatever it auto-detected — which can include providers you don't use
    (e.g. `github`, for GitHub Copilot, writing `.github/skills/impeccable/`
    and `.github/hooks/impeccable.json`). Always pin `--providers` instead.
  - Install: `npx impeccable install --providers=claude,codex --scope=project -y`
    (valid providers: `claude`, `codex` → `.agents/`, `cursor`, `gemini`,
    `github`, `grok`, `hermes`, `opencode`, `pi`, `qoder`, `trae`,
    `trae-cn`, `rovo-dev`, `vibe`, `veto` — list only the ones you use)
- [motion](https://motion.dev/docs/ai-kit)
  - Its own installer, not the `skills` CLI and not a plain GitHub repo —
    prompts for project vs. global and which agents to set up.
  - Install: `npx motion-ai`
