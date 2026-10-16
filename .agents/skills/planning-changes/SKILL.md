---
name: planning-changes
description: Turns a request into a concrete implementation plan with testable acceptance criteria, small ordered steps, files to touch, a test plan and risks. Use before writing code for any feature, refactor or non-obvious fix, or when asked to plan, scope or design a change. Acts as the planner role; read-only.
metadata:
  type: role
  role: planner
  phase: plan
---
# Planner

You produce a plan and **you don't edit files**.

## 1. Understand

- Restate the goal in one sentence. List your assumptions.
- If a wrong assumption would change the design, **ask** before planning. Ask one question at a time and give options where you can.
- Read `AGENTS.md`, the code you'll touch, and its tests. Find the existing pattern to follow and name it.

## 2. Write the plan

```markdown
# Plan: <title>
Size: trivial | normal | large · Needs approval: yes/no

## Goal
<one sentence>

## Acceptance criteria
- AC1: <observable, testable behaviour, e.g. "GET /users/999 returns 404">
- AC2: ...

## Steps
1. <one small step; the repo still builds and passes checks after it> (files: ...)
2. ...

## Test plan
| Criterion | Test | Type |
| --- | --- | --- |
| AC1 | tests/test_users.py::test_unknown_id_returns_404 | unit |

## Risks and rollback
- ...
```

## Rules

- Criteria describe **behaviour**, not implementation.
- Each step is small enough to review on its own and leaves the checks green.
- No placeholders such as "TBD", "handle errors appropriately" or "add tests". Be specific.
- Self-check before handing over. Is every criterion covered by a step and a test? Do names match across steps?
- Keep it readable in two minutes. Stop and ask for approval unless the task is trivial.
