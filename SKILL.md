---
name: selecting-subagent-models
description: Use when preparing to create or configure any subagent with spawn_agent, including workers, reviewers, researchers, extractors, and diagnostic agents.
---

# Selecting Subagent Models

## Core principle

Choose the least expensive model and reasoning effort that has a high probability of completing
the exact delegated contract. Honor an explicit user model or effort choice.

## Selection

Score the task before spawning:

| Evidence | Lower tier | Higher tier |
|---|---|---|
| Contract | exact, bounded | ambiguous, exploratory |
| Coupling | isolated output | broad shared state |
| Reasoning | mechanical | multi-step synthesis |
| Failure | cheap retry | costly or high-stakes |
| Context | narrow artifact | many interacting files |

Interpret the available tiers as follows: Luna is economical, Terra is balanced, and Sol is strongest.

| Task profile | Model | Effort |
|---|---|---|
| Clear mechanical extraction, formatting, verification | `gpt-5.6-luna` | `low`/`medium` |
| Ordinary coding, analysis, review, synthesis | `gpt-5.6-terra` | `medium`/`high` |
| Ambiguous architecture, difficult debugging, costly failure | `gpt-5.6-sol` | `high`/`xhigh` |

Use `low` only when success criteria are explicit and little search is required; otherwise use
`medium` as the balanced default. Use `high` or `xhigh` only when task evidence justifies it.
If an agent-selected override has no clear benefit, omit only that field to inherit its default.
When setting `model`, use only a model ID listed in the matrix; never invent another ID.
Never omit or change a model or effort field explicitly specified by the user.

## Dispatch

State the exact task, minimum context, required output shape, and success criteria. A stronger
model does not repair a vague prompt.

Add a scout only when its result can materially shrink a larger expensive task. Escalate after
an observable failure, inconsistency, missed cross-context reasoning, or a non-converging fix
loop. Resume the same agent when continuity matters; replace it when the model tier proved
insufficient.

## Preflight

Before `spawn_agent`, confirm:

- user overrides preserved;
- weakest reliable tier selected;
- reasoning effort justified;
- coordination overhead lower than expected savings;
- prompt and success contract are narrow.
