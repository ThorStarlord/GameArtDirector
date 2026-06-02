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

### Art Director Version
Focus on narrative intent, faction or world cohesion, mood, and how the asset supports the game's visual identity.

### Asset Pipeline Version
Optimize for extraction, integration, and consistency in a game production pipeline.

## Prompting rules

- Prefer clear descriptive language over hype terms.
- Emphasize silhouette, readability, composition, and visual intent.
- Keep the prompt grounded in 2D game production needs.
- Mention transparent backgrounds, isolation, padding, and modularity when relevant.
- If the request implies a franchise, project, or shared art style, keep the output style-consistent rather than inventing a disconnected look.
- Avoid text, watermarks, logos, UI labels, borders, or unrelated secondary subjects unless requested.

## Output requirements

- Start with a short context line only when inferred assumptions materially improve the prompt.
- Return these sections in order: Production Safe, Enhanced Detail, Art Director Version, Asset Pipeline Version.
- Write each prompt as natural-language art direction, not a comma-separated keyword list.

When the request closely matches an existing example, load that example first and adapt it rather than regenerating structure from scratch.

See [asset types](references/asset-types.md), [art styles](references/art-styles.md), [readability rules](references/readability-rules.md), [icon guidelines](references/icon-guidelines.md), [sprite guidelines](references/sprite-guidelines.md), [prompt patterns](references/prompt-patterns.md), and [examples](references/examples.md).