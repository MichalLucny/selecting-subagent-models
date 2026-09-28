---
name: selecting-subagent-models
description: Use when preparing to create or configure any subagent with spawn_agent, including workers, reviewers, researchers, extractors, and diagnostic agents.
---

# Selecting Subagent Models

## Core principle

Choose the least expensive model and reasoning effort that has a high probability of completing the exact delegated contract. Honor an explicit user model or effort choice. Apply this skill only when delegation is authorized by the active instructions.

## Selection

Score the task before spawning:

| Evidence | Lower tier | Higher tier |
| --- | --- | --- |
| Contract | exact, bounded | ambiguous, exploratory |
| Coupling | isolated output | broad shared state |
| Reasoning | mechanical | multi-step synthesis |
| Failure | cheap retry | costly or high-stakes |
| Context | narrow artifact | many interacting files |

Interpret the available tiers as follows: GPT-6 Luna is the economical choice for scoped work, and GPT-6 Sol is the stronger choice when the outcome needs more judgment or reliability. Use the FrontierCode cost-per-task results below as a routing signal, not a guarantee for an individual task.

| Task profile | Model | Starting effort |
| --- | --- | --- |
| Mechanical extraction, formatting, verification, or a tiny specified edit | `gpt-6-luna` | `low` |
| Scoped coding or coordinated edits with clear requirements and cheap verification | `gpt-6-luna` | `medium` |
| Harder but still bounded coding where a stronger Luna attempt is cheaper than handing the task to Sol | `gpt-6-luna` | `max` |
| Ambiguous coding, broad shared state, cross-file synthesis, difficult debugging, or costly failure | `gpt-6-sol` | `medium` |
| A Sol task with concrete evidence that more reasoning is needed | `gpt-6-sol` | `high` |

In the supplied FrontierCode benchmark, Luna `medium` scored 35.5% at $0.053 per task, Luna `max` scored 42.4% at $0.11, and Sol `medium` scored 45.9% at $0.80. Sol `medium` cost about 7.3 times Luna `max` for a 3.5 percentage-point gain. Luna `xhigh` cost more and scored slightly lower than Luna `high`; Sol `low` cost much more than Luna `high` for the same aggregate score. Do not select those two dominated settings by default. These are benchmark averages, so task fit and the cost of a wrong answer still govern the choice.

Use `low` only when success criteria are explicit and little search is required. Start most scoped coding at Luna `medium`; use Luna `max` when a bounded task needs more reasoning and its result can be checked cheaply. Start Sol at `medium` when judgment or reliability warrants its cost. Use Sol `high` or `xhigh` only when task evidence justifies the additional expense; do not select Sol `max` routinely. If an agent-selected effort override has no clear benefit, omit that field to inherit its default. For automatic model selection, use only a model ID listed in the matrix; never invent another ID. Never omit or change a model or effort field explicitly specified by the user.

## Dispatch

State the exact task, minimum context, required output shape, and success criteria. A stronger model does not repair a vague prompt.

Add a scout only when its result can materially shrink a larger expensive task. Review a Luna result against the full contract and check relevant edge cases when that is cheap; passing provided tests alone may miss one. After a failed Luna attempt, identify whether the problem was missing context, an unclear contract, or insufficient reasoning. Give the same agent focused feedback for a local, correctable miss before paying for a stronger model. Increase Luna effort for a bounded, checkable task that needs more reasoning; move to Sol `medium` when the work needs judgment across contexts or Luna remains insufficient. Increase Sol effort only after an observable failure or when the cost of failure warrants it. Resume the same agent when continuity matters; replace it when the model choice proved insufficient.

## Preflight

Before `spawn_agent`, confirm:

- user overrides preserved;
- weakest reliable tier selected;
- reasoning effort justified;
- coordination overhead lower than expected savings;
- prompt and success contract are narrow.

## Sources

- User-provided `FrontierCode.svg` benchmark (28 September 2026): cost per task and aggregate coding score by model and reasoning effort.
- [OpenAI model selection guidance](https://developers.openai.com/api/docs/guides/model-selection)
- [GPT-6 model guidance](https://developers.openai.com/api/docs/guides/latest-model)
