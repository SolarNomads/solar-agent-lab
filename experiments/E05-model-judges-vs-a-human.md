# E05. Model judges vs a human's taste

[Back](../README.md) · 9–11 Nov 2026 (ongoing)

## Question
Content work runs on judgements like "which cover makes you tap" or "which reel would you watch to the end". Taste is the weakest point of the agents. If a model judge could reproduce Sol's choices, it could pre-rank variants at scale. Can it? And what changes the answer?

## Setup ("Taste Brain")
- **Labels.** Sol answers A/B pairs on a phone page: A, B, both good (with a tie-break), both bad, or skip. Each answer can carry reason chips and a free-text "why".
- **Typed pairs.** Cover; reel (8 frames in order plus a 2-line story); story text; lettering; colour; transition; comment. A validator enforces the matching rules: sides come from different posts, the same topic family, randomised position, and no view counts or creator names are shown.
- **Frozen test sets** are hashed; the build tool refuses to overwrite them; they are never trained or tuned on.
- **Outcome pairs.** The winner is known from public performance, judged relative to each creator's own follower count and median reach rather than raw views. Sources: 1,360 reels from 361 creators.
- **Criterion-first judging.** Each run fixes one criterion (for example "which is more catchy" or "which makes you curious enough to tap"), and the judge's context holds that criterion only.

## Results
- Gold v1, 60 pairs: judges asked a *different* question from Sol's agreed at chance (46.7–51.9%). Asked his *exact* question, the best judges reached 27 of 41 (65.9%, interval 50.5–78.4).
- Outcome pairs v0, 24 pairs (10 Nov): the best judge reached 72.9% (52.9–86.6).
- **Outcome pairs v1, 86 pairs (11 Nov): Terra 54% (44–64) on 86 pairs; Grok 56% (42–69) on 49. Neither clears chance. The v0 result did not replicate.**
- Running digest against Sol's labels: Sonnet 56.0% agreement on 182 judgements (48.8–63.1).
- **Judges bring their own objective.** Reading the judges' reasons, Sol saw one judge choosing what it thought was better *for the account* instead of answering the question asked. That observation is why judging became criterion-first.
- An audit found an earlier "108 of 108 pairs on-niche" check was too lenient: 37 of 108 under a strict rule. It was corrected.

## Lessons
- "Taste agreement" is not a fixed property of a model. It moved from chance to above chance just by changing the question, and then fell back to chance on a harder, outcome-based set.
- Small-sample wins evaporate. The 24-pair 72.9% looked like progress; 86 pairs said otherwise. The bar now is at least 200 held-out pairs per niche before any judge pre-ranks anything.
- A judge's free-text reasons are data. They revealed a hidden objective that the scores alone would not have shown.
- Taste data has to be specific to a niche. Mixing everything Sol likes produced pairs that taught nothing.

## Limitations
One main labeller (a guest-link page exists, but friend labels so far: 6); small samples; niche content (Indian temples and pilgrimage); public performance metrics are themselves noisy.
