# Solar Agent Lab

**Field notes from building and studying a small team of AI agents that does real work.**
Sol (Nicolas Martinez) · Medellín · [LinkedIn](https://www.linkedin.com/in/meetnico) · October–November 2026

Since late October 2026 I have run a team of AI agents from different providers on one Mac mini. The team runs an Instagram account, two websites, a notes app, an Android companion app, and its own control dashboard. I am not an academic. I come from business development and from guiding expeditions in the Amazon, where you learn to test what you think you know against the terrain. I treat this operation the same way: every time the agents fail or surprise me, I turn it into a question, a measurement, and a change to the system.

This repository collects those experiments. It covers how the team is built, what each model turned out to be good at, how we found out, and what did not work. It holds no private data: the knowledge base, accounts and app source stay private. What is here are the methods, numbers, failures and lessons.

## The questions I keep coming back to

1. **Did the agent actually do what was asked?** Tests pass, reviews say "pass", and the requested behaviour is still missing. How do you catch that cheaply?
2. **Can a model stand in for a human's judgement?** It might stand in for taste, for "is this a different person", or for "is this finished". On which question, and how do you know?
3. **What context does an agent need to be right?** More of it, or the right slice?
4. **How should a human and an agent talk?** Voice, pointing, recording, consent. And what happens when the interface tells the human something different from what happened?
5. **Where does oversight pay?** Reviewers, QA agents and investigators cost tokens. Which ones catch real problems?

## Experiments

| # | Experiment | Headline result | Status |
|---|---|---|---|
| [E01](experiments/E01-which-model-for-which-role.md) | Which model for which role (seat benchmark) | Cheapest model tied the top score on writing, mining and building; a mid-size model led critique and UX review | Done, small n |
| [E02](experiments/E02-governed-context.md) | Governed context vs a naive agent (the brain) | 60% → 100% correct on 20 questions, with about 25% of the context | Done |
| [E03](experiments/E03-green-tests-hidden-failures.md) | Cheaper builders vs hidden behaviour checks | Hundreds of green tests; every hidden check of the actual request failed | Three rounds done; round 4 pre-registered |
| [E04](experiments/E04-where-oversight-pays.md) | Where oversight pays | 77% of a session's tokens went to one feature; fresh independent investigators found the causes | Done |
| [E05](experiments/E05-model-judges-vs-a-human.md) | Model judges vs a human's taste | A 73% result did **not** replicate (54% and 56% on 86 pairs) | Ongoing |
| [E06](experiments/E06-self-evaluation-postmortem.md) | A model grading its own edit | "Clearly a different person" per the editing model; face embeddings said same person | Postmortem |
| [E07](experiments/E07-auditing-an-agent-built-dashboard.md) | Auditing a dashboard the agents built | 38 of 80 taps did nothing; 892 of 893 log rows linked to nothing | Done; fixes shipped |
| [E08](experiments/E08-solar-buddy-human-ai-interface.md) | Solar buddy: a human–AI interface on the phone | Logs showed 7 places where the app told the user something different from what happened | Ongoing field study |
| [E09](experiments/E09-solar-list-growth-loop.md) | The @solar_list growth and trial loop | 297 → 7,070 followers (29 Oct to 15 Nov); single-variable trial experiments still have no outcome data | Ongoing |
| [E10](experiments/E10-hatha-notes-builder-led-app.md) | Hatha Notes: an app built mostly by a coding model | 14 releases in about 9 days with release evidence; on-device OCR far stronger than the web OCR | Ongoing |

Also: [TEAM.md](TEAM.md) (how the team is built and how the agents talk), [LEARNING-LOOP.md](LEARNING-LOOP.md) (how findings flow back into the system), [METHODS.md](METHODS.md) (rules I hold myself to), [FINDINGS.md](FINDINGS.md) (what holds across experiments), [OPEN-QUESTIONS.md](OPEN-QUESTIONS.md) (what I would study next with proper resources).

## How to read the numbers

- Every number has a date and a sample size. Most samples are small; I say so where it matters.
- Agreement rates carry 95% Wilson intervals. n=1 trials are labelled n=1.
- Costs are list-price estimates from token logs. The real spend was three flat subscriptions.
- Model names are the ones in use at the time (Claude Opus 5.5, Claude Sonnet 5.5, Claude Fable, GPT-6.1 Terra, GPT-6.1 Sol, Astra 6.0, Grok 4.7, and the Hermes Agent harness). Results are about these models *in this setup*, not general claims about them.
- Raw logs, run receipts and the private knowledge base can be shown on a screen share.

## Honesty notes

- This is one operator, one machine, a few weeks. It is a field notebook, not a benchmark paper.
- Several results are negative or did not replicate; they are reported as such.
- The write-ups were assembled by the team's orchestrator agent from the run records and then checked against them. The experiments, decisions and direction are mine.
