---
name: quality-gate
description: Pre-push judgment review of a change before it leaves your machine. Reviews scope creep, spec conformance, test-coverage sanity, leftover debug artifacts, and codebase convention consistency — the judgment calls a linter can't make. Use when the user asks to "review before push", "quality gate", "is this ready to push", "final review", or wants a last check before a PR. Assumes lint/typecheck/tests/build are already enforced by pre-commit hooks — it does not re-run them.
---

# quality-gate

The last judgment pass before code leaves the machine. Mechanical correctness (lint, types, tests, build) is assumed already gated by hooks — this skill only covers what a human reviewer would catch and a tool won't. Every finding must point at a specific file/hunk; a comment that could apply to any diff is noise.

## When to use this

Right before `git push` or opening a PR, on a change the author believes is done. Not a mid-development QA pass (that's `qa-check`) and not a mechanical checker — if the finding is "lint would flag this", stay silent and let the hook do its job.

## Steps

1. **Read the full change.** `git diff <base>...HEAD` for the branch's whole delta, not just the last commit. Use `git status` for untracked files. Establish what the change is *supposed* to do — from the branch name, commit messages, linked issue, or by asking if intent is genuinely unclear.

2. **Scope creep.** Compare the diff against the stated intent. Flag changes that don't serve it: unrelated refactors, drive-by reformatting, files touched with no connection to the goal. Small opportunistic fixes are fine — call them out so they can be split if the author prefers, don't block them.

3. **Spec conformance.** Does the change actually do what it set out to do, and *only* that? Look for half-implemented paths, a requirement from the intent that no hunk addresses, or behavior that drifted from what was asked.

4. **Test-coverage sanity.** This is judgment, not a coverage number:
   - New logic (a branch, a condition, an error path) with no corresponding new test.
   - Stray `.only` / `.skip` / `fdescribe` / `xit` left in test files — these silently disable other tests.
   - Tests that assert nothing meaningful, or were changed to pass rather than to verify.

5. **Debug artifact sweep.** Leftover `console.log`, `print`, `dbg!`, `debugger`, commented-out code blocks, `TODO`/`FIXME` added in this diff, hardcoded test values, or temporarily disabled guards. Only flag artifacts *introduced by this diff* — pre-existing ones aren't this change's problem.

6. **Convention consistency.** Compare the new code against how the surrounding codebase already does things: naming, file layout, error handling, import style, how similar problems were solved elsewhere. Flag divergence from established local patterns, not divergence from your personal preference.

7. **Verdict.** Group findings by the five areas above, each tagged blocking / consider / nit and citing the file:line. Every finding should point at *why* it matters, not just what it is — a reason the author can act on. End with a one-line call: ready to push, or the specific things to resolve first. If the change is clean, say so plainly — don't manufacture findings.

## Rules

- Every finding cites a specific file and hunk. No generic advice.
- Do not re-run or report lint, typecheck, unit tests, or build — those are hook-gated. If your only concern is mechanical, stay silent.
- Judge against the codebase's own conventions and the change's stated intent, not against your preferences.
- Separate blocking from optional. A nit is not a blocker; don't inflate severity.
- If the change is sound, a short "ready" is the correct output. Don't pad.

## Example

Branch intent: "add rate limiting to the login endpoint."

Weak finding (generic): "Consider adding tests for the new code."

Strong findings (specific):
> **Blocking — test coverage:** `rateLimiter.ts:42` adds the `windowExceeded` branch but no test exercises the over-limit path; `rateLimiter.test.ts` only covers under-limit.
>
> **Consider — scope creep:** `userProfile.tsx` reformatting (lines 8-90) is unrelated to rate limiting. Split into its own commit or drop from this push.
>
> **Blocking — debug artifact:** `loginController.ts:31` has `console.log(req.headers)` — leaks headers to logs, added in this diff.
