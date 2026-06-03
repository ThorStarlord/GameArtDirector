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

## Variant rules

### Production Safe
Most likely to generate a usable result quickly. Keep the direction clear, concise, and production-friendly.

### Enhanced Detail
Add richer materials, surface detail, lighting, ornament, and art direction without making the prompt noisy.
Keep the underlying room type and structural hierarchy unchanged; add detail through material specificity, lighting contrast, and controlled surface wear, not by adding more visible equipment, more room functions, or extra architectural complexity.

### Art Director Version
Focus on narrative intent, faction or world cohesion, mood, and how the asset supports the game's visual identity.
For corridor and interior backgrounds, keep the room type stable and let faction identity emerge through architecture, surface language, lighting hierarchy, and restraint; do not convert a transit space into a denser operations room just to make the world feel more specific.

### Asset Pipeline Version
Optimize for extraction, integration, and consistency in a game production pipeline.
Prioritize clean layer-friendly readability without over-sanitizing the scene: preserve identity anchors (clearance hardware rhythm, door cadence, wall segmentation, controlled glow zones) so the result does not collapse into a generic sterile hallway.
When specifying lighting for pipeline-safe backgrounds, avoid fully uniform ceiling-grid illumination unless requested; retain intentional contrast bands or light-pool rhythm so depth and mood survive integration.

## Prompting rules

- Prefer clear descriptive language over hype terms.
- Emphasize silhouette, readability, composition, and visual intent.
- Keep the prompt grounded in 2D game production needs.
- Mention transparent backgrounds, isolation, padding, and modularity when relevant.
- For institutional interiors, state both what must be present and what must stay absent: sealed access points, clearance hardware, and disciplined architectural severity should be explicit, while generic lab clutter, desk surfaces, loose monitors, and busy workstation detail should be excluded unless requested.
- In elevated-detail institutional backgrounds, treat monitors, desks, chairs, and instrument clusters as secondary or implied unless the scene is explicitly a control room or lab; otherwise the model tends to turn security corridors into generic research spaces.
- When a corridor or secure room includes glass partitions, clarify whether the contents beyond the glass should read as abstract technical glow only or as specific room detail; otherwise the model may drift toward ordinary office or research-facility interiors.
- If the intended mood is oppressive or surveilled, specify shadow separation and contrast between lit pools and dead zones instead of relying on evenly distributed ceiling light.
- For pipeline-oriented corridor prompts, include at least one repeatable but non-intrusive landmark pattern (for example: periodic access reader cadence, safety-strip continuity, or panel-break rhythm) to improve continuity across batches while keeping the lower gameplay area clear.
- If the request implies a franchise, project, or shared art style, keep the output style-consistent rather than inventing a disconnected look.
- Avoid text, watermarks, logos, UI labels, borders, or unrelated secondary subjects unless requested.

## Output requirements

- Start with a short context line only when inferred assumptions materially improve the prompt.
- Return these sections in order: Production Safe, Enhanced Detail, Art Director Version, Asset Pipeline Version.
- Write each prompt as natural-language art direction, not a comma-separated keyword list.

When the request closely matches an existing corpus entry, load that entry first and adapt it rather than regenerating structure from scratch.

See [asset types](references/asset-types.md), [art styles](references/art-styles.md), [readability rules](references/readability-rules.md), [icon guidelines](references/icon-guidelines.md), [sprite guidelines](references/sprite-guidelines.md), [prompt patterns](references/prompt-patterns.md), [prompt corpus](references/prompt-corpus.md), and [test set](references/test-set.md).