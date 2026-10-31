# Benchmark evidence

Source: table supplied by the user in this conversation, received 31 October 2026. The original report, sample counts, confidence intervals, and detailed evaluation setup were not supplied. These are the user's working data, not independently verified official prices or results.

| Model | Effort | Terminal-Bench 4.0 | Cost/task | FrontierCode 2.0 | Cost/task |
| --- | --- | ---: | ---: | ---: | ---: |
| GPT-6 Luna | low | 0.0% | $0.0054 | 2.2% | $0.0073 |
| GPT-6 Luna | medium | 2.5% | $0.0716 | 12.8% | $0.0528 |
| GPT-6 Luna | high | 4.5% | $0.1204 | 18.6% | $0.0844 |
| GPT-6 Luna | xhigh | 8.1% | $0.1982 | 18.4% | $0.1103 |
| GPT-6 Luna | max | 12.6% | $0.2362 | 25.2% | $0.1759 |
| GPT-6.1 Terra | low | 15.2% | $0.1066 | 24.5% | $0.1067 |
| GPT-6.1 Terra | medium | 32.3% | $0.2757 | 34.5% | $0.2575 |
| GPT-6.1 Terra | high | 41.9% | $0.4375 | 37.5% | $0.3919 |
| GPT-6.1 Terra | xhigh | 46.5% | $0.6570 | 36.9% | $0.5702 |
| GPT-6.1 Terra | max | 55.6% | $1.4853 | 42.3% | $1.2925 |
| GPT-6.1 Sol | low | 30.8% | $0.3829 | 20.2% | $0.2011 |
| GPT-6.1 Sol | medium | 48.0% | $0.6138 | 33.4% | $0.4107 |
| GPT-6.1 Sol | high | 51.5% | $0.8282 | 36.7% | $0.6299 |
| GPT-6.1 Sol | xhigh | 54.0% | $1.0259 | 38.5% | $0.7028 |
| GPT-6.1 Sol | max | 56.1% | $1.8228 | 40.6% | $1.1304 |

## Comparisons that affect routing

A setting is dominated within a benchmark if another setting costs no more and scores at least as well, with one strict improvement. This describes aggregate evidence, not every individual task.

- Terminal: Terra low dominates Luna high/xhigh/max; Terra medium dominates Sol low; Sol medium dominates Terra xhigh.
- FrontierCode: Luna high dominates Luna xhigh; Terra low dominates Sol low; Terra medium dominates Sol medium; Terra high dominates Terra xhigh and Sol high.
- Luna max is not dominated by Terra low on FrontierCode: 25.2% versus 24.5%, at $0.1759 versus $0.1067. Its extra 0.7 percentage points costs 64.9% more; it is not a general upgrade path.
- Avoid Luna xhigh, Terra xhigh, and Sol low as automatic defaults for these benchmark-shaped tasks. Relevant local evidence or a user override can justify them.
- Terminal: Sol max over Terra max buys 0.5 percentage points for 22.7% more cost.
- FrontierCode: Terra max has the highest supplied score, 42.3%; Sol max is cheaper ($1.1304 versus $1.2925) but scores lower (40.6%).

## Cost interpretation

Cost/task is the average spend per attempted benchmark task. Actual subagent spend varies with context, tool use, output, retries, and verification. Scores do not establish task-specific probabilities, latency, noncoding quality, or performance of retry chains.

Cost/task divided by aggregate success rate can describe benchmark cost per solved item when the score represents a solved fraction. It does not predict independent retry success on one hard task. Do not use repeated-attempt probability formulas without evidence about conditional outcomes.

Choose a route for its expected total cost and the required quality bar. Prefer measured outcomes on comparable real tasks when they conflict with this table.
