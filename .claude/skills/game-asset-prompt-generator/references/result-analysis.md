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

### CT-15: Component Collapse in Large Procedural Spaces

**Observed:** A prompt listing multiple distinct object types (fingerprint pedestals, camera arches, turnstiles, verification kiosks) produces a homogeneous row of identical terminals instead. The model collapses specific components into a generic "processing infrastructure" concept.

**Likely cause:** Large procedural spaces (halls, processing floors, open-plan facilities) trigger the model's "simplify and repeat" behavior. When given a large empty space and 5+ object types, the model fills the space with repetitions of the most generic object rather than placing each distinct type. The scale of the space overwhelms the specificity of the objects.

**Correction:** Either reduce the space scale (anchor as "checkpoint room" not "processing hall") or use spatial staging to assign specific zones: "foreground: three fingerprint scanning pedestals, middle ground: security turnstiles, background: observation windows and clearance desk." Explicit spatial layers prevent the model from defaulting to repetition.

**Most impacted variant:** Enhanced Detail, Asset Pipeline

**Status:** Observed

**Evidence:** 2 observations — Room test 8 (large hall, all objects → identical terminal rows, 8.2) and Room test 9 (spatial staging checkpoint, fingerprint pedestals → kiosks, turnstiles missing, 6.5). Consistent pattern: the model renders procedural hardware as generic computer stations regardless of space scale or staging.

---

### CT-16: Nouns Override Atmosphere — Maintenance/Utility Drift

**Observed:** A prompt that labels a space as a primary operations corridor (e.g., "AEGIS corridor", "command spine", "hero deployment route") and supports it with atmosphere words ("institutional", "operational", "surveilled") produces a maintenance tunnel, electrical utility corridor, or power infrastructure passage. The image contains conduit bundles, cable trays, junction boxes, and service panels but no authority signals, operational architecture, or evidence of personnel circulation.

**Likely cause:** The prompt's visible nouns — conduit bundles, cable trays, junction boxes, service panels, maintenance catwalks, utility pipes — are all maintenance infrastructure objects. The model builds its visual world from objects, not atmosphere words. Even when the high-level description and style block specify "AEGIS" and "institutional", the object vocabulary tells the model "this is a maintenance space." The model renders what it can see described, not what it is told the space is called.

This is a structural variant of CT-12 (Administrative Function Drift) and CT-14 (Consumer Kiosk Drift): the same mechanism — object vocabulary overriding intended room identity — but with maintenance/utility objects as the dominant signal rather than administrative or consumer objects.

**Correction:** Replace every maintenance-specific noun with an operations-equivalent object that communicates authority, oversight, or personnel circulation:

Replace these:
- conduit bundles → recessed data trunk raceways
- cable trays → integrated monitoring rail systems
- junction boxes → access control nodes
- service panels → security interface panels
- maintenance catwalks → observation galleries / inspection galleries
- utility pipes → sealed service columns / environmental control risers
- mechanical wall → hardened operations partition

Add these instead:
- recessed observation galleries at upper levels
- reinforced sector access portals at regular intervals
- integrated security stations or monitoring alcoves
- biometric or card-reader access architecture
- command-access nodes with clearance hardware
- deployment sector interface doors
- personnel traffic indicators (floor markings, wayfinding, zone numbering)

Maintain a deliberate object identity check: for every physical object in the prompt, ask "would this object be in the primary circulation spine of a national agency, or in a maintenance tunnel?" If the answer is "maintenance tunnel", replace it.

**Most impacted variant:** Enhanced Detail, Art Director — these variants add the most objects, making them most vulnerable to unexamined noun choices.

**Status:** Observed

**Evidence:** 1 observation — AEGIS corridor JSON prompt. Every structural noun described maintenance infrastructure. The image faithfully rendered a maintenance tunnel (9/10 match to that intent) despite atmosphere words specifying "AEGIS" and "institutional" (2/10 match to intended agency identity). The model followed the object vocabulary, not the context labels.

---

### CT-18: Regional Scene Lock (Ideogram)

**Observed:** Regional descriptions dominate scene composition to the point of overriding the high-level description. A "containment corridor" with command-center regions produces a command center. The model follows the regions' implied room type, not the HLD's stated room type.

**Likely cause:** In Ideogram's hierarchy, regional descriptions (Level 1) appear to have stronger influence than the high-level description (Level 3). If regions describe furniture/architecture from a different room type, the HLD is ignored.

**Correction:** Ensure every region description reinforces the same room type as the HLD. Each architectural mass noun must be consistent with the stated location. Corridor test: all regions must reference corridor-appropriate elements (doors, glass partitions, ceiling channels, floor). Do not mix room types across regions (e.g., analyst workstations in a corridor).

**Most impacted variant:** Production Safe, Asset Pipeline

**Status:** Observed

**Evidence:** 1 observation — Ideogram Test 3. HLD="containment corridor", regions="analyst workstations, tactical displays, observation gallery". Output was a convincing operations center, not a corridor.

