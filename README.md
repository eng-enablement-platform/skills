# Skills

Personal library of reusable [Agent Skills](https://agentskills.io)

| Skill | What it does |
|---|---|
| [qa-check](qa-check/) | Turns the current git diff into a tailored, executable QA checklist and drives the harness's browser tool to run it. Mid-development, not a pre-push gate. |
| [quality-gate](quality-gate/) | Pre-push judgment review: scope creep, spec conformance, test-coverage sanity, debug artifacts, convention consistency. Assumes lint/tests are hook-gated. |
| [sanity-check](sanity-check/) | A skeptical second look before committing to a direction - pushback, "are you sure?", root cause vs symptom, and hidden-assumption catching. |
| [find-a-path](find-a-path/) | Turns "don't know where to start" or "no clean win" into one concrete first step by scoping the problem and surfacing the unknowns. |
| [research](research/) | Finds exactly three credible sources on a topic, summarizes each in 2-3 sentences, and flags where they disagree. |
| [experiment](experiment/) | Throwaway prototyping mode - shortest path to a running result, no abstraction/error-handling/tests, explicitly not production code. |
| [get-unstuck](get-unstuck/) | Finds the actual friction point and shrinks the task to a two-minute first step, no hype. |

## Prototypes (work-in-progress skills)

`prototypes/` holds skills you're still building and haven't published to the table above. Because they live in a plain source dir, the harness **doesn't discover them automatically** — you have to symlink each one onto a discovery path. (This is why a new prototype "doesn't show up" until you wire it in.)

1. **Build the prototype.** Create `prototypes/<name>/SKILL.md` with valid frontmatter (`name` + `description`). Iterate freely here — nothing is published or shared until you decide.

2. **Symlink it onto the discovery path** so the agent can find it. Pick a scope:

   ```bash
   # Global — available in every project (matches gitlab-mr / gitlab-review)
   ln -s ~/Desktop/repos/skills/prototypes/<name> ~/.agents/skills/<name>

   # Project-local — only active inside a specific repo
   ln -s ~/Desktop/repos/skills/prototypes/<name> /path/to/repo/.claude/skills/<name>
   ```

   Symlinks (not copies) keep a single source of truth: edit the prototype once and every wired-in location stays in sync.

3. **Restart the harness** to pick up the new skill — descriptions are loaded into context at session start.

To promote a prototype to a real skill, move it out of `prototypes/` and add a row to the table above.

## Two ways to use these

These skills can be invoked two different ways, depending on who decides to trigger them.

### 1. Model-invoked (the Agent Skills spec)

This is the default. The harness reads each skill's `name` + `description` into context, and the agent decides when to pull one in by matching your request against the trigger conditions in the `description`. You don't type a command - you say "QA this diff" and the agent loads `qa-check` on its own.

To use the library this way in *another* project, install it with the [`skills`](https://agentskills.io) CLI:

```bash
npx skills add eng-enablement-platform/skills
```

This pulls the skills into that project's skills directory, where its harness discovers them on demand. You only need this to consume the library elsewhere - if you're in this repo, the skills are already here and nothing needs installing.

> Note: the exact `npx skills add` syntax is unverified against the real CLI. Confirm it before relying on it to publish/consume the library.

### 2. User-invoked (slash commands)

Some harnesses also let you trigger a skill explicitly by typing `/name`, giving you manual control instead of waiting for the agent to auto-match.

GSD/pi does this by scanning `~/.gsd/agent/prompts/` for `.md` files - each becomes a `/<filename>` command that fires the file's whole body as the prompt. To expose these skills as commands, symlink each `SKILL.md` into that dir:

```bash
ln -s /path/to/skills/qa-check/SKILL.md ~/.gsd/agent/prompts/qa-check.md
# ...one per skill
```

Symlinks (not copies) keep a single source of truth: edit the skill once and the command stays in sync. Restart the harness to pick up new commands - templates load at session start.

## License

MIT
