# E06. A model grading its own edit (postmortem)

[Back](../README.md) · 2–3 Nov 2026 · no-blame format

## What happened
For an Instagram carousel built from licensed photos of real people, Sol asked for the faces to be changed so the people would not be recognisable. The agent did it as a mild image-model edit of the face region: "different person, same pose, eyes and light; keep hair, headwear, ears and earrings". It then colour-matched and pasted the result back.

The quality check was the **same model's side-by-side judgement** ("clearly different"), plus a layout review. Mean pixel change in the face box was 6.2 out of 255. The carousel was posted by the scheduled job. Sol flagged the faces within the hour; clipped headline text was found on the same post.

## What measurement said afterwards
- Face-embedding similarity (SFace) between original and posted faces: 0.57, 0.62, 0.57, 0.51. The same-identity threshold is 0.363, so **all four were still the same person.**
- A blind test by a different model called all four "same".
- An ink check on the live slides found clipped headline descenders on 5 of 6 slides.

## Root causes
1. The requirement ("not recognisable") was never turned into a measurable acceptance test.
2. The prompt preserved the identity cues it was supposed to remove.
3. The model that made the edit also judged it.
4. No gate existed for faces or clipped text, so a scheduled job could post it.

## What changed
- Requirements become acceptance tests before the build: face identity is measured with embeddings against a threshold, and text clipping by an ink check.
- The same model never signs off on its own output; a different model or a script does.
- Every publish path runs the same gates, including scheduled ones.

## Why it matters
It is a small, concrete case of self-evaluation failure: the generator's judgement agreed with its own intent, not with the world. The fix that worked was not a better model but a measurement the generator could not influence.
