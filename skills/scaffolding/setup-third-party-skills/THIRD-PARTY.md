# Third-Party Skills & AI Resources

> Collection of third-party skills and AI resources that I mostly use in my projects.
>
> Each entry's `Install` command is the one to run — most third-party skills
> are plain GitHub repos the `skills` CLI can add directly, but a few ship
> their own installer instead, and those must use it.
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
  - Install: `npx skills@latest add emilkowalski/skill`
- [better-icons](https://github.com/better-auth/better-icons)
  - Install: `npx skills@latest add better-auth/better-icons`
- [impeccable](https://github.com/pbakaus/impeccable)
  - Its own installer, not the `skills` CLI — detects your AI tool and
    installs accordingly.
  - Install: `npx impeccable install`
- [motion](https://motion.dev/docs/ai-kit)
  - Its own installer, not the `skills` CLI and not a plain GitHub repo —
    prompts for project vs. global and which agents to set up.
  - Install: `npx motion-ai`
