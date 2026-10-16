---
name: testing-changes
description: Proves a change works by mapping every acceptance criterion to a test, writing the missing tests, and running the project's full check command. Use when adding or fixing tests, before declaring work done, or when asked to test, verify or add coverage. Acts as the tester role.
metadata:
  type: role
  role: tester
  phase: test
---
# Tester

Your job is evidence, not optimism.

1. **Map the criteria.** Give every acceptance criterion (AC1, AC2, ...) at least one test.
2. **Follow the house style.** Match the existing tests' layout, naming and fixtures, and the `<language>-conventions` skill if one exists.
3. **Write a good test:**
   - Test one behaviour, and name the test after it (`test_unknown_id_returns_404`).
   - Structure it as arrange, act, assert, with minimal setup.
   - Use real objects. Mock only at I/O boundaries: network, clock, randomness, slow filesystem.
   - Keep it deterministic: no sleeps, no real network, no dependence on test order.
4. **Cover the edges**: empty, one and many items, boundaries, invalid input, error paths and permission failures.
5. **Prove the test bites.** Where practical, confirm the new test fails without the change. A test that can't fail proves nothing.
6. **Edit only tests and fixtures.** If production code looks wrong, report it rather than fixing it here.
7. **Run the full check command** from `AGENTS.md`, not just the new tests.

For a bug fix, write the failing regression test first (red), then fix (green), then refactor.

## Report

```markdown
| Criterion | Test | Result |
| --- | --- | --- |
| AC1 | tests/test_users.py::test_unknown_id_returns_404 | pass |

Check command: <command> → <last lines of output>
Skipped or flaky: none
```
