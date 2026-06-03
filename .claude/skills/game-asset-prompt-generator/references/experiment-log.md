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
|      |       |       |         |   |   |   |   |          |           |       |

## Analysis prompts

After logging 10+ rows, ask:

- Which failure pattern has the highest frequency?
- Which correction correlates with score improvement?
- Which variant scores highest on average?
- Which asset type is hardest to get right?
- Are any CT codes never observed (possible candidates for removal)?
- Are any SP codes correlated with Q >= 8?

Enter answers into the relevant catalog headers.
