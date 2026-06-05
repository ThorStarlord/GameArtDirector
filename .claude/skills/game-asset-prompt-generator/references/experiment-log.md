# Experiment Log

Raw generation results before they are distilled into the corpus and pattern catalogs.

## Log structure

Each experiment run produces one row per variant.

**Track A — Environment Design:** Tests model capability — can it understand worldbuilding concepts? Architecture, lighting, materials, spatial organization. Used for developing prompting principles.

**Track B — VN Background Production:** Tests asset usability — does the image work as a VN background? Sprite space, readable composition, reusable perspective, low visual noise. Used for production asset validation.

| Date | Model | Asset | Variant | Track | R | S | P | Q | Failures | Successes | Notes |
|------|-------|-------|---------|-------|---|---|---|---|----------|-----------|-------|

Where:
- **R** = Readability (1-10)
- **S** = Style alignment (1-10)
- **P** = Production fitness (1-10)
- **Q** = Result quality (1-10), overall composite judgment
- **Failures** = CT-XX codes observed
- **Successes** = SP-XX codes observed
- **Notes** = free text on what stood out or what changed

## Results

| Date | Model | Asset | Variant | Track | R | S | P | Q | Failures | Successes | Notes |
|------|-------|-------|---------|-------|---|---|---|---|----------|-----------|-------|
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 1 baseline | A | 7 | 5 | 7 | 8.5 | CT-01 | SP-01 | generic secure corridor, glass dominant, weak identity, good depth from light rhythm |
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 2 architectural vocab | A | 7 | 8 | 6 | 8.7 | CT-05 | SP-02 | strong institutional identity via repetition, lost depth (camera hugged wall), less VN-usable |
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 3 lighting structure | A | 8 | 4 | 7 | 7.8 | CT-01 | SP-01 | model ignored light-pool/shadow abstractions, produced uniform bright corridor, drifted to modern office. Lighting vocabulary is a weaker signal than architectural vocabulary for this model. |
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 4 architecture + lighting | A | 7 | 6 | 7 | 8.0 | CT-01, CT-11 | SP-02 | generic cool lighting pulled the image back toward office aesthetic; architecture still present but identity diluted. Adding generic lighting to architecture makes it worse than architecture alone. |
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 5 architecture + materials | A | 7 | 9 | 7 | 9.0 | none | SP-02 | best result so far. material cohesion visible, strong institutional identity, no office drift. depth still a weakness — image reads as elevation concept art more than walkable environment. |
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 6 government facility identity | A | 7 | 10 | 7 | 9.2 | none | SP-02 | access control became architectural theme. negative identity constraints effective. first image that reads as restricted-access facility rather than secure hallway. still flat — identity strong, environmental storytelling weak. |
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 7 narrative function | A | 7 | 6 | 6 | 8.3 | CT-01 | SP-02 | model ignored high-level narrative (containment, oversight, clearance). translated "administrative" into office. introduced desks/workstations. Narrative/functional language is a weak signal. Key finding: purpose must be translated into visible artifacts, not abstract concepts. |
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 8 security infrastructure | A | 8 | 10 | 8 | 9.4 | signage-tbd | SP-02, SP-07 | cameras, turnstiles, warning signs, observation windows. First image where function is visible: corridor answers WHY it exists (screening, access control, monitoring). Still not AEGIS-specific — reads as high security broadly. Signage drift (generic symbols). |
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 9 full VN corridor | B | 9 | 9 | 9 | 9.5 | generic-identity | SP-01, SP-03, SP-05, SP-06 | First image with strong depth + atmosphere. Behavioral lighting (geometric pools, shadow zones, teal bleed, amber strip) finally worked. Composition solved (centered VP, receding darkness, 35% floor). Still organizationally generic. |
| 2026-06-03 | Anima | aegis-wing | Test 9a full VN corridor (cross-model) | B | 8 | 9 | 7 | 9.3 | artifacts | SP-01, SP-03, SP-06 | Same prompt as Test 9 but model is Anima. Better atmosphere/darkness but weaker architectural precision. See model-behavior.md. |
| 2026-06-04 | Z Image Turbo | checkpoint-lobby | Room test 1: checkpoint lobby | A | 8 | 9 | 8 | 9.2 | none | SP-02, SP-07 | Clear function, strong repetition. "Checkpoint" produces obvious visible consequences (gates, scanners, lanes). |
| 2026-06-04 | Z Image Turbo | operations-center | Room test 2: operations center | A | 7 | 6 | 6 | 7.5 | CT-01 | none | Model interpreted as SOC/IT room. No institutional consequences. |
| 2026-06-04 | Z Image Turbo | operations-center | Room test 3: operations center (alt) | A | 6 | 4 | 5 | 6.5 | CT-01 | none | Weaker prompt, drifted to generic office. |
| 2026-06-04 | Z Image Turbo | intake-processing | Room test 4: metahuman intake facility | A | 8 | 6 | 7 | 7.0 | CT-12 | SP-05 | Admin functions too abstract — model rendered locker room. |
| 2026-06-04 | Z Image Turbo | intake-processing | Room test 5: intake with concrete objects | A | 8 | 6 | 6 | 7.2 | CT-13 | SP-05 | Objects rendered correctly but scale collapsed to containment module. |
| 2026-06-04 | Z Image Turbo | intake-processing | Room test 6: intake hall with scale anchor | A | 9 | 7 | 8 | 8.5 | CT-12 | SP-03 | Scale anchor worked but admin functions still drift to office. |
| 2026-06-04 | Z Image Turbo | intake-processing | Room test 7: object visibility test | A | 8 | 7 | 8 | 8.0 | CT-14 | SP-03 | Screen-based terminals → consumer kiosks. |
| 2026-06-04 | Z Image Turbo | intake-processing | Room test 8: security architecture A/B | A | 8 | 7 | 7 | 8.2 | CT-14, CT-15 | SP-03 | Consumer drift reduced but objects collapsed to identical rows. |
| 2026-06-04 | Z Image Turbo | intake-checkpoint | Room test 9: spatial staging checkpoint | A | 8 | 7 | 7 | 6.5 | CT-15 | SP-03 | Spatial staging partial success. Hardware → generic kiosks. |
| 2026-06-04 | Z Image Turbo | operations-center | Room test 10: metahuman command center | A | 9 | 5 | 7 | 8.0 | structure-collapse | none | Atmosphere 9/10, structure 5/10. Multi-level collapsed. |
| 2026-06-04 | Ideogram 4 | containment-corridor | Ideogram Test 1: corridor with 4 regions (floor/doors/glass/ceiling) | A | 7 | 8 | 8 | 8.5 | depth-flat | SP-08 | Regions strongly obeyed (floor 10/10, doors 9/10, glass 9/10, ceiling 10/10) but corridor depth weak (3/10). Front-facing institutional room instead of hallway. Style config changed to VN profile — convincing VN art style. |
| 2026-06-04 | Ideogram 4 | intake-office | Ideogram Test 2: intake office with 4 regions (floor/counters/windows/ceiling) | A | 8 | 9 | 8 | 9.0 | none | SP-08 | Regions assembled into coherent waiting room despite no "waiting room" in prompt. Region descriptions (registration counters, observation windows) dominated composition. VN style consistent. |
| 2026-06-04 | Ideogram 4 | operations-center | Ideogram Test 3: command center regions overrode corridor HLD | A | 8 | 7 | 7 | 8.0 | scene-conflict | SP-08 | Regions (workstations, displays, gallery) overpowered "containment corridor" HLD. Model produced convincing VN operations center but wrong room type. Proves regions > HLD in hierarchy. |
| 2026-06-04 | Ideogram 4 | towering-corridor | Ideogram Test 4: towering corridor (vertical + depth mixed) | A | 7 | 9 | 8 | 8.8 | CT-18 | SP-08 | Vertical cues (multi-story, catwalks, conduits, low angle) overpowered circulation cues. Reads as vertical transit spine / interior canyon, not corridor. Monumental architecture 10/10, corridor readability 6/10. Strongest AEGIS atmosphere yet but wrong room type. |

