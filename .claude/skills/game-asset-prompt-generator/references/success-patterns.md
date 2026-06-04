# Success Patterns

What to preserve. Mirrors the failure catalog but captures prompt strategies that reliably produce strong results.

**Status levels:**
- **Proven** — confirmed across 5+ generations with consistent quality
- **Observed** — seen in specific batches but not yet systematically validated
- **Hypothesized** — logically sound, not yet confirmed

---

### SP-01: Strong Corridor Depth

**Observed:** Images consistently produce a sense of deep, believable corridor space rather than a shallow or boxy room.

**Likely cause:** Three factors working together: centered one-point perspective with a vanishing point not resolved at the frame edge, alternating light-pool/shadow rhythm, and a narrow horizontal field of view that emphasizes recession.

**Preserve:** Always pair unresolved depth with a lighting structure that reinforces it. "Far end unresolved in controlled darkness" + "light pools at regular intervals with shadow bands between" is a reliable combination.

**Most impactful variant:** Enhanced Detail, Asset Pipeline

**Status:** Observed

**Evidence:** 1 observation — Test 1 (baseline with light-pool rhythm) produced strong depth; Test 2 (architectural vocab without explicit camera position) lost depth, suggesting depth requires explicit centered-perspective + light-rhythm instructions

---

### SP-02: Institutional Identity Through Repetition

**Observed:** Rooms feel recognizably AEGIS (or similar faction) rather than generic secure facilities.

**Likely cause:** The prompt specified a vocabulary of repeated architectural elements — access-reader cadence, sealed door rhythm, wall segmentation pattern, safety-strip continuity — rather than relying on palette alone.

**Preserve:** Faction identity comes more from structural repetition than from color. Palette provides the first read, but repeated hardware elements provide the lasting identity signal. Always specify at least 3 repeating landmark types.

**Most impactful variant:** Art Director, Asset Pipeline

**Status:** Observed

**Evidence:** 3 observations — Tests 2, 4, 5 all showed stronger identity from architectural repetition. Test 5 (architecture + materials) scored highest at 9.0, suggesting materials amplify the repetition effect.

---

### SP-03: Clean UI Safe Zone

**Observed:** The bottom third of the image is reliably open floor with no foot-level objects, making it usable as a VN background without compositing issues.

**Likely cause:** Prompt explicitly reserved the lower frame with a percentage-based spatial constraint ("bottom 35% as open uncluttered floor") and excluded furniture, equipment, and floor clutter.

**Preserve:** Always include percentage-based spatial constraints in Production Safe and Asset Pipeline variants for any image that will receive UI overlays. Be specific about what occupies the zone ("open polished floor with controlled reflections") so the model fills it appropriately rather than leaving it empty in a visually unresolved way.

**Most impactful variant:** Production Safe, Asset Pipeline

**Status:** Observed

**Evidence:** 0 observations

---

### SP-04: Material Separation for Compositing

**Observed:** Different surfaces (wall panels, floor, ceiling, glass, hardware) occupy distinct value and color zones, making the image easy to tint, key, or adjust per-element in post-production.

**Likely cause:** Prompt specified each major material with a distinct color + light response pair rather than a single blanket description. "Deep navy-black structure, cold slate-blue wall panels, polished grey-blue floor, teal glow accents" forces value separation between planes.

**Preserve:** When compositing ease matters, specify each architectural plane with its own color-and-light pair. Avoid blanket terms like "institutional colors" — the model will blend everything into one value range.

**Most impactful variant:** Asset Pipeline

**Status:** Observed

**Evidence:** 0 observations

---

### SP-05: Glass Withheld / Abstract Presence

**Observed:** Glass partitions show controlled abstract glow rather than fully rendered room interiors, preserving the scene's intended mood and preventing narrative clutter.

**Likely cause:** Prompt specified both what the glass reveals AND what it conceals: "abstract blue-teal technical glow only, not legible equipment or workspace detail."

**Preserve:** For any glass partition in an institutional environment, always include the paired positive + negative. "Reveals X, not Y" is the reliable formulation. Adding "the glass surface itself has a reflective quality that obscures detail" reinforces the effect.

**Most impactful variant:** Production Safe, Enhanced Detail

**Status:** Observed

**Evidence:** 0 observations

---

### SP-06: Lighting as Atmosphere Driver

**Observed:** Lighting isn't just illumination — it establishes the emotional register of the space without needing explicit mood language.

**Likely cause:** Prompt described lighting in structural terms (position, beam shape, color temperature, falloff) rather than atmospheric terms (dim, bright, gloomy). The structural description constrained the model, and the atmosphere emerged from the constraints.

**Preserve:** Describe lighting through measurable qualities (source position, beam geometry, cast color, interval spacing, intensity variation) and let the mood follow. Structural lighting instructions are more reliable than mood adjectives.

**Most impactful variant:** Enhanced Detail

**Status:** Observed

**Evidence:** 1 observation — Test 9 (behavioral lighting: geometric pools, shadow zones, specific cast colors) produced strong atmosphere; earlier Tests 3/4 (generic lighting: "cool blue-white") produced office drift. Behavioral lighting is a strong signal; generic lighting is not.

---

### SP-07: Visible Consequences Over Abstract Intent

**Observed:** The model renders security infrastructure (cameras, turnstiles, warning signs, observation windows) far more reliably than it renders abstract concepts (containment, oversight, classification).

**Likely cause:** The model has been trained on photographs of environments with visible security hardware. Abstract organizational concepts lack a direct visual training signal.

**Preserve:** Instead of communicating purpose through narrative labels ("containment facility", "oversight division"), translate purpose into visible architectural consequences: checkpoints, cameras, turnstiles, observation windows, warning signage, clearance displays. Ask: "What would this function look like in a photograph?"

**Most impactful variant:** Enhanced Detail, Asset Pipeline

**Status:** Observed

**Evidence:** 1 observation — Test 8 (security infrastructure) scored 9.4, significantly outperforming Test 7 (narrative function) at 8.3

---

## Entry Template

Use this when you identify a new success pattern.

```
### SP-XX: [Short Pattern Name]

**Observed:** [What the image looks like that's working well]

**Likely cause:** [What in the prompt produced it]

**Preserve:** [Specific guidance for keeping this result consistent]

**Most impactful variant:** [Which of the 4 variants benefits most]

**Status:** [Proven / Observed / Hypothesized]

**Evidence:** [observation count and consistency rate if available]
```

---

## Iteration Workflow

When a result succeeds:

1. Identify the pattern from the catalog above
2. Confirm the cause is in the prompt, not randomness
3. If it reproduces across 3+ attempts, note the pattern
4. Record the generation in prompt-corpus.md with the SP code in the Notes field
5. Update the Evidence count on the pattern entry
