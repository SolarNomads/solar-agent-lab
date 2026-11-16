# E10. Hatha Notes: an app built mostly by a coding model

[Back](../README.md) · October–November 2026 (ongoing)

## What it is
An offline-first notes app for classical hatha yoga practice: scanned handwritten pages, on-device text recognition, audio recitation, practice boards and focus timers. It runs on the web and as an Android beta. The web version is the UX test bed.

## The experiment
Can a coding model (GPT-6.1 Terra via Codex) carry a real app from release to release, with a Claude orchestrator only setting UX direction and reviewing evidence?

## How releases work
- Each release ships with a **handoff document**: the source commit, test results, deploy links (production, an immutable deployment, a preview), the rollback target, the APK size and hash, and what was *not* checked.
  - Example, hn-v61 (9 Nov): clean install, build and 213 unit tests passed; the Android beta was rebuilt with the prior signing identity; "Installation on Sol's phone was not checked."
- Releases hn-v56 (8 Nov) to hn-v69 (16 Nov) shipped, 14 in 9 days, with about 120 commits to the app since 1 Nov.
- UX direction from the orchestrator: an OS-native look, a component handoff template, and a ship-and-verify playbook.

## Findings
- **On-device OCR (Google ML Kit, Android) is far better than the web OCR (Tesseract)** on Sol's handwritten and printed pages. We stopped judging OCR on the web; the web is for UX only.
- The builder model was reliable on well-specified tickets with release evidence. It was weaker on UX judgement without a design pack. This matches [E01](E01-which-model-for-which-role.md) (UX review 0.51 vs 0.79), and led to a written design pack and mock-spec template for GPT builders.
- Writing "what was not checked" into every handoff surfaced real gaps, such as installs that were never tested on the phone, that a "done" message would have hidden.

## Limitations
No formal measurement of user outcomes; one user. Release cadence is not quality. The evidence here is process evidence (tests, hashes, rollbacks), not a controlled comparison with a Claude-built version.
