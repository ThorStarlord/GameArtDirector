# Result Analysis

Diagnose image output failures and feed corrections back into prompt revision.

## How to use

1. Generate 4 prompt variants for a request
2. Produce images from each
3. Identify failure patterns below
4. Apply the correction to the prompt and regenerate
5. Record the result in prompt-corpus.md with the failure pattern noted

## Failure Pattern Catalog

Each entry links an observable image failure to its likely prompt cause, correction, and current evidence level.

**Status levels:**
- **Proven** — confirmed across 5+ attempts with consistent fix rate
- **Observed** — reasoned from specific batch results but not yet systematically tested
- **Hypothesized** — logical deduction, no direct evidence yet

---

### CT-01: Generic Office Drift

**Observed:** A security corridor or institutional interior looks like a standard office building — white walls, dropped ceiling tiles, generic doors, beige floor.

**Likely cause:** Prompt described the room function (corridor, secure facility) but didn't provide enough architectural vocabulary to differentiate it from the model's default "indoor hallway" training data. Generic lighting descriptions ("cool blue-white architectural lighting") exacerbate this drift.

**Correction:** Add clearance hardware cadence (sealed doors, access readers, panel rhythms, ID-card slots). Add negative identity: "not an office, not a research lab." Specify wall materials as institutional-grade (slate-blue panels, sealed concrete, brushed metal trim) rather than generic painted drywall. Avoid standalone generic lighting language — tie light to function or omit it.

**Most impacted variant:** Production Safe, Enhanced Detail

**Status:** Observed

**Evidence:** 3 observations — Test 1 (baseline), Test 3 (lighting only), Test 4 (architecture + lighting)

---

### CT-02: Uniform Brightness / Flat Lighting

**Observed:** Environment has even illumination across the entire frame. No shadow separation, no light pools, no depth through contrast. Feels clinical in a flat way rather than a purposeful way.

**Likely cause:** Prompt didn't specify lighting structure. Model defaulted to uniform overhead ceiling light.

**Correction:** Add shadow separation instruction: "recessed LED channels at 4m intervals creating geometric light pools with visible shadow bands between them." Specify that lighting should not be uniform. If surveilled mood is desired, add "measured and surveilling" as lighting behavior.

**Most impacted variant:** All

**Status:** Observed

**Evidence:** 0 observations

---

### CT-03: Laboratory / Research Facility Drift

**Observed:** What should be a security corridor, armory, or transit space has visible lab benches, monitors, beakers, equipment carts, or desk surfaces.

**Likely cause:** Prompt mentioned "technical" or "equipment" without specifying that the equipment should be behind glass and abstracted. Model associated "secure facility" with "lab."

**Correction:** Add paired positive + negative: "through glass: abstract blue-teal equipment glow only, not visible workstation interiors. No desks, chairs, monitors, or lab benches in view." Explicitly state room function negative identity: "transit corridor, not a lab or control room."

**Most impacted variant:** Production Safe, Enhanced Detail

**Status:** Observed

**Evidence:** 0 observations

---

### CT-04: Prestige Architecture / Too Clean

**Observed:** The facility looks like a tech headquarters or luxury lobby — marble, polished chrome, warm lighting, architectural awards aesthetic. Loses the "bureaucratic" or "institutional" register.

**Likely cause:** Material language was too generic or absent. Model defaulted to "high-end corporate" for any "secure facility" concept.

**Correction:** Add material ethos constraint (e.g., "Stoic Slick: matte charcoal, anti-reflective coatings, precise engineered lines — no warmth, no prestige finishes"). Specify what materials ARE allowed (sealed concrete, acoustic tile, brushed metal, safety glass) and what are NOT (marble, polished chrome, warm wood, decorative lighting).

**Most impacted variant:** Art Director, Enhanced Detail

**Status:** Observed

**Evidence:** 0 observations

---

### CT-05: Crowded Composition / No UI Safe Zone

**Observed:** The lower third of the image is filled with objects — desks, equipment, floor clutter — making it unusable as a VN background where dialogue text or character sprites need the space.

**Likely cause:** Prompt didn't reserve spatial zones. Model populated the scene normally.

**Correction:** Add percentage-based spatial constraint: "bottom 35%: open uncluttered floor, no foot-level objects, no equipment, no furniture." Specify foreground as clear.

**Most impacted variant:** Production Safe, Asset Pipeline

**Status:** Observed

**Evidence:** 0 observations

---

### CT-06: Generic Sterile Hallway

**Observed:** The corridor is a plain empty box — no identity anchors, no wall segmentation, no access hardware, no material variation. Could be any building.