## Analysis prompts

After logging 10+ rows, ask:

- Which failure pattern has the highest frequency?
- Which correction correlates with score improvement?
- Which variant scores highest on average?
- Which asset type is hardest to get right?
- Are any CT codes never observed (possible candidates for removal)?
- Are any SP codes correlated with Q >= 8?
- Which prompt signal types produce the strongest model response? (Early evidence: concrete countable objects > abstract spatial descriptions)

## Emerging signal hierarchy (preliminary, 9 tests)

| Signal type | Score | Responsiveness |
|-------------|-----------|----------------|
| Full VN corridor (behavioral lighting + composition + materials) | 9.5 | Strong — first image with depth + atmosphere + readability |
| Security infrastructure | 9.4 | Strong — cameras, turnstiles, signage immediately rendered |
| Arch + Materials + Identity | 9.2 | Strong — access control became theme |
| Architecture + Materials | 9.0 | Strong — material cohesion |
| Architecture alone | 8.7 | Strong — clear identity |
| Baseline (no signals) | 8.5 | — |
| Narrative function added | 8.3 | Weak — model ignores abstract purpose, translates to office |
| Architecture + Lighting (generic) | 8.0 | Weak — generic lighting dilutes architecture |
| Lighting alone (generic) | 7.8 | Weak — office drift

## Key discovery (9 tests, with important refinement)

> The model responds to things it can draw, not things it has to infer.

**Refinement — lighting signal strength depends on specificity:**
- Generic lighting ("cool blue-white architectural lighting") → weak signal, office drift
- Behavioral lighting ("geometric pools of light at 4m intervals, deep shadow zones between each pool, cool blue cast, ambient teal glow through glass") → strong signal, creates atmosphere

The earlier tests (3, 4) used generic lighting language. Test 9 used behavioral lighting language. The difference is significant. This refines CT-11: the failure is not "lighting descriptions" but "generic lighting descriptions without visible behavior."

**Signal strength ranking (refined):**
physical constraints (doors, readers, materials) > behavioral lighting (pools, shadows, specific cast colors) > identity constraints (negative space, access control theme) > functional narrative (purpose, classification) > generic atmospheric lighting (mood adjectives without visual behavior)
