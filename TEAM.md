# How the team is built

[Back](README.md)

## Who does what (November 2026)

| Role | Who | What it does |
|---|---|---|
| Owner | Sol (human, mostly from a phone) | Sets goals, approves anything public, paid or irreversible, labels taste data, tests apps on his phone |
| Orchestrator | Claude Opus 5.5 (in the Claude app, linked to the Mac) | Plans, writes briefs, dispatches execution agents, reviews, keeps the resume file current |
| Consultant | Claude Fable | Reads short prepared briefs and answers in text; used only for hard calls (at most about 2 briefs a week) |
| Operator | Hermes Agent on the Mac mini (open-source agent harness) | Always on: scheduled jobs (metrics, posting, inbox replies), signs in to sites, talks to Sol on Telegram |
| Builders | GPT-6.1 Terra via Codex CLI; Claude Sonnet 5.5 for small server fixes | Write code and media pipelines |
| Reviewers | Claude Sonnet 5.5 (diff and UX review), Codex review sessions | Independent review with short briefs |
| Scouts / research | Grok 4.7 (X and trend research), GPT-6.1 Sol and Astra 6.0 (specs, second opinions) | Research, cross-critique |
| Judges | Several models, scored against Sol's labels | Taste A/B judging (E05), blind design scoring |

Seats are assigned by benchmark, not habit ([E01](experiments/E01-which-model-for-which-role.md)). A seat changes only on a bench result or a quota incident, and the change is logged.

## How they talk to each other

- **One rulebook for every harness.** A single `AGENT-RULES.md` (12 rules plus where to read by task) is loaded automatically by Claude Code, Codex, Grok CLI and Hermes through pointer files. A smoke test asks each harness the same questions; all four answered correctly on 9 Nov 2026.
- **A resume file.** `STATE.md` holds what exists, what is in flight, the standing decisions and what needs Sol. Any model in any session starts there, so work survives session restarts and model switches.
- **A message folder ("harness chat").** For the harness-efficiency work, every message or delivery is a new file named `date-time-from-topic.md`, with `From / To / Re / Needs reply` on top. Nobody edits someone else's file. Between 13 and 16 Nov about 180 such files passed between the orchestrator, Codex review sessions and builders.
- **Consultant briefs.** One decision per brief, numbers and paths instead of logs, written to keep the expensive model's reading small. The consultant set its own criteria for when a brief is justified: a change to a standing decision, contradictory evidence that is costly to redo, a first-of-its-kind piece, two failed attempts, or ambiguous intent on something hard to undo.
- **Sync with the always-on operator.** Hermes appends an entry when a task for Sol finishes or blocks and reads guidance every 30 minutes, so the orchestrator and the operator do not drift apart while Sol talks to both.
- **A decision log.** About 200 lines of numbered decisions (`DECISIONS.md`) override every other file. A change to a decision needs evidence.

## What the team knows how to do (skills and playbooks)

- 39 team playbooks, 8 studio playbooks (reel from footage, animated reel, carousel, cover and caption, niche sourcing, short-form retention, creator lessons), 6 UX playbooks and handbooks, and 15 custom operator skills.
- Measurable design gates are written as scripts, not taste: text-safe zones, card fit and legibility (APCA contrast), card–voice sync, motion QC, tap-target and overflow checks for UI.
- A report format every agent uses: what was delivered, QC numbers, cost, what Sol must check, next step, in at most 15 lines.

## What we found each model is best at (see E01 and E03)

These are observations from this setup, with small samples:
- **Building well-specified code:** GPT-6.1 Terra matched the top scores at the lowest cost. On bigger, under-specified tasks it often stopped with requirements undone ([E03](experiments/E03-green-tests-hidden-failures.md)).
- **UX and design review:** Claude Sonnet 5.5 scored highest on UX review (0.79 against 0.51 for Terra). Sol also prefers Claude's design work, and that matched the blind scores.
- **Root-cause hunts on device bugs:** a fresh Claude Opus investigator working from real diagnostics. Expensive per run, but it ended 7 versions of guessing ([E04](experiments/E04-where-oversight-pays.md)).
- **Scouting and research on X:** Grok and GPT-6.1 Sol tied at 1.00 on the scout bench; Opus scored 0.50.
- **Judging taste:** no model yet ([E05](experiments/E05-model-judges-vs-a-human.md)).

## Guardrails that shape everything

- Nothing goes public, nothing is bought, and no new account is opened without Sol's explicit yes. The exception is proven content formats that pass every gate (Sol's standing rule from 10 Nov 2026).
- No secrets in chat, files or scripts; presence is checked, values are never printed.
- "Done" means deployed and verified live, not "code written".
- Agents that act on Sol's phone can only ask; only his tap grants access ([E08](experiments/E08-solar-buddy-human-ai-interface.md)).
