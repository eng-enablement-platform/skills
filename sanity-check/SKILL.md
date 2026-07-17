---
name: sanity-check
description: A skeptical second look before committing to a direction. Pushes back on a weak idea, re-checks your own confident conclusions ("are you sure?"), separates root cause from symptom, and surfaces the user's hidden assumptions or bias before they derail a decision. Use when the user says "sanity check this", "am I sure about this", "push back on me", "is this the real problem", "check my assumptions", or when about to commit to an approach that hasn't been challenged.
---

# sanity-check

A deliberate skeptical pass on a decision, plan, or conclusion - yours or the user's - before it gets acted on. The job is to find the weak point now, cheaply, rather than after it's built. Be direct and honest: if the idea is sound, say so; if it isn't, say why concretely. The goal is a real check, not point-scoring.


## When to use this

Any moment before commitment where a wrong turn is expensive: about to pick an approach, about to declare a bug fixed, about to accept a conclusion that "feels obviously right". Especially when confidence is high and nothing has actually challenged it yet.

## Input

If the decision or claim to check was given with the invocation, take it and run the lenses below. Only ask the user what to check when the invocation carried nothing.

## The four lenses

Run the ones that fit the situation - not all four every time.

1. **Pushback.** Is this idea actually good, or just the first one that arrived? State the strongest case *against* it. Name the cost, the failure mode, the simpler alternative it skipped. If it survives the strongest objection, it's probably sound - and now you know why.

2. **"Are you sure?"** Take the confident conclusion and try to falsify it. What evidence would prove it wrong, and has that been checked, or just assumed? Distinguish "I verified this" from "this seems right" - conclusions should be rooted in evidence, not vibes. If you can't explain *why* it holds and point at what backs it, you don't understand it yet, and that's the signal to go check before committing. Confidence without a check is a guess wearing a suit.

3. **Root cause vs symptom.** Is the thing being fixed the actual cause, or a surface effect? Ask "why" until it stops giving new answers. A fix that makes the symptom disappear without explaining *why* it appeared is a patch, and the real problem will resurface elsewhere. Name which one this is.

4. **Bias and assumptions.** What is being taken as given that hasn't been established? Look for: sunk-cost ("we already started, so..."), anchoring on the first framing, confirmation (only evidence that fits), and unstated constraints treated as fixed when they're actually choices. Surface the load-bearing assumption and ask whether it holds.

## Steps

1. **Restate the decision or conclusion** in one line, so the thing being checked is explicit and can't shift mid-review.
2. **Pick the lenses** that apply. A design choice wants pushback + assumptions; a "bug is fixed" claim wants "are you sure" + root cause.
3. **Apply them concretely** - tied to the actual situation, not generic caution. "Have you considered edge cases" is useless; "this assumes the token never expires mid-request - has that been checked?" is a sanity check.
4. **Deliver the verdict.** Either: this holds, and here's the reason it survives scrutiny. Or: here's the specific weak point and what would resolve it.

## Rules

- Be concrete. A challenge that could apply to any decision challenges nothing.
- Steelman before you strike - engage the strongest version of the idea, not a strawman.
- If the idea survives, say so clearly. The goal is a real check, not reflexive contrarianism.
- One sharp objection beats five vague ones. Find the load-bearing weakness.
- Challenge the reasoning, not the person. "This assumes X" not "you didn't think about X".

## Example

User: "The API is slow, I'll add a caching layer."

Weak response (generic caution): "Caching can add complexity, are you sure?"

Strong sanity check:
> **Restate:** You're adding a cache to fix API slowness.
> **Root cause vs symptom:** Have you measured *where* the time goes? If it's an N+1 query, caching hides it at first and it resurfaces under load with a stale-data bug on top. What does the profile actually show?
> **Assumption:** This assumes the responses are cacheable - are they per-user or is the data safe to share? If per-user, hit rate may be near zero.
> **Verdict:** Worth doing *if* the profile confirms repeated identical reads. If it's an unindexed query, the index is the root-cause fix and the cache is a patch.
