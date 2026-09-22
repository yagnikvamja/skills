---
name: setup-third-party-skills
description: Suggest the third-party skills used across these projects and install the ones the user picks into the current project.
disable-model-invocation: true
---

## 1. List the third-party skills

Read [`THIRD-PARTY.md`](./THIRD-PARTY.md). It groups skills by their source,
each with the `Install` command that entry actually needs. Present the full
list to the user, grouped the same way.

## 2. Ask which to install

Ask the user which of the listed skills they want installed (multi-select).

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

Repeat once per distinct source that has a pick.

## 4. Confirm

Verify the picks landed — check `skills-lock.json` and the target
`.agents/skills/` (or equivalent) directory — and report what was installed.
