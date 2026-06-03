# Canonical Test Set

Use this set to evaluate regression whenever skill guidance changes and to build the evidence base.

## How to run

1. Generate all four prompt variants for each request (prompts are in `prompt-corpus.md`)
2. Produce images with the same model and core settings when possible
3. Score each variant on readability, style fit, and production usability
4. Record failure codes (CT-XX) and success codes (SP-XX) per variant
5. Fill result quality scores into the corpus entry
6. When a failure pattern has been observed 5+ times with consistent fix outcomes, promote its status from Observed to Proven

## Canonical requests (20)

| # | Entry label | Asset type | Status |
|---|-------------|-----------|--------|
| 1 | poison-potion-icon | icon | seeded |
| 2 | mana-potion-icon | icon | seeded |
| 3 | iron-longsword-icon | icon | seeded |
| 4 | arcane-staff-icon | icon | seeded |
| 5 | forest-goblin-enemy-sprite | enemy sprite | seeded |
| 6 | skeleton-enemy-sprite | enemy sprite | seeded |
| 7 | healing-shrine-prop | prop | seeded |
| 8 | explosive-barrel-prop | prop | seeded |
| 9 | fantasy-inventory-panel | UI | seeded |
| 10 | quest-journal-ui | UI | seeded |
| 11 | fireball-spell-effect | VFX | seeded |
| 12 | ice-shield-effect | VFX | seeded |
| 13 | pixel-art-village-house | building | seeded |
| 14 | castle-watchtower | building | seeded |
| 15 | swamp-ground-tile | environment tile | seeded |
| 16 | forest-path-tile | environment tile | seeded |
| 17 | blacksmith-facade | building | seeded |
| 18 | treasure-chest-sprite | pickup sprite | seeded |
| 19 | poison-cloud-vfx | VFX | seeded |
| 20 | rune-altar-prop | prop | seeded |

## Scoring rubric

- **Readability (1-10):** Is the primary shape instantly clear at gameplay scale?
- **Style alignment (1-10):** Does it fit the intended genre or art direction?
- **Production fitness (1-10):** Is it easy to extract and integrate into a 2D asset pipeline?
- **Result quality (1-10):** Overall composite judgment of the generation

Optional composite formula:

```
Composite = 0.4R + 0.3S + 0.3P
```

Where:
- R = Readability score
- S = Style alignment score
- P = Production fitness score

## Evidence pipeline

After generating all 20 test set items, aggregate:

1. Which failure patterns appeared most frequently?
2. Which corrections reliably fixed them?
3. Which success patterns correlated with high result quality scores?
4. Which variant type scored highest on average?

Update `result-analysis.md` and `success-patterns.md` status levels and evidence counts after each batch of 10 generations.
