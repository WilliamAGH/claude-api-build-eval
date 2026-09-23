# claude-api build-eval

Claude Code CLI 2.1.280 ships a guided flow for building and improving evals for Claude-powered apps. It lives inside the bundled `claude-api` skill and runs as subcommands:

| Command | What it does |
|---|---|
| `/claude-api build-eval [what to measure]` | Interviews you, then produces a runnable eval: the inputs, a way to run your app on each input, and a grader. It pauses for your sign-off on the inputs, the grading method, and the cost before the first paid run. |
| `/claude-api hillclimb` | Improves your app against an existing eval. Each round reads failures, applies a change, and re-runs the eval on train/validation/test splits. It stops at a budget and stopping condition you approve. |
| `/claude-api cost-optimize` | Ranks ways to cut API spend without lowering quality. On request, it applies a change and measures it against your eval. |

You don't need this repo to use the flow. Run the command in Claude Code 2.1.280 or later. This repo is a snapshot of the guides the CLI loads, so the team can read them without starting a session.

## Files

Copied unmodified from the `claude-api` skill's `shared/evals/` directory in Claude Code 2.1.280.

| File | Purpose |
|---|---|
| [`build-eval.md`](build-eval.md) | The build-eval interview. Step 0: what's being evaluated. Step 1: where the prompts come from (an existing eval, transcripts, or synthesized cases). Step 2: grading method. Step 3: runnable script and measured cost. |
| [`eval-audit.md`](eval-audit.md) | The health checklist every eval must pass: task design, harness design, metrics hygiene, grader design, and whether the eval can detect the change you're testing. Both build-eval and hillclimb load it. |
| [`eval-hillclimb.md`](eval-hillclimb.md) | The hillclimb loop: read failures, propose a change, apply it, run, record. State is kept on disk. |
| [`cost-hillclimb.md`](cost-hillclimb.md) | How the hillclimb loop searches when the goal is lower cost at equal or better quality. |
| [`report/runner-scaffold.mjs`](report/runner-scaffold.mjs) | Runner template to copy into your repo. It takes `--variant`, `--model`, and `--reps`, resumes interrupted runs, and refuses to run a harness changed since your last approval. |
| [`report/build-report-lite.mjs`](report/build-report-lite.mjs) | Renders a static `report.html` and `trajectory/scores.tsv` from a run directory. |
| [`report/SCHEMA.md`](report/SCHEMA.md) | Schema of the `state.json` file passed from a run directory to the report renderer. |

## Getting the current version

The CLI extracts these files each session to a temporary path, `<tmp>/bundled-skills/<version>/<hash>/claude-api/shared/evals/`. Newer CLI releases may change them. To refresh this snapshot, invoke `/claude-api` in a new session and copy that directory again.

## Anthropic docs

- [Define success criteria](https://platform.claude.com/docs/en/test-and-evaluate/define-success)
- [Develop test cases](https://platform.claude.com/docs/en/test-and-evaluate/develop-tests)
- [Console Evaluation tool](https://platform.claude.com/docs/en/test-and-evaluate/eval-tool)

## License

The guides and scripts are Anthropic's content, distributed with Claude Code under Anthropic's commercial terms. This repo is private and for internal team reference only. Do not make it public.
