## Summary

<!-- What changed and why, in 2–4 concise bullets. -->

- 

## Spec / Issue Reference

- Issue: Closes #<!-- issue-number (or N/A for trivial changes) -->
- Spec folder: `specs/<id>-<slug>/` (`spec.md`, `plan.md`, `test-cases.md`, `qa-report.md` — or `N/A (trivial change)`)

## Verification & QA Evidence

<!-- Paste the verdict and gate table from specs/<id>-<slug>/qa-report.md (role-verifying-changes), or inline gate results for trivial changes. -->

- QA Verdict: `PASS` / `N/A`

| Gate | Command | Result |
| --- | --- | --- |
| Format / Lint | | |
| Type check | | |
| Tests | | |

## Checklist

- [ ] All acceptance criteria are covered by automated tests (or regression test added for bug fixes)
- [ ] All required quality gates in `AGENTS.md` pass locally with no skipped or deleted tests
- [ ] Reviewed in a clean context against `repo-review-rules` (`role-code-reviewer`)
- [ ] Security check completed (`role-security-reviewer` when relevant) and no secrets, keys, or `.env` values are committed
- [ ] Changes stay within scope (follow-ups noted separately)
