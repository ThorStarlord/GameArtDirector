# Evidence-Backed Principles

These principles are derived from controlled prompt experiments (8 tests, 1 model) rather than theory. They represent what *actually worked* for generating institutional environment backgrounds, ranked by signal strength.

---

## P1: Visible Consequences Principle

**Core idea:** The model renders visible infrastructure reliably but largely ignores abstract organizational concepts.

**Don't write:**
- "government oversight facility"
- "containment center"
- "administrative processing wing"
- "authorization and containment"

**Write instead:**
- turnstiles and security checkpoints
- security cameras (visible, positioned)
- observation windows (reinforced, positioned)
- warning signage (mounted on walls/doors)
- clearance reader terminals at every access point

**Evidence:** Test 7 (narrative function) scored 8.3. Test 8 (security infrastructure) scored 9.4. The only change was translating abstract purpose into visible hardware.

**Related SP:** SP-07

---

## P2: Architectural Rhythm Principle

**Core idea:** Repeated physical structures (doors, readers, wall modules) create stronger institutional identity than any mood or style language.

**Don't write:**
- "institutional atmosphere"
- "secure facility feel"
- "bureaucratic architecture"

**Write instead:**
- "repeating sealed doors at regular intervals"
- "repeating access-reader cadence at every door"
- "wall panel segmentation rhythm along the full length"
- "periodic structural columns dividing wall sections"

**Evidence:** Test 2 (architecture alone) scored 8.7, outperforming Test 3 (lighting only at 7.8). Repetition alone did more work than any other single signal.

**Related SP:** SP-02

---

## P3: Material Language Principle

**Core idea:** Specific material names with surface qualifiers produce more consistent results than generic architectural style labels.

**Don't write:**
- "cold futuristic building"
- "modern institutional design"
- "high-end corporate facility"

**Write instead:**
- "matte charcoal wall panels"
- "anti-reflective coated surfaces"
- "brushed metal trim and hardware"
- "cold slate-blue primary wall material"
- "sealed concrete floor, polished"

**Evidence:** Test 5 (architecture + materials) scored 9.0, the highest before security infrastructure was added. Materials amplified the effect of architectural repetition without diluting it.

---

## P4: Negative Identity Principle

**Core idea:** Explicitly stating what a room is NOT constrains the model more effectively than relying on what it IS to be distinctive enough.

**Do write — per-element exclusion pairs:**
- "glass reveals abstract glow only, not a visible workstation interior"
- "transit corridor, not an operations room or lab"
- "no office furniture, no laboratory equipment, no public-facing spaces"

**Evidence:** Test 6 (negative identity constraints) scored 9.2. The "no office, no lab, no public" exclusion appeared to suppress office drift more effectively than positive identity language alone.

**Related SP:** SP-02 (negative identity amplified the repetition effect)

---

## P5: Spatial Zone Principle

**Core idea:** Percentage-based spatial constraints reserve critical frame areas that the model would otherwise populate with unwanted objects.

**Do write:**
- "bottom 35%: open uncluttered floor, no foot-level objects"
- "clear foreground band for character/dialogue placement"

**Evidence:** Test 2 (architectural vocab without spatial constraints) scored lower on production fitness (6/10) because the wall-hugging composition left no usable VN space. Tests that omitted spatial reservation required compositing workarounds.

**Related CT:** CT-05

---

## P6: Batch Anchor Principle

**Core idea:** A shared identity anchor prepended to every prompt in a batch prevents model drift between related assets, and structural landmarks (door cadence, panel rhythm) are more reliable continuity signals than palette.

**Anchor layers (in order of reliability):**
1. Shared landmark patterns — strongest (doors, readers, panels, safety strips)
2. Shared architectural vocabulary — strong (wall segmentation, ceiling treatment)
3. Shared material palette — moderate (surface names, color qualifiers)
4. Shared lighting logic — weakest (model may not reliably reproduce it)

**Related CT:** CT-09

---

## Signal Strength Hierarchy

Ranked from the 8-test AEGIS corridor experiment:

| Rank | Signal type | Avg score | Reliability |
|------|-------------|-----------|-------------|
| 1 | Security infrastructure (cameras, turnstiles, signs) | 9.4 | High |
| 2 | Architectural vocabulary + materials + identity | 9.2 | High |
| 3 | Architectural vocabulary + materials | 9.0 | High |
| 4 | Architectural vocabulary alone | 8.7 | High |
| 5 | Baseline corridor | 8.5 | — |
| 6 | Narrative / functional purpose | 8.3 | Low |
| 7 | Lighting structure (with architecture) | 8.0 | Low |
| 8 | Lighting structure (alone) | 7.8 | Low |

**Key takeaway:** The model responds strongly to things it can draw (doors, readers, cameras, panels, signs) and weakly to concepts it must infer (atmosphere, purpose, mood, containment).

---

## How to use these principles

When writing any institutional environment prompt:

1. Open with the visible consequence — what hardware, infrastructure, and security elements are physically present (P1)
2. Specify the repeating architectural structures — what repeats and at what cadence (P2)
3. Name the materials with surface qualifiers — not just "metal" but "brushed metal with anti-reflective coating" (P3)
4. Add exclusion pairs — what the space IS NOT, per element (P4)
5. Reserve spatial zones for game readability — percentages, not vague terms (P5)
6. For batch work, generate a shared anchor using landmark patterns as the primary continuity signal (P6)
