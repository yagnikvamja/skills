# Sparkle UI Skills

A collection of skills for generating premium landing-page UI, sections and
for bootstrapping third-party skills into a project.

### [scaffolding/setup-third-party-skills](skills/scaffolding/setup-third-party-skills/SKILL.md)

Lists the third-party skills tracked in
[`THIRD-PARTY.md`](skills/scaffolding/setup-third-party-skills/THIRD-PARTY.md)
and installs the ones the user picks into the current project. Defaults to
installing into `.claude` and `.agents` only, and only adds other coding
agents (Cursor, Gemini, GitHub Copilot, etc.) when explicitly requested.

## Repository layout

```text
skills/
  sparkleui/
    SKILL.md               # entry point and read order
    references/
      design.md             # creative direction & block anatomy
      media-skill.md         # media/motion effects
      rules.md               # execution & output-format rules
  scaffolding/
    setup-third-party-skills/
      SKILL.md               # installer flow
      THIRD-PARTY.md          # catalog of third-party skills & install commands
```
