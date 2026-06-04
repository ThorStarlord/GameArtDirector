# Experiment Log

Raw generation results before they are distilled into the corpus and pattern catalogs.

## Log structure

Each experiment run produces one row per variant.

| Date | Model | Asset | Variant | R | S | P | Q | Failures | Successes | Notes |
|------|-------|-------|---------|---|---|---|---|----------|-----------|-------|

Where:
- **R** = Readability (1-10)
- **S** = Style alignment (1-10)
- **P** = Production fitness (1-10)
- **Q** = Result quality (1-10), overall composite judgment
- **Failures** = CT-XX codes observed
- **Successes** = SP-XX codes observed
- **Notes** = free text on what stood out or what changed

## Results

| Date | Model | Asset | Variant | R | S | P | Q | Failures | Successes | Notes |
|------|-------|-------|---------|---|---|---|---|----------|-----------|-------|
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 1 baseline | 7 | 5 | 7 | 8.5 | CT-01 | SP-01 | generic secure corridor, glass dominant, weak identity, good depth from light rhythm |
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 2 architectural vocab | 7 | 8 | 6 | 8.7 | CT-05 | SP-02 | strong institutional identity via repetition, lost depth (camera hugged wall), less VN-usable |
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 3 lighting structure | 8 | 4 | 7 | 7.8 | CT-01 | SP-01 | model ignored light-pool/shadow abstractions, produced uniform bright corridor, drifted to modern office. Lighting vocabulary is a weaker signal than architectural vocabulary for this model. |
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 4 architecture + lighting | 7 | 6 | 7 | 8.0 | CT-01, CT-11 | SP-02 | generic cool lighting pulled the image back toward office aesthetic; architecture still present but identity diluted. Adding generic lighting to architecture makes it worse than architecture alone. |
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 5 architecture + materials | 7 | 9 | 7 | 9.0 | none | SP-02 | best result so far. material cohesion visible, strong institutional identity, no office drift. depth still a weakness — image reads as elevation concept art more than walkable environment. |

## Analysis prompts

After logging 10+ rows, ask:

- Which failure pattern has the highest frequency?
- Which correction correlates with score improvement?
- Which variant scores highest on average?
- Which asset type is hardest to get right?
- Are any CT codes never observed (possible candidates for removal)?
- Are any SP codes correlated with Q >= 8?
- Which prompt signal types produce the strongest model response? (Early evidence: concrete countable objects > abstract spatial descriptions)

## Emerging signal hierarchy (preliminary, 5 tests)

| Signal type | Avg score | Responsiveness |
|-------------|-----------|----------------|
| Architecture + Materials | 9.0 | Strong — best result |
| Architecture alone | 8.7 | Strong — clear identity |
| Baseline (no signals) | 8.5 | — |
| Architecture + Lighting | 8.0 | Weak — lighting dilutes architecture |
| Lighting alone | 7.8 | Weak — office drift

Enter answers into the relevant catalog headers.
