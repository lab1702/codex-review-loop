# Codex Review Loop

A Codex plugin for independent whole-repository reviews, verified repairs, checks, and authorized local commits.

## Install

Add this repository as a plugin marketplace, then install the plugin:

```sh
codex plugin marketplace add lab1702/codex-review-loop
```

```sh
codex plugin add codex-review-loop@lab1702-review-loop
```

Alternatively, after adding the marketplace, open the desktop app's plugin directory, select the **Codex Review Loop** marketplace, and install **Codex Review Loop**. Restart the app if the marketplace is not visible. Start a new task in the repository you want reviewed after installation.

See the [official plugin packaging and marketplace documentation](https://developers.openai.com/plugins/build/plugins).

## Use

Automatic invocation is disabled through `policy.allow_implicit_invocation: false` in the skill's `agents/openai.yaml`. Explicitly invoke the bundled skill and authorize local commits:

```text
Run $review-loop. I authorize ordinary commits to the current branch.
```

The repository must have a valid commit on a checked-out branch, a clean working tree, and no merge, rebase, or other sequencer operation in progress. The host must support fresh subagents without inherited conversation history. The skill stops if those capabilities or prerequisites are missing.

Each pass reviews the whole repository at an exact commit, validates findings, fixes verified issues, and runs relevant checks. Completion requires two consecutive clean passes on the same unchanged commit, with at most ten attempted passes.

The workflow creates ordinary local commits when authorized. It never pushes, rewrites history, or switches branches. You handle pushes after reviewing the results.

## Repository layout

```text
.agents/plugins/marketplace.json
plugins/codex-review-loop/
  .codex-plugin/plugin.json
  skills/review-loop/SKILL.md
  skills/review-loop/agents/openai.yaml
```

The original skill instructions are preserved in [SKILL.md](plugins/codex-review-loop/skills/review-loop/SKILL.md).
