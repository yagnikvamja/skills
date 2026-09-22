---
name: setup-third-party-skills
description: Suggest the third-party skills used across these projects and install the ones the user picks into the current project.
disable-model-invocation: true
---

## 1. List the third-party skills

Read [`THIRD-PARTY.md`](./THIRD-PARTY.md). It groups skills by their source
repository. Present the full list to the user, grouped the same way.

## 2. Ask which to install

Ask the user which of the listed skills they want installed in this project
(multi-select). Don't install anything they didn't pick.

## 3. Install the picked skills

For each source repository that has at least one picked skill, run:

```shell
npx skills@latest add <owner>/<repo>
```

Follow its prompts: pick the specific skill(s) the user selected from that
repo, and the coding agent(s) to install them for. Repeat once per distinct
source repository with a pick.

## 4. Confirm

Verify the picks landed — check `skills-lock.json` and the target
`.agents/skills/` (or equivalent) directory — and report what was installed.
