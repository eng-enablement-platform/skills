# Skills

Personal library of reusable [Agent Skills](https://agentskills.io)

| Skill | What it does |
|---|---|
| [qa-check](qa-check/) | Turns the current git diff into a tailored, executable QA checklist and drives the harness's browser tool to run it. Mid-development, not a pre-push gate. |

## Install

Add a skill (or the whole library) to your project with the [`skills`](https://agentskills.io) CLI:

```bash
npx skills add eng-enablement-platform/skills
```

This pulls the skills into your agent's skills directory, where the harness discovers them by `description` and loads them on demand.

## License

MIT
