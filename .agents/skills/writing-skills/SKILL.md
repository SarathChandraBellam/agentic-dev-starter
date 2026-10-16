---
name: writing-skills
description: Creates or updates a skill in .agents/skills following the open Agent Skills spec. Use when the project's language or stack is decided (create its language skill), when a repo procedure is repeated or explained twice (create a repo skill), when a new role is needed (create a role skill), or when asked to add, write or improve a skill.
metadata:
  type: meta
---
# Writing skills

Skills live in `.agents/skills/<name>/SKILL.md` and follow the open [Agent Skills spec](https://agentskills.io/specification).
After adding or renaming a skill, **add it to the Skills index in `AGENTS.md`**. That index is how every agent finds it.

## Pick the type

| Type | Encodes | Name pattern | Template |
| --- | --- | --- | --- |
| **Role** | *How* a role does its job: planner, reviewer, tester, ... | verb-ing phrase (`reviewing-code`) | [assets/role-skill.md](assets/role-skill.md) |
| **Repo** | *This repo's* procedures: release, add an endpoint, migrations | verb-ing phrase (`adding-endpoints`) | [assets/repo-skill.md](assets/repo-skill.md) |
| **Language** | Conventions for one language or framework | `<language>-conventions` | [assets/language-skill.md](assets/language-skill.md) |

Where else knowledge can go:
- A one-line rule that applies to every task belongs in `AGENTS.md`, not in a skill.
- A rule for one directory belongs in a nested `AGENTS.md` in that directory.

## When to create each type

- **Language skill:** create it as soon as the stack is chosen (for example "we'll use Python + FastAPI"). Read the repo's actual
  tooling first (`pyproject.toml`, `package.json`, linter configs). Then write the conventions that are *true for this repo*:
  layout, commands, typing, error handling, test idioms. Make one skill per language and add framework sections inside it.
- **Repo skill:** create it the second time you have to explain or rediscover the same multi-step procedure.
- **Role skill:** create it when a recurring kind of work needs its own checklist and output format, for example a migrations
  reviewer or an accessibility auditor.

## Spec rules (validated by tools; keep them)

- `name`: 1–64 characters, lowercase letters, digits and hyphens, no leading, trailing or double hyphens. It **must equal the folder name**.
- `description`: up to 1024 characters. Say **what it does and when to use it**, and front-load the trigger words. Agents choose
  skills from this line alone.
- Optional fields: `license`, `compatibility`, `metadata` (string → string, here `type: role|repo|language`), and `allowed-tools`.
- Keep the body under about 500 lines. Move detail into `references/*.md`, one level deep, and link to it with a relative path.
  Templates go in `assets/` and runnable helpers in `scripts/`.

## Quality bar

- Be concrete: real paths and the real commands from `AGENTS.md`.
- One job per skill. If the description needs "and also", split it.
- Write imperative steps, give an example of the expected output, and name the common pitfalls.
- No secrets. Don't instruct agents to download and run remote code.
