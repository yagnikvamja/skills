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

Ask the user which of the listed skills they want installed (multi-select),
and which coding agent(s) to install them for. Don't install anything they
didn't pick.

## 3. Install the picked skills

Most sources are plain GitHub skill repos the `skills` CLI can add directly;
a few ship their own installer instead. **Run each source's `Install`
command from `THIRD-PARTY.md` exactly as written — do not assume
`npx skills@latest add <owner>/<repo>` works for every source.**

- **For a `skills` CLI source, always pass `-a`/`--agent` explicitly** —
  once per agent, e.g. `--agent claude-code --agent universal`
  (comma-separated, `--agent claude-code,universal`, is read as one invalid
  name and fails). Running the command through Bash gives the CLI no TTY,
  so it prints `Agent detected — installing non-interactively` and silently
  installs for the running agent only, skipping `.agents/skills/`
  (`universal`) entirely unless it's named explicitly. Include `universal`
  in every run so the skill also lands in the shared `.agents/skills/`
  location, plus whichever agent(s) the user picked (e.g. `claude-code`,
  `codex`). Also pass `-s`/`--skill` for the specific skill(s) picked from
  that source, and `-y` to skip the now-redundant confirmation prompt.
- For a source with its own installer, follow that installer's own prompts
  instead — it does not take `--agent`.

Repeat once per distinct source that has a pick.

## 4. Confirm

Verify the picks landed — check `skills-lock.json` and the target
`.agents/skills/` (or equivalent) directory — and report what was installed.