**Likely cause:** Prompt was too minimal. No architectural vocabulary was provided beyond "corridor."

**Correction:** Add identity anchor list: "periodic sealed door cadence, access-reader placements, wall segmentation rhythm, low amber safety strip, controlled glass glow zones." Specify that these repeat across the length of the corridor for visual interest and faction identity.

**Most impacted variant:** Production Safe, Asset Pipeline

**Status:** Observed

**Evidence:** 0 observations

---

### CT-07: Warm Color Cast

**Observed:** An institutional environment that should feel cold reads as warm-toned — amber lighting, beige walls, warm grey floors.

**Likely cause:** Prompt didn't specify color temperature for lighting cast or wall palette. Model defaulted to warm indoor lighting.

**Correction:** Add explicit cool palette: "lighting cast: near-white architectural LEDs with a cool blue cast (#dce8f0). Wall palette: cold slate-blue, deep navy-black structure, military green-grey secondary. Floor: cold polished grey-blue. No warm tones, no amber lighting except emergency safety strip at floor level."

**Most impacted variant:** Enhanced Detail, Art Director

**Status:** Hypothesized

**Evidence:** 0 observations

---

### CT-08: Glass Becomes Transparent Lab / Office

**Observed:** Glass partitions intended to show abstract glow instead reveal a fully rendered office, lab, or workspace beyond, cluttering the scene and changing the room's perceived function.

**Likely cause:** Prompt mentioned "glass" but didn't specify what content should appear beyond it. Model defaulted to "window into a room" and populated it.

**Correction:** Add specific behind-glass instruction: "right-side glass reveals only abstract blue-teal technical glow and broad silhouettes, not legible equipment or workspace detail." If the glow should be dominant, add "the glass surface itself has a reflective quality that obscures detail."

**Most impacted variant:** Production Safe, Enhanced Detail

**Status:** Observed

**Evidence:** 0 observations

---

### CT-09: Broken Batch Continuity

**Observed:** Two images meant to be the same location look unrelated — different wall colors, different lighting, different materials, different hardware.

**Likely cause:** No shared identity anchor was applied across the batch. Each prompt drifted independently.

**Correction:** Generate a batch identity anchor covering four layers: shared palette, shared lighting logic, shared architectural vocabulary, and shared landmark pattern. The landmark pattern (e.g., "periodic access-reader cadence, sealed door rhythm, amber safety-strip continuity, glass partition placement") is the strongest continuity signal — structures repeat more reliably than colors across model runs. Prepend the full anchor to every prompt in the batch verbatim.

**Most impacted variant:** All

**Status:** Observed

**Evidence:** 0 observations

---

### CT-10: Text / UI / Watermark Bleed

**Observed:** Model generates signage, labels, fake UI elements, or watermarks in the environment.

**Likely cause:** Training data included images with text. Model sometimes reproduces it in "institutional" contexts (signage, caution labels).

**Correction:** Add explicit exclusion at end of every prompt: "no text, no UI, no signage, no logos, no watermarks." Position it as the last sentence.

**Most impacted variant:** All

**Status:** Observed

**Evidence:** 0 observations

---

### CT-11: Corporate Office Drift from Generic Lighting

**Observed:** An institutional environment that uses security architectural vocabulary still reads as a modern office building. The identity gained from hardware repetition is undermined by the lighting register.

**Likely cause:** Generic architectural lighting descriptions — "cool blue-white architectural lighting", "bright even illumination" — trigger the model's "corporate office" training data rather than its "secure facility" training data. The model maps cool clean light to commercial interiors, not institutional ones.

**Refinement — generic vs. behavioral:** The failure is not "lighting descriptions" broadly but specifically *generic* lighting descriptions. Test 9 showed that behavioral lighting ("geometric pools of light at 4m intervals, deep shadow zones between each pool, cool blue cast, ambient teal glow through glass, amber safety strip") produces strong atmosphere and depth. The correction is to replace generic adjectives with measurable visible behavior.

**Correction:** Replace generic lighting adjectives with visible light behavior. Instead of "cool blue-white architectural lighting", use "recessed ceiling LED channels creating geometric light pools with visible shadow bands between. Cool blue cast. Ambient teal glow from equipment behind glass. Dim amber safety-strip at floor level." Describe what the light *does* geometrically, not what color it *feels* like.

**Most impacted variant:** Enhanced Detail, Art Director

**Status:** Observed