---

### CT-19: Flat Composition / No Perspective Depth (Ideogram Regional)

**Observed:** Regional prompting produces front-facing architectural elevations rather than deep perspective scenes. Walls left/right/ceiling/floor produce a flat room with no vanishing point even when the background description specifies depth.

**Likely cause:** Regions describe four architectural planes (left wall, right wall, ceiling, floor), which the model assembles as a front-facing box. The regional structure conflicts with the perspective requirement — the model prioritizes the regional plane assignment over the camera instruction in the background description.

**Correction:** Do not use left/right wall regions for corridor scenes. Background description alone controls perspective. For corridor depth, either omit regional prompting entirely or restructure regions as foreground/mid/background layers instead of wall planes. The background description must contain explicit perspective language ("centered vanishing point, corridor receding into darkness").

**Most impacted variant:** Production Safe, Asset Pipeline

**Status:** Observed

**Evidence:** 1 observation — Ideogram Test 1. 4 regions (floor/doors/glass/ceiling) produced strong semantic compliance but zero corridor depth despite background description specifying "long corridor receding into darkness."

---

### CT-20: Vertical Signal Overpowering Circulation (Ideogram / Z-Image Turbo)

**Observed:** A prompt intended to describe a towering corridor or multi-level passage produces a vertical shaft, interior canyon, or maintenance superstructure instead. The image reads as a vertical transit spine or atrium rather than a path forward. Circulation (depth movement) is secondary to architecture (vertical mass).

**Likely cause:** The ratio of vertical-to-depth cues is unbalanced. Strong vertical signals — "multi-story", "stacked catwalks", "towering", "overwhelming height", "low angle", "floor only 15-20%" — collectively tell the model to build upward. Weak depth signals — "corridor extends forward", "continuation path", "readable route" — cannot compete. The model prioritizes the stronger cue set, producing "monumental architecture with a corridor somewhere in it" rather than "a corridor with monumental architecture."

**Correction:** Maintain a minimum 1:1 ratio of depth-to-vertical cues when the scene must read as a corridor. For every vertical signal, include an equivalent depth signal. Specific corrections:
- Increase floor percentage from 15-20% to 30-40% — more floor area strengthens the circulation cue
- Make forward movement the primary visual focus: "long uninterrupted corridor axis, strong central vanishing point, repeating floor markings leading into depth, corridor continuation is the primary visual focus"
- Limit visible upper levels: "ceiling disappears into darkness, upper structural levels only partially visible, suggested height rather than fully revealed height" — hidden height reads as unknowable scale, not atrium openness
- Or, embrace the result as Category B (Monumental Space) and rename the location accordingly

**Most impacted variant:** Enhanced Detail, Art Director

**Status:** Observed

**Evidence:** 1 observation — Ideogram Test 4. Prompt mixed strong vertical cues (multi-story, stacked catwalks, low angle, 15-20% floor) with weak depth cues. Result: monumental architecture overpowering circulation. Vertical scale 10/10, corridor readability 6/10.

---

## Evaluation Template

Use this structured comparison after every Ideogram test run. Score each category 1-10 and document what the model captured vs missed.

```
## Overall Match Score

| Category | Score |
|----------|-------|
| Mood | /10 |
| Lighting | /10 |
| Perspective | /10 |
| [Scale / Vertical] | /10 |
| [Identity / Faction match] | /10 |
| [Utility / Function] | /10 |
| [Other relevant category] | /10 |
| [Other relevant category] | /10 |

## What the Image Successfully Captured

### [Category name]
Prompt said: [excerpt from prompt]
Image: [what rendered]
Score: ✅ / ⚠️ / ❌
Analysis: [why it matched or why the model interpreted it differently]

### [Next category]
...

## What the Image Failed To Capture

### [Category name]
Prompt said: [excerpt from prompt]
Image: [what rendered instead]
Score: ✅ / ⚠️ / ❌
Root cause: [why — trace to specific prompt nouns or structure]

### [Next category]
...

## What in the JSON Caused This

[Identify the specific phrase, noun, or structural choice that produced the failure]

[Quote the problematic JSON excerpt]

## Biggest Missing Element

[Single most impactful omission vs the intended concept]

## Revision Plan

[Specific changes to make in the next iteration]
```

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

### Full lifecycle (for standalone prompt development outside the 4-variant workflow)

When developing a single prompt (e.g., an Ideogram 4 JSON caption):

1. Draft using the core prompt framework and visual-descriptors.md vocabulary
2. If JSON caption: follow docs/ideogram-4-caption-spec.md for schema structure
3. Generate test image (Ideogram or target model)
4. Run structured evaluation using the evaluation template above
5. For each failure, check the failure pattern catalog and apply the correction
6. If the failure is novel and structural (not just wording), add a new CT entry
7. Revise the prompt and document both attempts
8. Iterate until the output meets all intended categories at 8/10 or higher
