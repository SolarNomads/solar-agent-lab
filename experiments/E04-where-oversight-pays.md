# E04. Where oversight pays

[Back](../README.md) · 11–13 Nov 2026

## Question
A typical agent-team setup puts an expensive model in charge and adds reviewers, QA agents and investigators. Which layers actually catch problems, and what do they cost?

## Data: one measured build session
Building Solar buddy 0.1 to 0.5.9 with a coordinator, Claude sub-agents, GPT-6.1 Terra builders and reviewers:
- 14 sub-agent runs used 5.1M tokens, 81% of them on Opus, with 1,078 tool calls and about 6.2 h of agent time. Terra's coding for the app cost about $23 at list price. **Building was the cheap part.**
- One feature (the sun pointing at an element on screen) took 7 versions and about 3.9M tokens, 77% of the session. Sol saw no pointing ring in 13 attempts on his phone until version 0.5.8.
- The feature failed for **four stacked reasons**: the ring was removed after 0.2 s; one entry point had no point action; another overlay hid the ring; and a server path never triggered it. Only the emulator QA agent and a fresh independent investigator, working from the phone's diagnostics, separated them.
- One parked run re-read a huge resumed context and spent 608k tokens writing a status note. Resuming long-context agents was the single largest waste.

## What paid off

| Layer | Paid off? | Evidence |
|---|---|---|
| Fresh investigator with real diagnostics | Yes, for device bugs | Found the four causes after 7 versions of guessing |
| QA on an Android emulator before release | Yes | First to show the ring lived 0.2 s |
| Mid-size reviewer (Sonnet) with a short brief | Yes, cheap | 26k tokens, 5 real findings ([E03](E03-green-tests-hidden-failures.md)) |
| Opus as a resumed app lead on small tweaks | No | 300k–600k tokens per small change |
| Several reviewers on small, scriptable changes | No | Cost more than the work |

## What changed
Token rules now in force for all agents: start fresh agents with a brief instead of resuming long contexts; cap reports at about 30 lines; never poll files; require a root cause proven from logs before rebuilding a failing feature; and run QA on the emulator before release rather than after. Harness "modes" (full, lean, saver) choose a setup by goal and by how much of the weekly Claude quota is left.

## Lesson
Builder errors did not cause most of the rework; misdiagnosis did. The oversight that paid was **independent** (fresh context, not the builder's story) and **grounded** (logs, emulator, hidden checks). Another pass of the same model reading the same transcript was not.

## Limitations
One product, one owner, one week. Per-version token attribution is approximate, because the logs are keyed by day.
