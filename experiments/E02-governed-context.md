# E02. Governed context vs a naive agent (building the brain)

[Back](../README.md) · 2–3 Nov 2026

## Background: the brain
The team keeps a shared knowledge base (private) following the "LLM wiki" pattern. The operator agent writes pages, Sol reads and corrects them, and a search index (GBrain) turns links into a graph. Each page has compiled truth on top and an append-only, dated timeline below. Every fact needs a source, and contradictions are kept with both claims and flagged. About 2,300 files by mid-November.

By 2 Nov the operation had grown parallel memories that overlapped: a state file, a context file, verdicts, an intake log, knowledge pages and index facts. Corrections did not propagate, and agents answered from whichever file they found first.

## Method for the redesign: multi-model deliberation
Sol wrote down nine intentions first (one unified context; everything traceable to its source; distilled knowledge on top of raw data; correct once and the fix propagates; small task-specific "context frames"; and so on). Then:
1. Two strong models from different providers (Claude Fable and Astra) each drafted an architecture.
2. Each critiqued the other's draft.
3. One model decided, listing what was adopted from the critiques and what was rejected and why.

The decision: files in git are the truth; each kind of information has one canonical store and one writer; everything else is a generated view; the index holds no fact of its own; and one entry point (`frame.py`) builds a small, budgeted context slice for each task. Rejected ideas were recorded too (for example, "silently inferred standing approvals").

## Experiment (KN-17): does governed context make agents more correct?
- **20 golden questions** about the operation: 11 live metrics (follower counts, click rates and so on), 7 state or decision questions ("is this already pinned? should we ask Sol?"), 2 entity questions.
- **Truth computed independently** at the start and end of the run, from raw APIs and databases by a separate script. Text answers were scored by keyword groups, with no model judge.
- **Same model in every arm** (Claude Sonnet 5.5, max 30 turns, read-only tools).
- **Arms:** naive (repo plus shell, no dictionary); governed v1 (metric dictionary, entity file, a certified metrics reader, full context frame); v2a (a small task card only); v2b (task card plus the certified read run before the question).

| | Naive | Governed v1 | v2a card only | v2b card + pre-run read |
|---|---:|---:|---:|---:|
| Correct (of 20) | 12 | 20 | 19 | **20** |
| Live-metric questions (of 11) | 3 | 11 | 10 | 11 |
| Median context per question | 88,230 | 59,128 | 47,988 | **22,604** |
| Total context | 2.72M | 1.07M | 0.96M | **0.69M** |
| List-price cost | $2.31 | $1.60 | $1.23 | $1.09 |

## Lessons
- **The naive agent was wrong on metrics, not on state.** It got 9 of 9 state questions right, but only 3 of 11 metric questions: it found plausible numbers in old files and reported them as current.
- **Trimming the prompt alone barely helped.** Cutting the frame from 13.9K to 1.2K characters saved about 3K tokens per turn. What saved most was **removing a turn**: each turn re-sends a fixed harness overhead of about 22K tokens (system prompt plus tools plus repo instructions). Pre-running the certified read let the agent answer in one turn.
- There was a trade-off. Without the full context, state questions got more expensive (median 28K → 49K), because the agent went looking for files.
- A correct "N/A" from a transient API failure was scored wrong in one arm. It was the right behaviour; reporting a missing value as 0 would have been the wrong one. "Missing is never 0" became a standing rule.

## Limitations
20 questions about one operation, written by us. Tolerances let either snapshot count. One model.
