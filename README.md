# Codex Review Loop

A Codex plugin for independent whole-repository reviews, verified repairs, checks, and authorized local commits.

## Install

In a Codex desktop task or an interactive Codex CLI session, paste this prompt into the message input:

```text
Add lab1702/codex-review-loop as a plugin marketplace, then install
codex-review-loop@lab1702-review-loop from that marketplace.
```

Codex will handle the marketplace setup and installation. Follow any installation prompts it presents, then start a new task or CLI session in the repository you want reviewed.

If you prefer to select the plugin yourself, ask Codex to add only the marketplace first. Then open the desktop app's plugin directory, select the **Codex Review Loop** marketplace, and install **Codex Review Loop**. Restart the app if the marketplace is not visible.

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

## License

This project is licensed under the [MIT License](LICENSE).
