---
name: research
description: Finds exactly three credible sources on a topic, summarizes each in two to three sentences, and flags where they disagree. Deliberately narrow - three good sources, not a link dump. Use when the user asks to "research X", "find sources on", "what does the literature say", "get me some references on", or wants a quick, credible read on a topic without an exhaustive survey.
---

# research

Give a fast, credible read on a topic: three sources, each summarized tightly, with any disagreement between them called out. The discipline is *exactly three* - enough to triangulate, few enough to actually vet. A ranked pile of ten links is a failure mode, not thoroughness.

## When to use this

When someone needs a trustworthy orientation on a topic quickly and doesn't need a literature review. If the ask is genuinely exhaustive ("survey everything on X"), this skill is the wrong tool - say so.

## Input

If a topic was given with the invocation (e.g. `/research everything on the PPC API`), take it and proceed - do the sharpening in Step 1 yourself rather than bouncing it back. Only ask the user for a topic when the invocation carried none at all.

## Steps

1. **Sharpen the question.** Restate the topic as a specific question the sources should answer. "Research caching" → "what are the current recommended cache-invalidation strategies for read-heavy web APIs?" A vague question yields vague sources.

2. **Find three credible sources.** Use web search. Prefer primary sources, official docs, peer-reviewed work, or recognized authorities over content-farm blogs. Check the date - flag anything stale for a fast-moving topic. If you can't find three credible ones, report how many you found rather than padding with weak links.

3. **Summarize each in 2-3 sentences.** For every source: what it claims relevant to the question, and why it's credible (who, when, what authority). Link it - if a claim can be checked, cite where. Gloss any jargon in plain English on first use ("a subnet is a network inside a network") so the summary stands on its own. No filler - it should let the reader decide whether to open the source.

4. **Flag disagreement.** State explicitly where the sources conflict, or note if they agree. Disagreement is the most valuable output - it tells the reader where the topic is genuinely unsettled versus settled. If they agree, that consensus is itself a finding.

## Rules

- Exactly three sources. Not two, not seven. If three credible ones don't exist, say so.
- Credibility over convenience - vet the source, don't grab the first result.
- Every summary links its source and notes why it's trustworthy.
- Disagreement is a required output. State it, or state that they agree.
- Never fabricate a source or a URL. If unsure a link is real, verify it before citing.

## Example

Topic: "Is `useMemo` worth using by default in React?"

> **1. React docs - "useMemo" (react.dev, current):** Recommends `useMemo` only for expensive computations or referential stability, not by default. Authoritative - the official reference.
>
> **2. Kent C. Dodds, "When to useMemo and useCallback":** Argues most `useMemo` usage costs more than it saves due to the hook's own overhead. Credible - well-known React educator, though a personal blog.
>
> **3. React Compiler docs (react.dev, current):** States the compiler auto-memoizes, making manual `useMemo` largely unnecessary going forward. Official and forward-looking.
>
> **Disagreement:** All three agree manual `useMemo`-by-default is wrong. They differ on *why it matters less over time* - (3) says the compiler removes the need entirely, while (1) and (2) still assume manual, hand-tuned use. Direction of travel: toward not needing it.
