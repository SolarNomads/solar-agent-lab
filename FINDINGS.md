# What holds across experiments

[Back](README.md)

These are working conclusions from one operation, not general laws.

1. **Green tests are not the requirement.** Builders passed hundreds of regression tests while every hidden check of the actual request failed ([E03](experiments/E03-green-tests-hidden-failures.md)).
2. **Generators should not grade themselves.** A model judged its own face edit "clearly different"; measurement said same person ([E06](experiments/E06-self-evaluation-postmortem.md)). A builder edited its own copy of the verifier ([E03](experiments/E03-green-tests-hidden-failures.md)).
3. **Preference is not correctness.** Blind judges preferred a failing candidate by +2 of 5 ([E03](experiments/E03-green-tests-hidden-failures.md)).
4. **What a judge is asked decides whether it agrees with a human, and small-sample agreement is fragile.** Chance → 65.9% by fixing the question; 72.9% on 24 pairs → 54–56% on 86 ([E05](experiments/E05-model-judges-vs-a-human.md)).
5. **The right slice of context beats more context.** 60% → 100% correct with a quarter of the tokens; the biggest saving was removing a turn, not trimming text ([E02](experiments/E02-governed-context.md)).
6. **Oversight pays when it is independent and grounded.** Fresh investigators with real diagnostics and emulator QA found what re-reading could not ([E04](experiments/E04-where-oversight-pays.md)).
7. **Interfaces can mislead even when the agent is right.** Seven contradictions between what the app said and what happened ([E08](experiments/E08-solar-buddy-human-ai-interface.md)); undated numbers and dead taps on the dashboard ([E07](experiments/E07-auditing-an-agent-built-dashboard.md)).
8. **Approval must live outside the agent's action space.** On the phone, only a human tap grants. In the trial authorisation, a path where silence counted as approval was found and removed ([E03](experiments/E03-green-tests-hidden-failures.md), [E08](experiments/E08-solar-buddy-human-ai-interface.md)).
9. **Cheap benchmarks flatter cheap models.** Every builder scored 1.00 on easy scripted tasks ([E01](experiments/E01-which-model-for-which-role.md)); real tasks with hidden checks separated them ([E03](experiments/E03-green-tests-hidden-failures.md)).
10. **Outcomes come before understanding.** The account grew 297 → 7,070 followers, but which choices caused it is still an open experiment ([E09](experiments/E09-solar-list-growth-loop.md)).
