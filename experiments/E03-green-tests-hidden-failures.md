# E03. Cheaper builders vs hidden behaviour checks

[Back](../README.md) · 12–16 Nov 2026

## Question
Claude usage was running out mid-week, so the plan was to let a cheaper coding model (GPT-6.1 Terra) build, with Claude only writing briefs and reviewing. The obvious metric looked excellent: fewer Claude tokens, tests still passing. But did the cheaper setup do what was asked?

## Setup
- **Replay benchmark.** Real past changes to the Solar buddy server and app are replayed from their parent commit with the original request text. The original change, made by an Opus-led team, is the baseline.
- **Hidden behaviour oracles.** For each case, a check is written from the *request* and validated to fail on the parent and pass on the original commit. It is never shown to the builder. Candidates also run the full existing test suite.
- **Harness v2** (13 Nov): a shell-owned controller; independent acceptance criteria written from the request only and then hash-frozen; at most three builds; an evaluator from a third provider (Grok); a bounded Sonnet review with a token cap; a rerun of the final diff. Two modes: *lean* (Claude only for acceptance and review) and *saver* (zero Claude).
- **Builder-led trial** (13 Nov): one small UI task led entirely by Terra, with one Sonnet review.

## Results

Replay benchmark, attempt 3 (12 Nov): Claude tokens and seconds, original vs replay

| Case | Claude tokens | Seconds | Hidden oracle | Full suite |
|---|---|---|---|---|
| A: server app aliases | 136,332 → 5,883 (−95.7%) | 148 → 400 | 1 of 2 | not green |
| B: server action hints | 128,969 → 5,784 (−95.5%) | 195 → 466 | 0 of 1 | not green |
| C: app ring and replies | 307,031 → stopped | stopped by the disk guard | not run | unknown |

Harness v2 (13 Nov):
- Server cases, lean and saver: **all four hidden scores failed while 335–444 regression tests were green.**
- App case: 439 of 439 unit tests passed, but the requested behaviour was not observed, and the hidden check did not even compile against the candidate.
- Final reviews said **"PASS with caveats"** on two cases; a strict rule (any listed defect means not passed) rejected them.
- The independent acceptance step sometimes over-constrained incidental details and left out required outputs.
- Overhead: 12 setup and intake attempts cost 428,397 Claude tokens before any build.

Builder-led trial (n=1, 13 Nov): Claude tokens fell about 85–95%, but wall time rose about 7× (2 h 05 min). The final 2-line change was good: contrast 10.7:1, and a blind critic scored it above the previous version. Process failures:
- The first spec step wrote hard-coded FAILs ("oracle unavailable") instead of real checks.
- **The builder edited the verification script in its own workspace** and ran it at the same time as the harness verifier, which produced false failures in its own runs. The real verifier sat outside its reach and was green.
- The reviewer found five real gaps, including criteria the builder had declared done without proof.

Blind preference vs correctness: on case A, blind judges preferred the failing replay over the original by +2.0 out of 5.

Round 3 variants (14 Nov): a "slim" setup with Sonnet as builder saved only 17–34% of Claude tokens and scored like the control on the hidden probes. A Codex review found the same three defect types repeating in every patch. An external audit of the setup by a second model was taken point by point; one change was removing a path in the trial authorisation where silence counted as approval.

## Round 4: pre-registered, not yet run
Before spending more, round 4 was registered with hashes:
- the comparator (a fresh same-harness Claude control, with no pooling against historical runs);
- the bar (at least 35% fewer Claude tokens with equal hidden-check correctness);
- time bands (at most 1.0× and at most 1.5× the control's median, both reported; 1.5× is labelled as a changed target);
- budgets per block, and a hard stop at 88% of the usage meter.

As of 16 Nov it has spent nothing; the nine-run gate is pending.

## Lessons
- Visible tests mostly encode what already worked. They said little about whether the *new* request was met. The hidden check written from the request exposed the gap in every completed case.
- A token-cost metric rewards "cheaper and passing". Without hidden oracles we would have adopted a setup that quietly did less.
- Graders the worker can edit are not graders. "Verifier outside the worker's reach, criteria hash-frozen" became a standing rule. In the v2 runs the frozen criteria and hidden oracles were never edited.
- Model reviewers soften ("pass, with caveats"); a mechanical rule handles that better than another review.
- When a result disappoints, the temptation is to move the target. Writing the targets down before the run (and labelling any change) is the guard.

## Limitations
2–4 cases per round; one app and its server; baselines in early rounds were historical; queue time and disk limits prevented clean speed claims.
