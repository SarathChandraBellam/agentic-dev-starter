# AGENTS.md

Instructions for AI coding agents (Claude Code, Codex, Copilot, Gemini CLI, Cursor, ...) and for humans.
This file is the **index**. Project rules live here, personas live in `.agents/agents/`, and know-how lives in `.agents/skills/`.

## Project

<!-- Fill in: what this project does, who uses it, the main components. -->
_TBD_

## Stack

<!-- Fill in once decided, e.g. "Python 3.12, FastAPI, PostgreSQL". -->
_Not decided yet._ When it's decided, create the language skill (see **Language skills**).

## Commands

<!-- Fill in the real commands. Agents use these and never guess. -->
| Task | Command |
| --- | --- |
| Install | _TBD_ |
| Run locally | _TBD_ |
| Format | _TBD_ |
| Test (all) | _TBD_ |

## Quality gates

These run **in the repo, before merge**. `verifying-changes` runs all of them, and work is done only when all pass.

| Gate | Command | Required |
| --- | --- | --- |
| Format check | _TBD_ | yes |
| Lint | _TBD_ | yes |
| Type check | _TBD_ | if the language has one |
| Unit tests | _TBD_ | yes |
| Integration tests | _TBD_ | yes, once there are integration points |
| Coverage of changed lines | _TBD_ | target: _TBD_ |
| Dependency / secret scan | _TBD_ | recommended |

## How we work

For anything non-trivial, the main agent acts as **orchestrator** and follows
[`dev-cycle`](.agents/skills/dev-cycle/SKILL.md):

1. **Inception**: `explorer` maps the code, `planner` writes the spec and plan, and `qa-engineer` designs test cases from the
   acceptance criteria. **Gate 1:** the human approves.
2. **Construction**: `developer` implements test-first, `code-reviewer` (and `security-reviewer` when relevant) reviews in a clean
   context, and `qa-engineer` verifies all quality gates. **Gate 2:** the human reviews the QA report and merges.

Artifacts live in `specs/<id>-<slug>/`: `spec.md`, `plan.md`, `test-cases.md`, `qa-report.md`.
Trivial changes skip the specs and still pass the quality gates.

## Agents (subagents the orchestrator dispatches)

To dispatch an agent, read `.agents/agents/<name>.md` and start a subagent with its body as instructions. If your CLI has no
subagents, play the role yourself. Only **one writer at a time**. Read-only agents may run in parallel.

| Agent | Phase | Tier | Access | Skills |
| --- | --- | --- | --- | --- |
| [`explorer`](.agents/agents/explorer.md) | inception | fast | read-only | exploring-codebase |
| [`planner`](.agents/agents/planner.md) | inception | strong | read-only | writing-specs, planning-changes |
| [`qa-engineer`](.agents/agents/qa-engineer.md) | inception + construction | balanced | tests only | designing-test-cases, writing-tests, verifying-changes, fixing-flaky-tests |
| [`developer`](.agents/agents/developer.md) | construction | balanced | read-write | implementing-plans, writing-tests |
| [`code-reviewer`](.agents/agents/code-reviewer.md) | construction | strong | read-only | reviewing-code |
| [`security-reviewer`](.agents/agents/security-reviewer.md) | construction | strong | read-only | reviewing-code |
| [`debugger`](.agents/agents/debugger.md) | any | strong | read-write | debugging-systematically, writing-tests |

### Model tiers

Agents declare a **tier**, not a model name. Map each tier to whatever your orchestrator offers:

| Tier | Use for | Pick |
| --- | --- | --- |
| **strong** | judgment: planning, review, security, hard debugging | your most capable reasoning model, ideally from a different model family than the developer for reviews |
| **balanced** | volume work with clear instructions: implementation, tests | your default coding model, or inherit the session model |
| **fast** | read-only lookups, run many in parallel | your smallest, fastest model |

If unsure, **inherit the orchestrator's model**. Escalate a task to **strong** after a failed attempt.

## Skills

Skills follow the open [Agent Skills spec](https://agentskills.io/specification). If your agent doesn't load them automatically,
read the matching `SKILL.md` before that kind of task.

### Role skills: how each persona does its job

| Skill | Persona | Use when |
| --- | --- | --- |
| [`dev-cycle`](.agents/skills/dev-cycle/SKILL.md) | orchestrator | building, adding, fixing or changing anything non-trivial |
| [`exploring-codebase`](.agents/skills/exploring-codebase/SKILL.md) | explorer | "where is", "how does", or before touching unfamiliar code |
| [`writing-specs`](.agents/skills/writing-specs/SKILL.md) | planner | requirements, acceptance criteria, user stories |
| [`planning-changes`](.agents/skills/planning-changes/SKILL.md) | planner | design notes, task breakdown, test strategy |
| [`designing-test-cases`](.agents/skills/designing-test-cases/SKILL.md) | qa-engineer | test cases from acceptance criteria, before code |
| [`implementing-plans`](.agents/skills/implementing-plans/SKILL.md) | developer | executing an approved plan, test-first |
| [`writing-tests`](.agents/skills/writing-tests/SKILL.md) | qa-engineer, developer | automating test cases, regression tests |
| [`reviewing-code`](.agents/skills/reviewing-code/SKILL.md) | code-reviewer, security-reviewer | reviewing a diff or PR |
| [`verifying-changes`](.agents/skills/verifying-changes/SKILL.md) | qa-engineer | the final in-repo QA gate, before done or a PR |
| [`fixing-flaky-tests`](.agents/skills/fixing-flaky-tests/SKILL.md) | qa-engineer | intermittent or CI-only test failures |
| [`debugging-systematically`](.agents/skills/debugging-systematically/SKILL.md) | debugger | failures whose cause isn't obvious |

### Repo skills: this repo's procedures

| Skill | Use when |
| --- | --- |
| [`writing-skills`](.agents/skills/writing-skills/SKILL.md) | adding or changing a skill or an agent |

<!-- Add repo skills as procedures emerge (e.g. adding-endpoints, running-migrations, cutting-releases).
     Rule: the second time a multi-step procedure is explained, make it a repo skill. -->

### Language skills: conventions per language

_None yet._ Once the stack is decided, an agent creates `.agents/skills/<language>-conventions/SKILL.md` with `writing-skills`,
based on the repo's real tooling, and adds it here.

<!-- e.g. | [`python-conventions`](.agents/skills/python-conventions/SKILL.md) | editing any .py file | -->

## Rules

- Use the commands and gates above. Don't invent alternatives.
- Stay in scope. Note follow-ups instead of making drive-by changes.
- Never read, print or commit secrets (`.env`, keys). Reference environment variables by name.
- Ask before destructive commands, dependency installs, network calls, or writing outside the repo.
- Don't commit, push or open PRs unless asked.
- When you add or rename a skill or agent, update the tables above.
