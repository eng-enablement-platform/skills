---
name: experiment
description: Throwaway prototyping mode — get something running fast to learn whether an idea works, explicitly not production code. Skips abstraction, error handling, and tests; favors a single file and the shortest path to a result. Use when the user says "let's prototype", "quick experiment", "just hack something together", "throwaway spike", "does this even work", or wants to validate an idea before investing in a real implementation.
---

# experiment

Prototyping mode for learning fast. The goal is a running result that answers "does this approach work?" — not clean code. Abstraction, error handling, validation, and tests are deliberately skipped because they cost time and the code is going to be thrown away. Optimize for speed to signal.

## When to use this

When the question is "will this work?" and the answer is worth more than the code. Validating a library, checking an API's behavior, testing a hunch. Not for anything that ships — the moment a prototype is meant to survive, switch out of this mode and build it properly.

## Steps

1. **Name what you're testing.** One sentence: the specific question the prototype answers. "Can I stream tokens from this API and render them incrementally?" A prototype without a question is just unfinished code.

2. **Build the shortest path to an answer.** One file if possible. Hardcode inputs. Skip config, DI, and abstraction. Use the most direct call, not the "right" one. If a value can be a literal instead of a parameter, make it a literal.

3. **Skip everything that isn't the question.** No error handling (let it crash — the stack trace is data). No input validation. No tests. No logging beyond a `print` to see what happened. No edge cases unless an edge case *is* the thing being tested.

4. **Run it and read the result.** Get it executing fast. The output — including failures — is the entire point. A crash that answers the question is a success.

5. **State the finding and mark it throwaway.** Report what the prototype revealed and whether the approach is viable. Explicitly label the code as throwaway: it is not production-ready and should be rewritten, not extended, if the idea proceeds.

## Rules

- Speed to signal over everything. If it delays a running result, cut it.
- No abstraction, error handling, validation, or tests unless they *are* what's being tested.
- Single file, hardcoded inputs, most direct API call. Boring and literal beats flexible.
- Always label the output as throwaway. Never let a prototype silently become the real thing.
- A crash that answers the question is fine — don't harden against failures you're trying to observe.

## Example

Question: "Does this vector DB return relevant results for our docs with default settings?"

Prototype approach:
- One script. Hardcode 5 sample docs and 2 query strings inline — no loader, no config file.
- Direct client call with defaults. No connection pooling, no retry, no error handling.
- `print` the results. Eyeball relevance by hand.

> **Finding:** Default settings returned the right doc as top hit for query 1 but ranked it 4th for query 2 (the paraphrased one). Suggests default embedding is weak on paraphrase — worth testing a better embedding model before committing. **This script is throwaway** — if we proceed, the real ingestion path needs to be built properly, not grown from this.
