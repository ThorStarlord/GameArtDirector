---
name: game-asset-prompt-generator
description: Generate production-ready prompt variants for 2D game assets from rough art requests. Use when users ask for icons, sprites, enemies, props, buildings, environments, UI panels, VFX, or other game art prompts for modern image models.
---

# Game Asset Prompt Generator

## Quick start

Turn a rough 2D game asset idea into 4 prompt variants for modern image models:
- Production Safe
- Enhanced Detail
- Art Director Version
- Asset Pipeline Version

Default to natural-language prompt writing. Do not use legacy quality tags such as `masterpiece`, `best quality`, `8k`, `award winning`, or `trending on artstation`.

## Workflow

1. Identify the asset type, genre, and intended in-game function.
2. Infer missing production context only when it improves output reliability.
3. Apply readability and asset-production rules that fit the request.
4. Expand the request using the core prompt framework.
5. Return exactly 4 prompt variants with distinct emphasis.
6. For batch requests (multiple related assets), generate a shared batch identity anchor first, then adapt per-asset prompts from it.
7. After image generation, evaluate results against the failure pattern catalog in references/result-analysis.md and feed corrections back into prompt revision.

## Core prompt framework

Every prompt should cover:
- Subject
- Function in game
- Visual style
- Shape language
- Materials
- Color palette
- Lighting
- Camera or view
- Composition
- Background rules
- Game readability
- Technical requirements
- Spatial zones: which frame regions must stay clear (e.g., lower 35% for VN dialogue overlay, top margin for UI)
- Exclusion pairs: for each key element, specify what it IS and what it IS NOT (e.g., "glass reveals abstract glow only, not a busy lab interior")

Organize dense prompts into labeled sections (e.g., SCENE, LIGHTING, PALETTE, MOOD, COMPOSITION) to keep detail additive without becoming a keyword wall.

## Variant rules

### Production Safe
Most likely to generate a usable result quickly. Keep the direction clear, concise, and production-friendly.
Keep environmental function tightly scoped to the requested room type; do not add extra operational complexity just to increase perceived detail.
If the brief requests unresolved depth, avoid a strongly readable terminal focal endpoint (for example, a crisp hero door or hard end wall) unless explicitly requested.

### Enhanced Detail
Add richer materials, surface detail, lighting, ornament, and art direction without making the prompt noisy.
Keep the underlying room type and structural hierarchy unchanged; add detail through material specificity, lighting contrast, and controlled surface wear, not by adding more visible equipment, more room functions, or extra architectural complexity.
When adding density, organize the prompt into clear labeled sections (SCENE, LIGHTING, PALETTE, MOOD, COMPOSITION) so the detail accumulates in structured blocks rather than a wall of prose.
Use paired positive + negative specifications per element to prevent model drift into unintended room types (e.g., "teal equipment glow through glass, not a visible workstation interior").

### Art Director Version
Focus on narrative intent, faction or world cohesion, mood, and how the asset supports the game's visual identity.
For corridor and interior backgrounds, keep the room type stable and let faction identity emerge through architecture, surface language, lighting hierarchy, and restraint; do not convert a transit space into a denser operations room just to make the world feel more specific.
Open with a narrative thesis — one metaphor sentence that collapses the space's worldbuilding into a single frame (e.g., "architecture of governance: a place where powered heroes are processed by policy, not celebrated"). This constrains every subsequent decision more effectively than any material list.
State the room's narrative function and its negative identity: what the space IS and what narrative purpose it DOES NOT serve (e.g., "a transit corridor, not an operations room"). This prevents the model from drifting into a different room type at the functional level.
Couple composition choices to narrative meaning: explain WHY a compositional decision serves the story (e.g., "far end unresolved in controlled darkness to suggest classified depth"), not just what the composition looks like.
Distill each faction's material language into a 2-3 word ethos (e.g., "Stoic Slick", "Brutalist Utility", "Worn Field Kit") that constrains all surface choices — this prevents the model from defaulting to generic or prestige finishes.
Extend beyond materials into a faction's architectural vocabulary: the repeated element types that define its spaces (e.g., "clearance door cadence, access readers, wall segmentation, anti-reflective glass, and precise beam-based lighting"). This gives the model a pattern language, not just a finish palette.
Frame lighting as institutional behavior rather than mood: describe what the lighting DOES (e.g., "measured and surveilling", "it watches, it controls") not just how it looks. This ties atmosphere directly to power dynamics.
Write the Art Director prompt as coherent narrative prose without labeled sections; the narrative arc IS the organization. Target ~150 words of flowing art direction, not a structured breakdown.

### Asset Pipeline Version
Optimize for extraction, integration, and consistency in a game production pipeline.
Open by naming the output artifact type explicitly (e.g., "production-ready 2D VN background plate", "modular UI panel", "isolated sprite sheet") so the model treats the output as a specific production deliverable, not a standalone illustration.
Before adding pipeline constraints, state a stable base description (the "blueprint") that defines the unchanging structural ground truth — perspective, composition, dimensions, and spatial zones. Pipeline instructions build on top of this, not instead of it.
Frame identity anchors as cross-shot continuity mechanisms, not just decoration: periodic door cadence, access-reader placements, wall segmentation rhythm, controlled glow zones. Label their purpose (e.g., "preserve repeatable identity anchors for cross-shot continuity").
Specify material separation for clean extraction: materials should occupy distinct value/color zones so they survive compositing, keying, or tinting without bleeding into each other.
Include compositing survival language: frame lighting and contrast choices around what survives post-generation processing (e.g., "retain light-pool rhythm and contrast bands so depth and mood survive compositing").
End with a short explicit exclusion punchlist: comma-separated absolutes the output must not contain (e.g., "exclude people, text, UI, logos, and watermarks").

