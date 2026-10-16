# AGENTS.md

Instructions for AI coding agents (Claude Code, Copilot, Gemini CLI, Cursor, Codex, ...) and for humans.
This file is the index: project rules live here, and reusable know-how lives in skills under `.agents/skills/`.

## Project

<!-- Fill in: what this project does, who uses it, the main components. -->
_TBD_

## Stack

<!-- Fill in once decided, e.g. "Python 3.12, FastAPI, PostgreSQL". -->
_Not decided yet._ When the stack is decided, create its language skill (see **Language skills** below).

## Commands

<!-- Fill in the real commands. Agents must use these instead of guessing. -->
| Task | Command |
| --- | --- |
| Install | _TBD_ |
| Format | _TBD_ |
| Lint | _TBD_ |
| Test | _TBD_ |
| Full check (must pass before done) | _TBD_ |

## How we work: plan → implement → review → test

For any non-trivial change, follow `.agents/skills/dev-cycle/SKILL.md`:
1. **Plan**: write acceptance criteria and steps, then get the human's approval.
2. **Implement**: the plan, one step at a time.
3. **Review**: the diff, in a fresh subagent if you can.
4. **Test**: every criterion gets a test, and the full check passes.

Work is **done** only when the full check command passes and review has no open blocker or major findings.

## Skills

Skills follow the open [Agent Skills spec](https://agentskills.io/specification). If your agent doesn't load skills
automatically, read the matching `SKILL.md` before starting that kind of task.

### Role skills: how each role does its job

| Skill | Role | Use when |
| --- | --- | --- |
| [`dev-cycle`](.agents/skills/dev-cycle/SKILL.md) | orchestrator | building, adding, fixing or changing anything non-trivial |
| [`planning-changes`](.agents/skills/planning-changes/SKILL.md) | planner | before writing code; scoping or designing a change |
| [`implementing-plans`](.agents/skills/implementing-plans/SKILL.md) | implementer | executing an approved plan |
| [`reviewing-code`](.agents/skills/reviewing-code/SKILL.md) | reviewer | reviewing a diff or PR, and before declaring done |
| [`testing-changes`](.agents/skills/testing-changes/SKILL.md) | tester | proving acceptance criteria and running the full check |
| [`debugging-systematically`](.agents/skills/debugging-systematically/SKILL.md) | debugger | something fails and the cause isn't obvious |

If your agent supports subagents, run a role in a fresh subagent and tell it which skill to use. Pass it the goal, the plan,
the diff and the criteria.

### Repo skills: this repo's procedures

| Skill | Use when |
| --- | --- |
| [`writing-skills`](.agents/skills/writing-skills/SKILL.md) | adding or changing any skill, including the ones below |

<!-- Add repo skills here as procedures emerge (e.g. adding-endpoints, cutting-releases, running-migrations).
     Rule: the second time a multi-step procedure is explained, turn it into a repo skill. -->

### Language skills: conventions per language

_None yet._ Once the stack is decided, an agent should create `.agents/skills/<language>-conventions/SKILL.md` using the
`writing-skills` skill. Base it on the repo's real tooling, then list it here.

<!-- e.g. | [`python-conventions`](.agents/skills/python-conventions/SKILL.md) | editing any .py file | -->

## Rules

- Use the commands above. Don't invent alternatives.
- Stay in the scope of the task. Note follow-ups instead of doing drive-by changes.
- Never read, print or commit secrets (`.env`, keys). Reference environment variables by name.
- Ask before destructive commands, dependency installs, network calls, or writing outside the repo.
- Don't commit, push or open PRs unless asked.
- After adding or renaming a skill, update the Skills index above.
