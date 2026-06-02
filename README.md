# GameArtDirector

GameArtDirector is a model-agnostic AI art-direction repository for 2D game development. Its first skill, `game-asset-prompt-generator`, turns rough asset ideas into production-ready prompt variants for modern image models.

## Current scope

- Expand rough asset requests into 4 prompt variants.
- Apply game-art readability and production constraints automatically.
- Support icons, sprites, enemies, props, buildings, environments, UI elements, and VFX.
- Stay focused on prompt quality, not workflow execution or model orchestration.

## Repository direction

This repository is intended to grow into a broader game art direction knowledge base. The current skill is intentionally narrow and reusable.

Planned adjacent skills:
- `game-art-bible`
- `game-character-designer`
- `game-ui-designer`
- `game-vfx-designer`

## Structure

```text
.claude/skills/
  game-asset-prompt-generator/
    SKILL.md
    references/
      asset-types.md
      art-styles.md
      icon-guidelines.md
      prompt-patterns.md
      readability-rules.md
      sprite-guidelines.md
```

## Positioning

This is not a ComfyUI workflow repository. It sits one layer earlier in the pipeline:

Creative idea -> game asset analysis -> art direction -> prompt variants

That keeps the skill durable across model changes and useful even when the image-generation stack changes.