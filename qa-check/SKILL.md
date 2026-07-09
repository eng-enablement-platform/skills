---
name: qa-check
description: Generates a QA checklist tailored to the current git diff during mid-development, then drives a browser to execute the checks. Each checklist item traces back to a specific change in the diff - no generic boilerplate. Use when the user asks to "QA this", "check my work", "test what I changed", "run a QA pass", or wants to verify in-progress changes before moving on. Not a pre-push gate; this is for catching regressions while still building.
---

# qa-check

Turn the current git diff into a focused, executable QA pass. The value is specificity: every check must point at something that actually changed. A checklist a reader could have written without seeing the diff is a failed checklist.

Mid-development behavioral verification - not the final pre-push gate, and it does not re-run lint/typecheck/tests/build (assume those are handled elsewhere).

## Steps

1. **Read the diff.** Determine the scope of changes:
   - Uncommitted work: `git diff HEAD` (staged + unstaged) and `git status` for new files.
   - If the working tree is clean, fall back to the last commit: `git diff HEAD~1 HEAD`.
   - Ask the user which range they mean only if it's genuinely ambiguous (e.g. a large branch with many commits). Otherwise infer.

2. **Extract what changed and what it affects.** For each meaningful hunk, identify:
   - The user-facing behavior it touches (a route, a component, a form, an API response, a state transition).
   - The inputs that reach it (props, query params, request bodies, user actions).
   - The edge cases the change introduces or moves (empty states, loading, errors, boundary values, permissions).
   Ignore pure formatting, comment-only, and mechanical refactors with no behavioral change - do not pad the checklist with them.

3. **Generate the tailored checklist.** Produce a numbered list where **each item names the specific change it verifies**. Group by area if there are many. For every item, state:
   - *What to check* - the concrete action or observation.
   - *Why* - the change in the diff that makes this worth checking (reference the file/function/component).
   - *Expected result* - what "pass" looks like.

   Cover the golden path first, then the edge cases the diff introduced, then regressions in adjacent behavior the change could plausibly break. If a change has no observable behavior (e.g. internal logging), say so rather than inventing a check.

4. **Detect the browser capability.** Look at what the harness has connected before deciding how to execute:
   - If a browser automation tool is available (Playwright, a browser MCP server, a built-in browser tool, etc.), use it.
   - If nothing is connected, present the checklist for the user to run manually and say explicitly that no browser tool was detected.
   - Do not bundle or install automation - use only what the environment already provides.

5. **Execute against a running app.** Start the dev server if needed (ask for the URL only if non-obvious). Drive UI checks through the connected browser tool, capturing the observed result plus a screenshot or assertion. For non-UI checks (API shape, data changes), verify with an actual request or query rather than guessing.

6. **Report results.** Mark each item pass / fail / skipped-and-why. For failures, give the concrete observation, not "didn't work". End with a short verdict: is the changed behavior sound, and what needs another look.

## Rules

- Every checklist item must trace to a specific change. If you can't name the hunk it verifies, cut it.
- Verify behavior, not appearance. "The component renders" is not a check; "submitting the form with an empty email shows the new validation error added in `LoginForm.tsx`" is.
- Never claim a check passed without having actually run it. Fresh evidence or mark it skipped.
- Don't duplicate mechanical gates (lint, typecheck, unit tests, build) - those are assumed covered.
- Keep the checklist proportional to the diff. A three-line change gets a few checks, not twenty.

## Example

Diff adds `disabled={!isDirty}` to a submit button in `CheckoutForm.tsx`.

Bad (generic): "Test the checkout form works."

Good (tailored):
> **1. Submit disabled on pristine form** - *Why:* `CheckoutForm.tsx` now sets `disabled={!isDirty}`. *Check:* Load checkout, touch nothing. *Expected:* Submit is disabled.
>
> **2. Re-enables after edit** - *Why:* verifies the `isDirty` transition. *Check:* Type in a field, then clear it. *Expected:* Enables on first keystroke, stays enabled after clearing (the diff doesn't reset `isDirty`).
