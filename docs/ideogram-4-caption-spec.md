# Ideogram 4 Caption Specification

A structured prompt specification for generating Ideogram 4 JSON captions. Designed for use with `Ideogram4PromptBuilderKJ` as the `import_json` input.

For regional prompting patterns, coordinate diagrams, layout recipes, and failure modes, see [Ideogram 4 regional prompting](../.claude/skills/game-asset-prompt-generator/references/ideogram-regional-prompting.md).

## Overview

This document defines the repository standard for transforming an image request into a production-ready Ideogram 4 JSON caption. It covers style mode selection, high-level description, style generation, color palettes, compositional deconstruction, element generation, bbox layout control, element formatting, typography, element palettes, quality improvements, and validation.

Ideogram 4 accepts plain text, but the official Ideogram prompting guide states that structured JSON captions produce better controllability, spatial layout, and style fidelity because the model was trained on that caption structure.

## Style Modes

- **PHOTO MODE** - photography, product shots, fashion, food, architecture, automotive, portraits, realistic scenes. Generates `photo` field.
- **ART_STYLE MODE** - anime, manga, concept art, digital painting, graphic design, posters, illustrations, pixel art. Generates `art_style` field.
- **NONE MODE** - no style guidance. Omit `style_description` entirely.

When `style_description` is present, use exactly one of `photo` or `art_style`.

## Output Rules

- Output valid JSON only.
- No explanations, markdown, code fences, commentary, or placeholders.
- Correct key ordering enforced.
- `bbox` uses `[y_min, x_min, y_max, x_max]` in normalized `0-1000` coordinates.
- Coordinate origin is top-left.
- JSON ready for direct `import_json` usage.

## Key Order

Top-level order:

```text
high_level_description
style_description
compositional_deconstruction
```

Non-photo `style_description` order:

```text
aesthetics
lighting
medium
art_style
color_palette
```

Photo `style_description` order:

```text
aesthetics
lighting
photo
medium
color_palette
```

Element order:

```text
obj:  type, bbox, desc, color_palette
text: type, bbox, text, desc, color_palette
```

`bbox` and `color_palette` are optional element fields, but if included they should remain in the positions above.

## Color Palette Rules

The official Ideogram schema documents uppercase hex strings in `color_palette`. This repository intentionally uses a different production rule:

**Do not use hex codes in Ideogram `color_palette` arrays.** Local testing found that Ideogram processes named color descriptions more reliably than hex values. Hex codes have weak-to-no influence on generated output in this repository's test corpus.

Use named color descriptions drawn from the memory colors vocabulary in `references/visual-descriptors.md`:

```json
"color_palette": [
  "deep navy-black shadows",
  "gunmetal-grey architecture",
  "icy steel-blue illumination",
  "cold blue-white LED cast",
  "muted amber guidance strip"
]
```

Rules:
- 5-12 entries for global `style_description.color_palette`.
- Up to 5 entries for per-element `color_palette`.
- Pair a memory color name with a material or light qualifier, such as "charcoal matte walls" or "glacier LED strips".
- Draw vocabulary from `references/visual-descriptors.md` memory colors and color modifier sections.
- If a brand hex reference is necessary, include it in prose outside `color_palette`, for example: `brand reference: deep navy approximately #080A0E`.

## Coordinate and Bbox Rules

Ideogram 4 bounding boxes use normalized image coordinates:

```text
[y_min, x_min, y_max, x_max]
```

Do not use `[x_min, y_min, x_max, y_max]`.

```text
0,0 ┌──────────────────────────────┐ 0,1000
    │                              │
    │                              │
1000,0 └───────────────────────────┘ 1000,1000
```

Example:

```json
"bbox": [100, 200, 400, 800]
```

Means:
- Top edge at `y = 100`.
- Left edge at `x = 200`.
- Bottom edge at `y = 400`.
- Right edge at `x = 800`.

Use bboxes for layout-sensitive assets:
- Ads
- Posters
- Covers
- Product shots
- Logo lockups
- Text designs
- Infographics
- VN background safe zones

Avoid bboxes when exact placement is not useful or when the composition needs natural, unconstrained scene flow.

## Regional Prompting Decision Rule

Use regional prompting for semantic zone occupancy, not pixel-perfect control.

Good regional use:
- Reserve an empty VN foreground floor.
- Separate a logo mark from a wordmark.
- Keep headline, subject, and footer zones stable across social templates.
- Keep product labels away from the product.
- Assign architectural masses to room zones.

Risky regional use:
- Long corridors where left/right wall boxes flatten perspective.
- Dense scenes with many small props.
- Conflicting room types across regions.
- Tiny boxes for long text.
- Overlapping boxes that should remain visually separate.

For corridors, perspective must come from `compositional_deconstruction.background`. Prefer no regions or foreground/midground/background regions. Avoid left-wall/right-wall regions unless the intended output is a flat architectural elevation.

## What Actually Influences Ideogram Output

Based on empirical testing, prompt signals have different influence weights:

| Tier | Signal type | Influence |
|------|-------------|-----------|
| 1 | Concrete architectural/visual nouns | Very strong - drives geometry |
| 2 | Composition and perspective terms | Strong - respected closely |
| 3 | Named color descriptions | Strong for broad palette |
| 4 | Atmosphere labels | Moderate - influences mood, not geometry |
| 5 | Hex codes in color_palette | Weak - largely ignored in this repo's tests |
| 6 | Percentages, such as "30% floor" | Weak - imprecise approximation only |

The model extracts visual anchors from the text. Concrete nouns that describe visible objects drive output more than structural metadata or abstract labels.

## Full Specification

The complete 12-step prompt workflow used in this project:

1. **Determine Style Mode** - PHOTO, ART_STYLE, or NONE.
2. **High-Level Description** - 1-2 sentence overview of subject, action, environment, mood.
3. **Style Generation** - aesthetics, lighting, photo/art_style.
4. **Color Palette** - 5-12 named color descriptions, no hex codes in `color_palette`.
5. **Compositional Deconstruction** - background description + elements array.
6. **Element Generation** - separate elements for important visual subjects; focus on concrete visible nouns.
7. **Bbox Decision Rule** - use layout control for ads, posters, covers, product shots, text designs, and safe zones.
8. **Element Format** - `obj` type for objects/subjects and `text` type for visible text.
9. **Typography** - visible text elements with font category, weight, rendering style, and literal text.
10. **Element Palettes** - per-element named color palettes, up to 5 entries, no hex codes in `color_palette`.
11. **Quality Improvements** - composition, hierarchy, storytelling, realism, color harmony.
12. **Validation** - structural and semantic checks before output.

## Validation Checklist

Before returning an Ideogram 4 JSON caption:

- JSON parses successfully.
- Top-level fields follow the expected order.
- `style_description` uses either `photo` or `art_style`, not both.
- `compositional_deconstruction.background` appears before `elements`.
- Element `bbox` arrays use `[y_min, x_min, y_max, x_max]`.
- All bbox coordinates are integers from `0` to `1000`.
- Text elements use escaped `\n` for intentional line breaks.
- `color_palette` arrays use named color descriptions, not hex strings.
- Region descriptions reinforce the same scene type as the high-level description.
- Corridor prompts do not default to left/right wall regions when depth is required.