## Prompting rules

- Prefer clear descriptive language over hype terms.
- Emphasize silhouette, readability, composition, and visual intent.
- Keep the prompt grounded in 2D game production needs.
- Mention transparent backgrounds, isolation, padding, and modularity when relevant.
- For institutional interiors, state both what must be present and what must stay absent: sealed access points, clearance hardware, and disciplined architectural severity should be explicit, while generic lab clutter, desk surfaces, loose monitors, and busy workstation detail should be excluded unless requested.
- In Production Safe corridor prompts, default behind-glass spaces to abstract glow and broad silhouettes; avoid legible interface-like panel content unless the user explicitly asks for visible technical detail.
- In elevated-detail institutional backgrounds, treat monitors, desks, chairs, and instrument clusters as secondary or implied unless the scene is explicitly a control room or lab; otherwise the model tends to turn security corridors into generic research spaces.
- When a corridor or secure room includes glass partitions, clarify whether the contents beyond the glass should read as abstract technical glow only or as specific room detail; otherwise the model may drift toward ordinary office or research-facility interiors.
- If the intended mood is oppressive or surveilled, specify shadow separation and contrast between lit pools and dead zones instead of relying on evenly distributed ceiling light.
- In safe-mode interior prompts, keep lighting readable but directional; avoid fully uniform illumination unless the request explicitly calls for flat clinical brightness.
- For pipeline-oriented corridor prompts, include at least one repeatable but non-intrusive landmark pattern (for example: periodic access reader cadence, safety-strip continuity, or panel-break rhythm) to improve continuity across batches while keeping the lower gameplay area clear.
- If the request implies a franchise, project, or shared art style, keep the output style-consistent rather than inventing a disconnected look.
- Avoid text, watermarks, logos, UI labels, borders, or unrelated secondary subjects unless requested.
- In Asset Pipeline prompts, open by naming the production artifact type (e.g., "background plate", "modular panel", "isolated sprite"). State the stable base blueprint before pipeline constraints. Label identity anchors as cross-shot continuity mechanisms. End with a short exclusion punchlist.
- For VN backgrounds and environment shots where UI elements will overlay the frame, specify percentage-based spatial constraints (e.g., "bottom 35% as uncluttered floor space") to reserve safe zones for dialogue text, menus, or character sprites.
- When a faction or location has a consistent surface language across multiple assets, condense it into a 2-3 word material ethos (e.g., "Stoic Slick: matte charcoal, anti-reflective coatings, precise engineered lines") and use it as a recurring constraint anchor across the batch.
- For multi-asset batch requests, generate a shared batch identity anchor covering palette, material ethos, lighting logic, and exclusion rules before writing per-asset prompts. Prepend the anchor to every prompt in the batch to enforce visual continuity.
- Use color hex references (`#0d1117`) only for specific glow/lighting cast values where precision matters. For broad surface and material descriptions, use descriptive color names paired with a material qualifier (e.g., "deep navy-black structure", "cold slate-blue wall panels") — models handle named ranges more reliably than hex codes for diffuse surfaces.
- In Art Director prompts, open with a narrative thesis that collapses the space's worldbuilding into one frame. State the room's narrative function AND what narrative purpose it does NOT serve. Couple each compositional choice to its narrative reason, not just its visual form.
- When defining a faction's identity across multiple environment assets, provide two layers: a 2-3 word material ethos for surface constraint, and a vocabulary of repeated architectural elements (door cadence, panel rhythm, access hardware pattern) for spatial identity.
- For dense prompts exceeding ~150 words, organize into labeled sections (SCENE, LIGHTING, PALETTE, MOOD, COMPOSITION, TECHNICAL) to keep the prompt additive and navigable rather than a flat prose wall.

## Output requirements

- Start with a short context line only when inferred assumptions materially improve the prompt.
- Return these sections in order: Production Safe, Enhanced Detail, Art Director Version, Asset Pipeline Version.
- Write each prompt as natural-language art direction, not a comma-separated keyword list.
- For dense prompts (Enhanced Detail especially), use labeled section headers (SCENE, LIGHTING, PALETTE, etc.) to organize content. Art Director Version should avoid section headers and instead flow as coherent narrative prose — the narrative arc IS the organization. Production Safe and Asset Pipeline Version may omit section headers if they remain under ~120 words.
- For batch requests, output the shared batch identity anchor first, then the per-asset variants.

When the request closely matches an existing corpus entry, load that entry first and adapt it rather than regenerating structure from scratch.

See [asset types](references/asset-types.md), [art styles](references/art-styles.md), [readability rules](references/readability-rules.md), [icon guidelines](references/icon-guidelines.md), [sprite guidelines](references/sprite-guidelines.md), [prompt patterns](references/prompt-patterns.md), [prompt corpus](references/prompt-corpus.md), [test set](references/test-set.md), [result analysis](references/result-analysis.md), [success patterns](references/success-patterns.md), [experiment log](references/experiment-log.md), [principles](references/principles.md), and [Z-Image Turbo notes](references/z-image-turbo-notes.md).