**Evidence:** 2 observations — Test 4 (generic lighting, 8.0) vs Test 9 (behavioral lighting, 9.5). The 1.5 point gap isolates the difference between generic and behavioral lighting language.

---

### CT-12: Administrative Function Drift

**Observed:** A prompt describing a facility's administrative purpose (intake processing, registration, documentation) produces a generic or unrelated space (locker room, storage, office). The model fails to render the intended function.

**Likely cause:** The prompt described bureaucratic functions ("biometric registration stations", "evidence intake counters", "containment documentation kiosks") that lack strong visual identities. The model has no clear training reference for "evidence intake counter" — it could look like a thousand different things. The model falls back to its strongest visual prior for any room with "containment" + "booths": storage compartments.

**Correction:** Translate every administrative function into concrete, drawable objects. Replace:
- "biometric registration station" → "fingerprint scanner pedestal, camera-based facial recognition terminal, ID card enrollment desk"
- "evidence intake counter" → "sealed evidence lockers, numbered evidence drawers, secure transfer windows"
- "containment documentation kiosk" → "wall-mounted clearance terminal, security report touchscreen, authorization workstation"

**Most impacted variant:** Enhanced Detail, Art Director

**Status:** Observed

**Evidence:** 2 observations — Room test 4 (bureaucratic labels → locker room, 7.0) and Room test 6 (scale fixed but admin functions still → computer terminals/desks, 8.5). Even with correct scale, Level 3 administrative concepts default to generic office furniture.

---

### CT-13: Scale Collapse

**Observed:** The model renders all specified objects correctly but collapses the scene scale — a facility prompt produces a single room, a single module, or a small compartment. Objects are present but the environment feels like a closet rather than a hall.

**Likely cause:** The prompt listed objects that individually imply small enclosed spaces: "holding cell doors", "secure evidence lockers", "security transfer gates". The model assembled them at their implied scale rather than scaling up to "facility." Objects carry scale priors — lockers and holding cells live in small rooms.

**Correction:** Establish room scale as the first constraint before listing any objects. Open with a scene anchor: "large institutional processing hall, wide-open floor, multiple intake stations arranged in rows, central security circulation zone." Then list the infrastructure. The model needs to know "hall first, details second" — otherwise objects with small-scale priors dominate the composition.

**Most impacted variant:** Enhanced Detail, Asset Pipeline

**Status:** Observed

**Evidence:** 1 observation — Room test 5 rendered all objects correctly but collapsed to containment module scale. Contrasts with checkpoint lobby test where "checkpoint lobby" itself anchors large room scale.

---

### CT-14: Consumer Kiosk Drift

**Observed:** Screen-based terminals in a security context render as arcade machines, ticket kiosks, airport check-in terminals, or shopping mall information points. The environment shifts from "government security processing" to "consumer public service."

**Likely cause:** Objects with strong consumer-context training data (screen + pedestal + kiosk form factor) override the intended security context. The model has far more photographs of airport check-in kiosks, arcade machines, and subway ticket terminals than it does of biometric enrollment stations or security clearance kiosks. The consumer prior dominates.

**Correction:** Surround screen-based terminals with unambiguous security architecture that constrains the context. Instead of isolated kiosks, embed them within: "security turnstiles between processing zones, reinforced ballistic observation windows, overhead surveillance cameras, armed checkpoint architecture, access-controlled security doors branching from the hall." The security infrastructure must be integrated into the same frame as the kiosks — not mentioned elsewhere in the prompt.

**Most impacted variant:** Enhanced Detail, Asset Pipeline

**Status:** Observed

**Evidence:** 1 observation — Room test 7 (8.0). Scale and organization correct, but fingerprint scanners and clearance kiosks rendered as consumer ticket/check-in terminals.

---

## Analysis Entry Template

Use this when you encounter a failure pattern not in the catalog above.

```
### CT-XX: [Short Failure Name]

**Observed:** [What the image looks like that's wrong]

**Likely cause:** [What in the prompt likely caused it]

**Correction:** [Specific prompt change to fix it]

**Most impacted variant:** [Which of the 4 variants is most prone]

**Status:** [Proven / Observed / Hypothesized]

**Evidence:** [observation count and fix rate if available]

**Image reference:** [batch/corpus entry link if available]
```

---

## Iteration Workflow

When a result fails:

1. Identify the failure pattern from the catalog above
2. Apply the correction to the specific variant(s)
3. Regenerate and compare
4. If the pattern is new and confirmed across 3+ attempts, add it to the catalog
5. Record both attempts in prompt-corpus.md with the CT code in the Notes field
6. Update the Evidence count on the pattern entry
