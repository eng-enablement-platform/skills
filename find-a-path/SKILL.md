---
name: find-a-path
description: Turns "I don't know where to start" or "there's no clean win here" into a concrete first step. Scopes a vague or stuck problem, surfaces the unknowns blocking progress, and proposes a specific place to begin - even when every option has a downside. Use when the user says "I don't know where to start", "I'm stuck", "there's no good option", "help me scope this", "what's the path forward", or is stalled on an ambiguous or no-win situation.
---

# find-a-path

Get an ambiguous or stuck situation moving again. Two shapes: "too vague to start" and "no option is clean". Both are solved the same way - reduce the fog until one concrete next action becomes obvious, then commit to it. The output is always a specific first step, not a menu of possibilities.

## When to use this

When forward motion has stalled because the problem is under-defined, or because every path has a real cost and none is a clear winner. If the user already knows what to do and just needs it done, this skill doesn't apply.

## Steps

1. **Pin down the actual goal.** What does "done" look like, concretely? Vague problems often hide because the objective was never stated sharply. Strip it to one sentence: "I need X so that Y." If that can't be written yet, that *is* the first unknown to resolve.

2. **Separate the knowns from the unknowns.** List what's actually established versus what's being assumed or is genuinely unknown. Stuckness usually comes from an unknown being treated as an obstacle when it's really just an unanswered question. Name each unknown as a question that could be answered.

3. **Find the constraints that are real vs. assumed.** In no-win situations, some constraints are fixed and some are self-imposed. "We can't change the schema" - is that true, or just uncomfortable? Removing one false constraint often reveals a path that looked blocked.

4. **Identify the cheapest reducer of uncertainty.** The best first step is rarely the "main" work - it's whatever collapses the most unknowns for the least effort. A quick spike, one measurement, one question answered, one throwaway prototype. What single action would tell you the most about which path is viable?

5. **Propose one concrete starting point.** Name a specific first action - not a phase, not a strategy, an actual thing to do next. For no-win cases, state the tradeoff honestly ("this accepts cost A to avoid worse cost B") and pick the least-bad path rather than stalling for a clean one that doesn't exist.

## Rules

- End with one concrete action, not a list of options to choose between. If choice is unavoidable, recommend one and say why.
- Smallest reversible step first - favor the action that buys the most information cheaply.
- Name the unknowns explicitly. An unnamed unknown stays an obstacle; a named one becomes a task.
- In no-win situations, don't stall waiting for a clean option. Pick the least-bad path and be honest about the cost - don't sanitise the tradeoff away.
- Distinguish real constraints from assumed ones - question the ones that only feel fixed.

## Example

User: "I need to migrate this legacy service but there's no clean way - everything's coupled and there's no test coverage."

Weak response (restates the problem): "That's tough, migrations are hard. Maybe start by adding tests?"

Strong path-finding:
> **Goal:** Replace the legacy service without breaking its callers.
> **Real vs assumed constraint:** "No tests" is real. "Must migrate all at once" - is that assumed? A strangler-fig (route new traffic to the new path, leave old alone) may sidestep the coupling entirely.
> **Cheapest uncertainty reducer:** You don't yet know what actually calls this service. One afternoon grepping callers + logging live traffic tells you whether an incremental cutover is even possible. That's the highest-information, lowest-cost first step.
> **Start here:** Add request logging at the service boundary and let it run for a day. Everything else depends on what that reveals - do that before touching the code.
