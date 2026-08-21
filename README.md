# Convergent Agent Work

English | [简体中文](README.zh-CN.md)

A standalone Codex skill for keeping extended, iterative, and multi-workstream agent tasks focused and convergent without weakening completeness or required verification.

## Why

Long-running agent work can drift into repeated tool calls, overlapping reviewers, full re-reviews after small fixes, and ever-growing context. This skill gives Codex a compact set of decision rules for making each additional action reduce uncertainty, change the artifact, or verify a material risk.

## What it does

- Maintains a compact working ledger of constraints, findings, changes, validations, and unresolved risks.
- Requires each new tool call, retry, review pass, or subagent to provide incremental value.
- Keeps delegated workstreams distinct and reuses existing agent context.
- Narrows review follow-ups to the changed surface instead of restarting the whole review.
- Reconciles ambiguous external mutations before retrying them.
- Compacts accumulated context and continues instead of ending an achievable task early.

The skill does not impose fixed limits on tools, time, tokens, or agents, and it never treats efficiency as permission to skip required work.

## When to use it

Good fits include:

- extended coding, debugging, migration, or release tasks;
- multi-agent work with independent risk surfaces;
- iterative review, fix, and validation cycles;
- workflows showing repeated commands, duplicate reviews, or context churn.

It is intentionally not meant for ordinary short tasks merely because they use more than one tool.

## Install

### With the skill installer

Ask Codex to install this repository:

```text
$skill-installer Install the skill from https://github.com/iloveOREO/convergent-agent-work
```

If the repository is private, make sure the environment running Codex is authenticated to GitHub.

### Manually

Clone the repository into the user-level skills directory documented by Codex:

```bash
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/iloveOREO/convergent-agent-work \
  "$HOME/.agents/skills/convergent-agent-work"
```

Codex detects skill changes automatically. If the skill does not appear, restart Codex.

To update a manual installation:

```bash
git -C "$HOME/.agents/skills/convergent-agent-work" pull --ff-only
```

## Use

Invoke the skill explicitly:

```text
$convergent-agent-work Keep this multi-agent migration thorough and convergent.
```

Codex may also invoke it implicitly when a task matches the scope in the skill description.

## Safety boundaries

- Review-only requests remain read-only; fixes require implementation authorization.
- Ambiguous external writes are reconciled against authoritative state before any retry.
- Required repository checks and risk-proportionate validation are never skipped for efficiency.
- A fresh-task handoff is reserved for a user request, a genuine blocker, or an environment that cannot continue safely or reliably.

## Repository layout

```text
.
├── README.md
├── README.zh-CN.md
├── SKILL.md
└── agents/
    └── openai.yaml
```

- `README.md` and `README.zh-CN.md` provide English and Simplified Chinese documentation.
- `SKILL.md` contains the skill metadata and workflow instructions.
- `agents/openai.yaml` provides Codex UI metadata and enables implicit invocation.

See the [official OpenAI documentation for building skills](https://developers.openai.com/codex/skills) for the skill format, discovery locations, and invocation behavior.
