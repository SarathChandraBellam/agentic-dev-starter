---
name: dev-cycle
description: Runs the full plan → implement → review → test loop for a change until checks pass. Use when the user asks to build, add, fix or change something non-trivial, or says "dev cycle", "full cycle", or "do it properly".
metadata:
  type: workflow
---
# Dev cycle: plan → implement → review → test

Drive a change through four phases. Each phase has a **role skill**. Load it and follow it.

| Phase | Role | Skill | Exit gate |
| --- | --- | --- | --- |
| 1. Plan | planner | `planning-changes` | A plan with acceptance criteria, approved by the human for anything non-trivial |
| 2. Implement | implementer | `implementing-plans` | Every plan step done, format and lint pass |
| 3. Review | reviewer | `reviewing-code` | No open blocker or major findings |
| 4. Test | tester | `testing-changes` | Every acceptance criterion has a passing test, and the full check command passes |

When something fails and the cause isn't obvious, switch to `debugging-systematically`, then return to the phase you were in.

## Running the roles

- **If your agent can start subagents** (Claude Code, Codex, Gemini CLI, Cursor and Copilot all can), run **Review** in a
  fresh subagent and tell it to use the `reviewing-code` skill. A reviewer that didn't write the code catches more. Planning
  and testing may also run in subagents. Pass each subagent the goal, the plan, the diff or file paths, and the acceptance
  criteria, because it can't see this conversation.
- **Otherwise** run the phases yourself in order, and re-read the diff from scratch before reviewing it.

## Loop rules

1. Size the task first:
   - **Trivial** (typo, one-liner): skip the written plan, then implement, self-review and run checks.
   - **Normal**: put the plan in the chat and wait for approval.
   - **Large or risky** (cross-module, migrations, public API, security): write the plan to `.agents/work/<task-slug>/plan.md`
     and wait for approval.
2. Any blocker or major review finding, or any failing test, sends you back to **Implement**.
3. After **3 loops** without converging, stop. Summarise what's blocking and ask the human. The plan is probably wrong.
4. Never skip the gate checks. "Should work" is not done.

## Finish with

- **What changed**, as files and behaviour.
- **Why**, linked to the acceptance criteria.
- **How it was verified**: the commands you ran and the tail of their output.
- **Follow-ups**: anything out of scope that you noticed.
