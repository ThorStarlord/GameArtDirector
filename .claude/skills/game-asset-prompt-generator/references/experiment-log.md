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
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 6 government facility identity | 7 | 10 | 7 | 9.2 | none | SP-02 | access control became architectural theme. negative identity constraints effective. first image that reads as restricted-access facility rather than secure hallway. still flat — identity strong, environmental storytelling weak. |
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 7 narrative function | 7 | 6 | 6 | 8.3 | CT-01 | SP-02 | model ignored high-level narrative (containment, oversight, clearance). translated "administrative" into office. introduced desks/workstations. Narrative/functional language is a weak signal. Key finding: purpose must be translated into visible artifacts, not abstract concepts. |

## Analysis prompts

After logging 10+ rows, ask:

- Which failure pattern has the highest frequency?
- Which correction correlates with score improvement?
- Which variant scores highest on average?
- Which asset type is hardest to get right?
- Are any CT codes never observed (possible candidates for removal)?
- Are any SP codes correlated with Q >= 8?
- Which prompt signal types produce the strongest model response? (Early evidence: concrete countable objects > abstract spatial descriptions)

## Emerging signal hierarchy (preliminary, 7 tests)

| Signal type | Score | Responsiveness |
|-------------|-----------|----------------|
| Arch + Materials + Identity | 9.2 | Strong — access control became theme |
| Architecture + Materials | 9.0 | Strong — material cohesion |
| Architecture alone | 8.7 | Strong — clear identity |
| Baseline (no signals) | 8.5 | — |
| Narrative function added | 8.3 | Weak — model ignores abstract purpose, translates to office |
| Architecture + Lighting | 8.0 | Weak — lighting dilutes architecture |
| Lighting alone | 7.8 | Weak — office drift

Enter answers into the relevant catalog headers.

## Key discovery (7 tests)

> The model responds to things it can draw, not things it has to infer.
>
> High-level narrative purpose → weak signal. Must be translated into visible architectural consequences (checkpoints, observation windows, hardware) before the model will render it.
>
> Signal strength ranking: physical constraints (doors, readers, materials, segmentation) > identity constraints (negative space, access control theme) > functional narrative (purpose, classification, oversight) > atmospheric lighting (mood, shadow, beam structure).
