# Methods I hold myself to

[Back](README.md)

- **Write the question and the bar before the run.** For larger trials, register the comparator, the targets, the budget and the stop rule with hashes ([E03](experiments/E03-green-tests-hidden-failures.md), round 4). If a target changes, the report says so.
- **Hide the check from the worker.** Behaviour oracles are written from the request, validated on the parent and the original, and never shown to the builder.
- **Freeze test sets.** Hash them; tools refuse to overwrite them; never train or tune on them.
- **Score with scripts where possible.** Deterministic scorers, keyword groups and embeddings come before model judges. When a model judges, report its agreement with a human and an interval.
- **Separate plumbing failures from model failures.** A harness that never answered is not a model scoring zero.
- **Report intervals and n.** Wilson 95% intervals on agreement; n=1 is labelled n=1; small-sample wins are re-run before they are believed ([E05](experiments/E05-model-judges-vs-a-human.md)).
- **Tag claims by how they are known.** [L] logged, [I] inferred, [X] could not be determined. Used in the Solar buddy field study ([E08](experiments/E08-solar-buddy-human-ai-interface.md)).
- **No self-sign-off.** The model that produced something does not approve it ([E06](experiments/E06-self-evaluation-postmortem.md)).
- **Multi-model deliberation for big designs.** Two models from different providers draft, each critiques the other, and one decides and lists what was adopted and rejected ([E02](experiments/E02-governed-context.md)).
- **External audit.** For high-stakes setups, a second model reviews the plan, and each point is answered: adopted, tested or rejected with a reason.
- **No-blame postmortems** with a timeline, measured evidence, root causes and the gate that now prevents a repeat.
- **Costs from logs.** List-price estimates from token logs, labelled as estimates; the real spend was flat subscriptions.
- **Missing is never 0.** An unknown value is shown as unknown.
