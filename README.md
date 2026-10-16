# agentic-dev-starter

A minimal GitHub template that makes any new repo ready for AI coding agents, built only on the two open
standards the tools are converging on:

- **[AGENTS.md](https://agents.md)**: one instruction file at the root, the index for everything.
- **[Agent Skills](https://agentskills.io/specification)**: reusable know-how in `.agents/skills/<name>/SKILL.md`.

There are no CLI-specific folders, no generators and no sync scripts.

```
AGENTS.md                  # project rules + commands + the skills index (you own this)
.agents/
  skills/
    dev-cycle/             # orchestrates plan → implement → review → test
    planning-changes/      # role: planner
    implementing-plans/    # role: implementer
    reviewing-code/        # role: reviewer (+ references/security-checklist.md)
    testing-changes/       # role: tester
    debugging-systematically/  # role: debugger
    writing-skills/        # repo: how to add skills (+ templates for role, repo and language skills)
  work/                    # git-ignored scratch space for plans
```

## Quick start

1. Click **Use this template**, then clone.
2. Fill in **Project**, **Stack** and **Commands** in `AGENTS.md`.
3. Ask your agent: *"We're using Python + FastAPI. Create the language skill."* It uses `writing-skills` to create
   `.agents/skills/python-conventions/` from your real tooling and adds it to the index.
4. Build things: *"Use the dev-cycle skill to add a health-check endpoint."*

## Three kinds of skills

| Type | What it holds | Shipped | Grows when |
| --- | --- | --- | --- |
| **Role** | How a role does its job, with a defined output | planner, implementer, reviewer, tester, debugger (+ `dev-cycle`) | a recurring kind of work needs its own checklist |
| **Repo** | This repo's procedures (release, migrations, endpoints) | `writing-skills` | a procedure is explained a second time |
| **Language** | Conventions for the chosen language or framework | none. The agent creates one when you pick the stack | a new language enters the repo |

Roles are skills rather than CLI-specific agent files, so they work everywhere. If a CLI supports subagents, it runs the role in
a fresh subagent with that skill. If not, the agent follows the skill itself. `AGENTS.md` says when to do which.

## How each agent picks it up

| Agent | Reads `AGENTS.md` | Discovers `.agents/skills` |
| --- | --- | --- |
| Codex CLI | ✅ | ✅ |
| GitHub Copilot CLI | ✅ | ✅ |
| Cursor | ✅ | ✅ |
| Gemini CLI | ⚠️ reads `GEMINI.md` by default, so set `"context": {"fileName": ["AGENTS.md"]}` once in your Gemini settings | ✅ |
| Claude Code | ✅ when there's no `CLAUDE.md` (v2.1.277+) | ⚠️ not yet ([issue #31005](https://github.com/anthropics/claude-code/issues/31005)), so it follows the index in `AGENTS.md` |

The skills index in `AGENTS.md` is what keeps this portable. An agent that doesn't auto-discover skills still sees the list,
with paths and "use when" triggers, and reads the `SKILL.md` it needs. When Claude Code and Gemini adopt the standard defaults,
nothing in this repo has to change.

## Adding a skill

Ask your agent to "create a repo skill for cutting releases", or do it by hand:

1. Copy a template from `.agents/skills/writing-skills/assets/` to `.agents/skills/<name>/SKILL.md`.
2. Keep `name` equal to the folder name, and write a `description` that says what the skill does and when to use it.
3. Add a row to the Skills index in `AGENTS.md`.

Optionally validate with [`skills-ref`](https://github.com/agentskills/agentskills/tree/main/skills-ref) (`skills-ref validate .agents/skills/<name>`).
To pull in community skills, the open-source [`npx skills`](https://github.com/vercel-labs/skills) installer writes to `.agents/skills/`.

## Credits and further reading

- [Agent Skills specification](https://agentskills.io/specification), the format these skills follow.
- [obra/superpowers](https://github.com/obra/superpowers) (MIT): the plan → execute → review → verify workflow and systematic
  debugging that inspired the role skills.
- [anthropics/skills](https://github.com/anthropics/skills) (Apache-2.0): skill structure and the skill template.
- [VoltAgent/awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills): official skills from Sentry, Trail of Bits,
  Microsoft, Vercel and others, to add as repo or role skills.
