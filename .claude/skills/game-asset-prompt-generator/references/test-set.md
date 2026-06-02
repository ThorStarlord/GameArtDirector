# Canonical Test Set

Use this set to evaluate regression whenever skill guidance changes.

## How to run

1. Generate all four prompt variants for each request.
2. Produce images with the same model and core settings when possible.
3. Score each request on readability, style fit, and production usability.
4. Record score deltas in prompt-corpus entries.

## Canonical requests (20)

1. poison potion icon
2. mana potion icon
3. iron longsword icon
4. arcane staff icon
5. forest goblin enemy sprite
6. skeleton enemy sprite
7. healing shrine prop
8. explosive barrel prop
9. fantasy inventory UI panel
10. quest journal window UI
11. fireball spell effect
12. ice shield spell effect
13. pixel-art village house
14. castle watchtower building
15. swamp ground tile
16. forest path tile
17. blacksmith workshop facade
18. treasure chest pickup sprite
19. poison cloud VFX burst
20. rune altar environment prop

## Lightweight scoring rubric

- Readability (1-10): Is the primary shape instantly clear at gameplay scale?
- Style alignment (1-10): Does it fit the intended genre or art direction?
- Production fitness (1-10): Is it easy to extract and integrate into a 2D asset pipeline?

Optional composite score:

$$
\text{Composite} = 0.4R + 0.3S + 0.3P
$$

Where:
- $R$ = Readability score
- $S$ = Style alignment score
- $P$ = Production fitness score