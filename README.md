# agentic-dev-starter

A minimal GitHub template that makes a new repo ready for AI-assisted development, focused on the **developer** and
**QA** personas, with quality built and proven **inside the repo** before merge.

It's built only on the open standards the tools are converging on:

- **[AGENTS.md](https://agents.md)**: the single index for project rules, quality gates, agents and skills.
- **[Agent Skills](https://agentskills.io/specification)**: reusable know-how in `.agents/skills/<name>/SKILL.md`.
- **Subagents**: thin persona files in `.agents/agents/` that the main agent dispatches as an orchestrator.

There are no CLI-specific folders, generators or sync scripts.

```
AGENTS.md            # index: project, commands, quality gates, agents + tiers, skills
.agents/
  agents/            # role-explorer · role-planner · role-qa-engineer · role-developer · role-code-reviewer ·
                     # role-security-reviewer · role-debugger · repo-domain-expert  (stack-* agents added per stack)
  skills/            # role-* (11) · repo-* (4: intent, knowledge, review rules, writing skills) · stack-* (created when the stack is chosen)
.github/
  workflows/ci.yml   # auto-detects Python/TS manifests and calls shared reusable CI workflows
  dependabot.yml     # weekly grouped updates for github-actions (+ commented uv / npm blocks)
  ISSUE_TEMPLATE/    # feature_spec.yml · bug_report.yml · config.yml
  PULL_REQUEST_TEMPLATE.md
specs/               # per-change artifacts: spec.md · plan.md · test-cases.md · qa-report.md
```

## The lifecycle

The lifecycle is adapted from AWS's AI-Driven Development Lifecycle, Spec Kit and Kiro: the AI proposes, the human approves at gates,
and the artifacts live in the repo.

| Phase | Agents | Output | Gate |
| --- | --- | --- | --- |
| **Inception** | explorer → planner → qa-engineer (design) | spec with Given/When/Then criteria, plan, **test cases before code** | Gate 1: human approves |
| **Construction** | developer (test-first) → code-reviewer (+ security) → qa-engineer (verify) | code, tests, findings, **QA report** | Gate 2: human reviews and merges |

Two design rules come from what has worked in practice:
- **One writer at a time.** Read-only agents (explorer, reviewers) may run in parallel.
- **Review in a clean context**, by an agent that didn't write the code.

## Personas and model tiers

Agents declare a tier (`strong`, `balanced` or `fast`), not a model name, so the same files work under any orchestrator.
`AGENTS.md` maps the tiers:
- **strong** for planning, review and security,
- **balanced** for the developer and QA,
- **fast** for read-only exploration,
- **inherit** when unsure.

## Quick start

1. Click **Use this template**, then clone.
2. Fill in **Project**, **Stack**, **Commands** and **Quality gates** in `AGENTS.md`, then the three repo skills
   (`repo-project-intent`, `repo-domain-knowledge`, `repo-review-rules`), or ask an agent to draft them for you to approve.
3. Ask: *"Our stack is Python + FastAPI + pytest. Create the stack skills."* (`repo-writing-skills` creates `stack-python`, and `stack-fastapi` if needed.)
4. Ask: *"Use role-running-dev-cycle to add rate limiting to the login endpoint."*

## GitHub workflows & templates

The `.github/` folder comes ready for repos spawned from this template:

- **CI (`.github/workflows/ci.yml`)**: runs on pushes to `main` and pull requests, detects `pyproject.toml`/`requirements.txt` and `package.json`, and conditionally calls the shared reusable Python and TypeScript CI workflows (`SarathChandraBellam/.github`). Works out of the box on empty, Python, TypeScript, or polyglot repos.
- **Dependabot (`.github/dependabot.yml`)**: active weekly grouped updates for `github-actions`, with ready-to-uncomment grouped blocks for `uv` (Python) and `npm` (TypeScript) once you pick a stack.
- **Issue Forms (`.github/ISSUE_TEMPLATE/`)**: structured forms for feature/spec proposals (`feature_spec.yml`, feeding directly into `specs/<id>-<slug>/spec.md`) and bug reports (`bug_report.yml`), with blank issues enabled.
- **PR template (`.github/PULL_REQUEST_TEMPLATE.md`)**: captures summary, spec/issue references, QA gate evidence from `qa-report.md`, and the test/review/security checklist.

## Compatibility

| Agent | Reads `AGENTS.md` | Discovers `.agents/skills` | Subagents |
| --- | --- | --- | --- |
| Codex CLI | ✅ | ✅ | ✅ generic subagents |
| GitHub Copilot CLI | ✅ | ✅ | ✅ |
| Cursor | ✅ | ✅ | ✅ |
| Gemini CLI | ⚠️ set `context.fileName: ["AGENTS.md"]` once | ✅ | ✅ |
| Claude Code | ✅ (no `CLAUDE.md` present) | ⚠️ via the index in `AGENTS.md` ([#31005](https://github.com/anthropics/claude-code/issues/31005)) | ✅ |

`.agents/agents/` is a convention, not yet a standard. The orchestrator reads the file and passes it to a generic subagent, which
works in every CLI above. To also register agents natively, copy or symlink them into your CLI's agents folder locally.

## Credits

- [AWS AI-DLC](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle) and [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows): inception and construction phases, approval gates, artifacts in the repo.
- [GitHub Spec Kit](https://github.com/github/spec-kit) and [Kiro specs](https://kiro.dev/docs/specs/index): spec → plan → tasks.
- [obra/superpowers](https://github.com/obra/superpowers) (MIT): writing plans, TDD, systematic debugging, verification before completion.
- [Anthropic: Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) and [multi-agent research](https://www.anthropic.com/engineering/multi-agent-research-system): progressive disclosure, delegation briefs, effort scaling.
- [Cognition: multi-agents that work](https://cognition.com/blog/multi-agents-working): clean-context review, read-only subagents, a single writer.
- [OpenAI Codex best practices](https://developers.openai.com/codex/learn/best-practices): a short AGENTS.md, skills with one job each, subagents for bounded work.
