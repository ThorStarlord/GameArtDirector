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
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 8 security infrastructure | 8 | 10 | 8 | 9.4 | signage-tbd | SP-02, SP-07 | cameras, turnstiles, warning signs, observation windows. First image where function is visible: corridor answers WHY it exists (screening, access control, monitoring). Still not AEGIS-specific — reads as high security broadly. Signage drift (generic symbols). |
| 2026-06-03 | Z Image Turbo | aegis-wing | Test 9 full VN corridor | 9 | 9 | 9 | 9.5 | generic-identity | SP-01, SP-03, SP-05, SP-06 | First image with strong depth + atmosphere. Behavioral lighting (geometric pools, shadow zones, teal bleed, amber strip) finally worked — earlier tests used generic "cool blue-white" which is weak, but specific visible light behavior is strong. Composition solved (centered VP, receding darkness, 35% floor). Still organizationally generic: reads as secure facility broadly, not AEGIS specifically. |
| 2026-06-03 | Anima | aegis-wing | Test 9a full VN corridor (cross-model) | 8 | 9 | 7 | 9.3 | artifacts | SP-01, SP-03, SP-06 | Same prompt as Test 9 but model is Anima. Dramatically better atmosphere and darkness — light pools, shadows, depth all stronger. Weaker architectural precision, added artifacts and tiny figures in glass rooms. ZIT excels at architecture; Anima excels at atmosphere. See model-behavior.md. |
| 2026-06-04 | Z Image Turbo | checkpoint-lobby | Room test 1: checkpoint lobby | 8 | 9 | 8 | 9.2 | none | SP-02, SP-07 | Clear function, strong repetition, feels like a real place. "Checkpoint" produces obvious visible consequences (gates, scanners, lanes). Supports P1: model renders operational function better than organizational identity. |
| 2026-06-04 | Z Image Turbo | operations-center | Room test 2: operations center | 7 | 6 | 6 | 7.5 | CT-01 | none | Model interpreted as SOC/IT room with monitors. No institutional consequences — feels like startup or NOC, not government ops. "Operations center" has too many competing visual references. |
| 2026-06-04 | Z Image Turbo | operations-center | Room test 3: operations center (alt) | 6 | 4 | 5 | 6.5 | CT-01 | none | Weaker prompt, drifted to generic office. "Tiered workstations, status displays, consoles" doesn't constrain environment strongly enough. Model filled gaps with office prior. |
| 2026-06-04 | Z Image Turbo | intake-processing | Room test 4: metahuman intake facility | 8 | 6 | 7 | 7.0 | CT-12 | SP-05 | Admin functions (biometric stations, evidence counters, documentation kiosks) too abstract — model rendered locker room / changing facility instead. "Observation booths" + "containment" triggered storage compartments. Key: every function must be translated into visible furniture or infrastructure, not bureaucratic labels. |
| 2026-06-04 | Z Image Turbo | intake-processing | Room test 5: intake with concrete objects | 8 | 6 | 6 | 7.2 | CT-13 | SP-05 | Revise with concrete objects (fingerprint scanners, facial recognition terminals, evidence lockers, holding cell doors, transfer gates). Model rendered all objects correctly but collapsed scale to single containment module / superhuman storage locker. Objects implied "small compartment" not "facility." Progress: outputs are becoming specific, just converging on wrong scale. Need scene anchor first. |
| 2026-06-04 | Z Image Turbo | intake-processing | Room test 6: intake hall with scale anchor | 9 | 7 | 8 | 8.5 | CT-12 | SP-03 | Scale anchor worked — "large institutional intake hall, wide processing floor, central circulation corridor" produced hall instead of compartment. Spatial organization (rows, circulation path) respected. Composition VN-ready. But admin functions still drift: "biometric registration stations" → computer terminals, "evidence intake counters" → desks. Level 3 concepts (administrative functions) still fall back to generic office. Need to replace all abstract functions with physical objects (fingerprint pedestals, camera arches, turnstiles, numbered lockers). |
| 2026-06-04 | Z Image Turbo | intake-processing | Room test 7: object visibility test | 8 | 7 | 8 | 8.0 | CT-14 | SP-03 | Replaced all admin functions with physical objects (fingerprint pedestals, camera arches, turnstiles, numbered lockers, kiosks). Model understood scale/organization but interpreted screen-based terminals as arcade machines / ticket kiosks / airport check-in. Objects with strong consumer-context training data overwhelm security intent. Need security architecture (turnstiles, ballistic windows, cameras) integrated into the terminal layout to overpower consumer drift. |
| 2026-06-04 | Z Image Turbo | intake-processing | Room test 8: security architecture A/B | 8 | 7 | 7 | 8.2 | CT-14, CT-15 | SP-03 | Added security turnstiles, ballistic windows, cameras, checkpoint architecture. Consumer drift reduced — no longer arcade/airport, now reads as processing/screening facility. BUT: specific objects collapsed into "row of identical terminals." Model understands "processing infrastructure" generically but not individual components. Large procedural spaces cause simplification. Strategic question: is intake hall a major location or one-scene room? |
| 2026-06-04 | Z Image Turbo | intake-checkpoint | Room test 9: spatial staging checkpoint | 8 | 7 | 7 | 6.5 | CT-15 | SP-03 | Spatial staging (foreground/mid/background) partially worked — three large kiosks in FG, control room in MG, observation windows in BG. But: fingerprint pedestals → computer kiosks, turnstiles missing, no checkpoint lane structure. Model understands checkpoint as atmosphere but NOT as specific procedural hardware. Architecture > process infrastructure for this model. Recommend pivot to operations center (architectural space) next. |

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
