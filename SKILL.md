---
name: selecting-subagent-models
description: Use when preparing to create or configure any subagent with spawn_agent, including workers, reviewers, researchers, extractors, and diagnostic agents.
---

# Selecting Subagent Models

## Scope and objective

Apply only when delegation is already authorized. Preserve explicit user choices of model and reasoning effort. Optimize the total cost of an accepted result: initial attempt, verification, coordination, and likely rework.

Classify the actual delegated work before choosing a model:

- Mechanical: supplied inputs, exact transformation, deterministic acceptance.
- Coding: code changes, code review, or reasoning about interacting files.
- Terminal: environment diagnosis, build or dependency repair, and multi-step tool execution.

Use the most specific matching row in the policy table. For mixed tasks, route by the hardest substantial component. For writing, research, and other work outside these coding benchmarks, treat this matrix as an initial hypothesis and prefer evidence from comparable tasks.

## Balanced policy

Use Terra as the default for substantive coding. Spend more on terminal tasks when the table shows a useful improvement in success.

| Delegated task | Model | Starting effort |
| --- | --- | --- |
| Mechanical extraction, formatting, or exact verification | `gpt-6-luna` | `low` |
| Small isolated fix or simple terminal task with clear acceptance | `gpt-6.1-terra` | `low` |
| Ordinary coding, code review, or cross-file work | `gpt-6.1-terra` | `medium` |
| Difficult coding or costly missed interactions | `gpt-6.1-terra` | `high` |
| Multi-step terminal diagnosis or build/environment repair | `gpt-6.1-sol` | `medium` |
| Terminal work after a demonstrated reasoning gap at Sol medium, or with unusually expensive failure | `gpt-6.1-sol` | `high` |

Start multi-step terminal diagnosis at Sol medium, including difficult tasks with objective acceptance checks. Start at high when a medium attempt has demonstrated a reasoning gap or failure would be unusually expensive.

On FrontierCode, Terra medium scores 34.5% at $0.2575 versus Sol medium's 33.4% at $0.4107: 37.3% less cost with a higher aggregate score. On Terminal, Sol medium scores 48.0% at $0.6138 versus Terra xhigh's 46.5% at $0.6570. These support different defaults for coding and terminal work.

## Recovery and expensive settings

Correct missing context or a local misunderstanding with focused feedback. For a persistent reasoning gap, move Terra low/medium to Terra high for coding, or move to Sol medium/high for terminal work. Reuse the same agent when continuity helps and its model remains suitable; replace it when the model is insufficient. Skip intermediate efforts when the observed problem warrants a larger step.

For unusually difficult work, Sol xhigh is a terminal option and Terra max is a coding option. Neither is routine. Choose Sol max on Terminal only when its last aggregate gain is valuable enough: 56.1% versus Terra max's 55.6%, at $1.8228 versus $1.4853. Do not infer that more effort always improves the result.

## Runtime and dispatch

Use only model IDs and effort values accepted by the current spawn tool. The measured automatic choices are `gpt-6-luna`, `gpt-6.1-terra`, and `gpt-6.1-sol`, with efforts from `low` through `max`. Other available models and `ultra` have no evidence in this dataset; select them only for an explicit user choice or separate task-specific evidence.

For an automatically selected route, set both `model` and `reasoning_effort` explicitly. When the runtime permits overrides only with an isolated fork, use `fork_turns: "none"` and supply the necessary context. If the user explicitly requires an incompatible model, effort, or full-history fork, explain the conflict instead of silently changing that choice.

Supply the exact task, relevant context, required output, and acceptance criteria. Add a scout or reviewer when its expected savings or reduction in rework exceeds its cost. Check the returned artifact against the full contract, including relevant edge cases.

## Evidence

These defaults use the user's Terminal-Bench 4.0 and FrontierCode 2.0 table received on 31 October 2026. Cost/task means a benchmark average, not a token tariff or a spending cap. Aggregate scores do not estimate success probabilities for a specific task, and small score differences have no supplied uncertainty estimates.

Read [references/benchmarks.md](references/benchmarks.md) when comparing alternatives, checking dominance, or revising a route. Compare settings within the relevant benchmark; do not average the two scores into a universal ranking.

General guidance: [OpenAI model selection](https://developers.openai.com/api/docs/guides/model-selection) recommends comparing representative inputs and choosing the lightest setting that meets the quality bar.
