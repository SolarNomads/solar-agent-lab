# E07. Auditing a dashboard the agents built (Mission Control)

[Back](../README.md) · 3–11 Nov 2026

## Background
Mission Control is the phone dashboard where Sol sees the operation: jobs, metrics, spend, approvals, projects. Agents built it fast. On 10 Nov Sol reported from his phone that it felt untrustworthy: no "last updated" times, taps that did nothing, an old interface mixed in, and nothing linked to anything.

## Method
Instead of redesigning by opinion, we measured. A crawler inventoried every card and row on 18 routes at phone size (390×844) and tapped up to 6 non-link rows per route, recording what happened. Then a UX checker ran on every route: contrast with APCA, tap-target size, overflow, and console errors.

## Findings (before)
- **55%** of visible blocks (218 of 393) showed no time at all. Every spend row and every gear card was undated.
- **38 of 80** tested taps did nothing. Some rows looked tappable but had no handler: false affordances.
- 10 of 16 views were still the legacy interface inside the new shell.
- **Nothing linked:** 111 of 166 queue tasks had no project; **892 of 893** task-log rows matched no known task and session pair; 0 of 101 jobs carried a project; scheduled-job sessions carried no job id.

## What changed
- One UI and one source of truth per entity (project, task, run, spend, evidence, decision).
- **R1 Freshness:** every number shows when its data was read. Missing is shown as missing, never 0.
- Every row that looks tappable opens something; every metric opens a sheet with its value, as-of time, refresh cadence and the job that refreshes it.
- **A logging contract at the source:** every run carries project, task and session ids, so the 892 unlinked rows cannot recur. Until the gap closes, the dashboard shows "unlinked" counts per source instead of hiding them.
- Intelligence features (cost per deliverable, kill rules) wait until the links exist. Nothing is built on unlinked data.
- A shared component kit plus a UI gate script (contrast, taps, overflow, console, light and dark at 360 and 390 px) that every new page must pass, and a screenshot review by a reviewer model before "done".
- After the first fixes: on the 9 rebuilt routes, every number and rank row shows an as-of time.

## Lessons
- Agent-built interfaces fail in ways that are easy to measure and hard to see in a code review: undated numbers, dead taps, orphaned logs.
- Observability of the agents themselves (which run did what, for which project, at what cost) has to be designed at the source. Joining logs afterwards failed: 892 of 893 rows.
- A dashboard that shows a confident number with no date is worse than one that shows "unknown".

## Limitations
One dashboard, one user. Usability judged by the owner plus automated checks; no user study.
