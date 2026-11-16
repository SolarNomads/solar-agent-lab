# How findings flow back into the system

[Back](README.md)

An experiment only counts if something changes because of it. This is the loop the team runs:

```
 observation (Sol on the phone, a log, a gate failure, a metric)
        │
        ▼
 question + measurement  ──► experiment write-up (numbers, dates, limits)
        │
        ▼
 finding (claim · evidence · confidence · what failed)  ──► knowledge base (findings store, rendered pages)
        │
        ▼
 change in the system:
   • a standing decision (numbered, overrides everything; changes need evidence)
   • a rule in the shared rulebook every harness loads
   • a playbook or skill (how to do the task next time)
   • a gate script (so the failure cannot ship again)
   • a seat change (which model fills which role)
        │
        ▼
 resume file (STATE.md) updated → the next session, in any model, starts from it
```

## Examples from these experiments

| Finding | Became |
|---|---|
| The editing model judged its own face edit "clearly different"; embeddings said same person ([E06](experiments/E06-self-evaluation-postmortem.md)) | Acceptance tests before builds; no self-sign-off; gates on every publish path |
| A naive agent reported stale metrics as current ([E02](experiments/E02-governed-context.md)) | A certified metrics reader, a metric dictionary, "missing is never 0", one context entry point |
| Cheap builders passed tests but missed the request ([E03](experiments/E03-green-tests-hidden-failures.md)) | Hidden request oracles, verifier outside the worker's reach, hash-frozen criteria, strict review rule, pre-registration |
| Resumed long-context agents burned 600k tokens on trivia ([E04](experiments/E04-where-oversight-pays.md)) | Token rules (fresh agents with briefs, caps, no polling), harness modes by quota |
| Judges answered their own question, not Sol's ([E05](experiments/E05-model-judges-vs-a-human.md)) | Criterion-first judging; a criteria registry Sol owns; a 200-pair bar |
| 892 of 893 logs linked to nothing ([E07](experiments/E07-auditing-an-agent-built-dashboard.md)) | A logging contract at the source; "unlinked" counts shown instead of hidden |
| The app's UI claimed things the logs contradicted ([E08](experiments/E08-solar-buddy-human-ai-interface.md)) | UI claims logged next to events; opt-in interaction replay; a one-page "how it sees" explainer |
| Untitled trial canaries performed weakly ([E09](experiments/E09-solar-list-growth-loop.md)) | "Never post an untitled trial" |

## Knowledge base design (private, described here)
- Pages carry compiled truth on top and a dated, append-only timeline below. Every fact has a source, and contradictions keep both claims.
- Files in git are the truth. One writer per kind of information; everything else is a generated view.
- A distilled findings store (claim, evidence, confidence with its basis, status, what failed) is rendered into topic pages, a contradictions page and an experiments page.
- A kill rule protects the distiller: if more than 20% of a week's new findings are marked "drop" in the weekly review, the distill job stops until its prompt is fixed.
- "No new knowledge" is a valid research outcome and is recorded as one.
