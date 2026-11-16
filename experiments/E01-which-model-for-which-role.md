# E01. Which model for which role

[Back](../README.md) · 9 Nov 2026

## Question
The team had four agent harnesses and several models, assigned to roles by habit. Which harness and model should fill each role, judged on the same tasks with the same scorer?

## Setup
- Eight studio roles: writer, scout, miner, critic, UX reviewer, builder, footage judge, animator.
- Each role has a small fixed task set (3 to 8 tasks) and a **deterministic scorer** (a script, not a model), so the score does not depend on another model's opinion.
- Each harness and model pair ran the same set. A seat moves only if a challenger beats it by more than 0.10.
- Failures are split into two kinds. An **adapter** failure means the harness never returned an answer (bad flag, permission or budget); it is fixed and re-run, never counted against the model. A **model** failure means an answer came back and scored 0.

## Results (score 0–1, list-price cost for the set)

| Role | Claude | Codex / GPT-6.1 Terra | Hermes / GPT-6.1 Sol | Grok 4.7 |
|---|---|---|---|---|
| Writer | 0.94 (Sonnet, $0.20) | **0.94 ($0.07)** | 0.94 ($0.37) | 0.69 ($0.62) |
| Scout | 0.50 (Opus, $1.22) | 0.61 ($0.20) | **1.00 ($0.47)** | **1.00 ($1.14)** |
| Miner | 0.75 (Opus, $0.52) | **0.96 ($0.04)** | 0.83 (GPT-6 Luna) | 0.74 ($0.12) |
| Critic | **0.74 (Sonnet, $0.19)** | 0.68 ($0.06) | adapter fail | 0.60 ($0.18) |
| UX reviewer | **0.79 (Sonnet, $0.21)** | 0.51 ($0.05) | adapter fail | adapter fail |
| Builder | 1.00 (Sonnet, $0.17) | **1.00 ($0.05)** | 1.00 ($0.53) | 0.67 (1 adapter fail) |
| Footage judge | 0.47 (Sonnet) | not run | not run | 0.48 |

n = 3 to 8 tasks per cell. The animator role was not benchmarked.

## What changed
Seats were set from these results: Terra for writer, miner and builder; Sonnet for critic and UX review; Grok for scouting (it was also the cheapest on its own subscription). A dispatcher routes each call to its seat, falls back to a second harness on quota errors, and logs every call. Over 9 to 10 Nov the log held 346 lines, with 11 automatic fallbacks and 12 failed attempts.

## Lessons
- The most expensive model was not best at the narrow roles. Opus scored lowest on scouting and mining.
- Separating adapter failures from model failures mattered: three "zeros" were harness plumbing, not model ability.
- Small, scriptable tasks hide differences that appear on real work. All builders scored 1.00 here, yet [E03](E03-green-tests-hidden-failures.md) shows large gaps on real tasks. A benchmark with easy oracles flatters cheap models.
- Footage judging scored about 0.47 for both models tried. Neither is usable for it yet.

## Limitations
Tiny task sets, one run per pair, one day. The scorers encode our own definitions of good.
