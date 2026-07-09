# Skills

Personal library of reusable [Agent Skills](https://agentskills.io)

| Skill | What it does |
|---|---|
| [qa-check](qa-check/) | Turns the current git diff into a tailored, executable QA checklist and drives the harness's browser tool to run it. Mid-development, not a pre-push gate. |
| [quality-gate](quality-gate/) | Pre-push judgment review: scope creep, spec conformance, test-coverage sanity, debug artifacts, convention consistency. Assumes lint/tests are hook-gated. |
| [sanity-check](sanity-check/) | A skeptical second look before committing to a direction — pushback, "are you sure?", root cause vs symptom, and hidden-assumption catching. |
| [find-a-path](find-a-path/) | Turns "don't know where to start" or "no clean win" into one concrete first step by scoping the problem and surfacing the unknowns. |
| [research](research/) | Finds exactly three credible sources on a topic, summarizes each in 2-3 sentences, and flags where they disagree. |
| [experiment](experiment/) | Throwaway prototyping mode — shortest path to a running result, no abstraction/error-handling/tests, explicitly not production code. |
| [unstick](unstick/) | For low motivation: finds the actual friction point and shrinks the task to a two-minute first step, no hype. |

## Install

Add a skill (or the whole library) to your project with the [`skills`](https://agentskills.io) CLI:

```bash
npx skills add eng-enablement-platform/skills
```

This pulls the skills into your agent's skills directory, where the harness discovers them by `description` and loads them on demand.

## License

MIT
