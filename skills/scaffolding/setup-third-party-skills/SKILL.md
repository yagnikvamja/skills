---
name: setup-third-party-skills
description: Suggest the third-party skills used across these projects and install the ones the user picks into the current project.
disable-model-invocation: true
---

## 1. List the third-party skills

Read [`THIRD-PARTY.md`](./THIRD-PARTY.md) **in full, every time** — new
categories get appended to it over time (at the time of writing: Design
skills, Code skills, QA skills, and more may exist by the time you read
this). Do not rely on any category names mentioned elsewhere, including in
this file — always derive the actual list of categories and tiers by
scanning the file's current headings. It groups skills into categories at
the `##` level, and within each category splits skills into **Core** (used
on every section) and **Situational** (installed, but only invoked when the
task calls for it) at the `###` level — a category may only have a Core tier
if no Situational skills exist for it yet. Within each tier, skills are
grouped by their source, each with the `Install` command that entry actually
needs. Present the full file to the user, grouped the same way: category by
category, tier by tier within each — every `##` category found in the file,
not just the ones this skill's instructions happen to mention by name.

## 2. Ask which to install

Ask which skills to install as **one multi-select question per `###` tier
that exists in the file right now** — e.g. "which Design → Core skills?",
"which Design → Situational skills?", "which Code → Core skills?", "which QA
→ Core skills?", and one more per any further category/tier appended to the
file later. Never merge two tiers (even within the same category) or two
categories into one question: a user who wants every Design-Core skill may
still want only a couple of Situational ones, and a user working on QA may
want all of Code but none of Design. One question per `###` tier keeps those
choices independent.

The `AskUserQuestion` tool caps out at 4 questions per call. If the file
currently has 4 or fewer tiers, ask them all in a single call. **If it has
more than 4, split across multiple `AskUserQuestion` calls** rather than
dropping any tier — silently skipping the tiers that don't fit is the exact
failure this note exists to prevent.

A skill that is cross-listed under more than one tier (e.g.
`vercel-composition-patterns` appears under both Design → Core and Code →
Core because it serves both purposes) is still just one skill — picking it
in either question is enough; don't ask about it twice or treat the two
listings as separate installs.

**Default target is `.claude` and `.agents` only — install there without
asking.** Separately ask whether they also want any *other* coding agent
(e.g. `cursor`, `gemini`, `github` for Copilot, `codex`'s own non-`.agents`
manifest, etc.) — only add one if they explicitly say so. Don't install
anything, skill or agent, they didn't pick.

## 3. Install the picked skills

Most sources are plain GitHub skill repos the `skills` CLI can add directly;
a few ship their own installer instead. **Run each source's `Install`
command from `THIRD-PARTY.md` exactly as written — do not assume
`npx skills@latest add <owner>/<repo>` works for every source.** Both
default commands already target `.claude` + `.agents` only; append flags
for any extra agent the user asked for in step 2.

- **For a `skills` CLI source, always pass `-a`/`--agent` explicitly** —
  once per agent, e.g. `--agent claude-code --agent universal`
  (comma-separated, `--agent claude-code,universal`, is read as one invalid
  name and fails). Running the command through Bash gives the CLI no TTY,
  so it prints `Agent detected — installing non-interactively` and silently
  installs for the running agent only, skipping `.agents/skills/`
  (`universal`) entirely unless it's named explicitly. `claude-code` +
  `universal` is the default pair (`.claude/skills/` + `.agents/skills/`);
  add more agent flags only for what the user asked for in step 2. Also
  pass `-s`/`--skill` for the specific skill(s) picked from that source,
  and `-y` to skip the now-redundant confirmation prompt.
- For a source with its own installer, use the exact flags given in its
  `Install` command — none of these run interactively either (no TTY in
  Bash), so any installer that normally prompts will instead silently fall
  back to an auto-detected default rather than asking. That default can
  include tools the user doesn't use (e.g. impeccable defaulting in
  `github`, installing into `.github/` for Copilot). Never assume the bare
  command from THIRD-PARTY.md is enough — check its flags actually pin the
  target tools (default: `.claude` + `.agents` only) before running it, and
  add a flag for any extra agent the user asked for.
- Some entries aren't a single runnable command at all — an MCP server
  registered via `claude mcp add` rather than the `skills` CLI (e.g.
  Playwright MCP), or template files meant to be copied by hand into
  `.claude/commands/`, `.claude/agents/` or `CLAUDE.md` (e.g. the
  design-review workflow). Follow that entry's own instructions in
  THIRD-PARTY.md literally — fetch/clone the repo if needed, then run the
  registration command or copy the named files to the named paths — instead
  of forcing it through the `skills` CLI pattern.

Repeat once per distinct source that has a pick. **A source that shows up in
more than one tier or category** (e.g. `emilkowalski/skill` under Design →
Core and Design → Situational, or `vercel-labs/agent-skills` under Design →
Core and Code → Core) gets one combined install command — merge the `-s`
flags from every tier/category it was picked from rather than running the
source's install more than once, and de-duplicate any `-s` flag for a skill
that was cross-listed (like `composition-patterns`) so it isn't passed twice.

## 4. Confirm

Verify the picks landed — check `skills-lock.json` and the target
`.agents/skills/` (or equivalent) directory — and report what was installed.
