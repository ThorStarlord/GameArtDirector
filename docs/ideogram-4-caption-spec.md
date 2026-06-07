# Ideogram 4 Caption Specification

A structured prompt specification for generating Ideogram 4 JSON captions. Designed for use with `Ideogram4PromptBuilderKJ` as the `import_json` input.

## Overview

This document defines a 12-step workflow for transforming an image request into a production-ready Ideogram 4 JSON caption. It covers style mode selection, high-level description, style generation (aesthetics, lighting, photo/art_style), color palette, compositional deconstruction, element generation, bbox layout control, element formatting, typography, element palettes, quality improvements, and validation.

## Style Modes

- **PHOTO MODE** — photography, product shots, fashion, food, architecture, automotive, portraits, realistic scenes. Generates `photo` field.
- **ART_STYLE MODE** — anime, manga, concept art, digital painting, graphic design, posters, illustrations, pixel art. Generates `art_style` field.
- **NONE MODE** — no style guidance. Omit `style_description` entirely.

## Output Rules

- Output valid JSON only.
- No explanations, markdown, code fences, commentary, or placeholders.
- Correct key ordering enforced.
- Colors use uppercase `#RRGGBB`.
- `bbox` uses `[ymin, xmin, ymax, xmax]` range 0–1000.
- JSON ready for direct `import_json` usage.

## Full Specification

The complete 12-step prompt used in this project is maintained as the canonical reference:

1. **Determine Style Mode** — PHOTO, ART_STYLE, or NONE
2. **High-Level Description** — 1–2 sentence overview of subject, action, environment, mood
3. **Style Generation** — aesthetics, lighting, photo/art_style
4. **Color Palette** — professional global palette, 5–12 colors, uppercase `#RRGGBB`
5. **Compositional Deconstruction** — background description + elements array
6. **Element Generation** — separate elements for important visual subjects
7. **Bbox Decision Rule** — layout control for ads, posters, covers, product shots, text designs
8. **Element Format** — obj type (with optional bbox) and text type
9. **Typography** — visible text elements with font category, weight, rendering style
10. **Element Palettes** — per-element color palettes (up to 5 colors)
11. **Quality Improvements** — composition, hierarchy, storytelling, realism, color harmony
12. **Validation** — structural and semantic checks before output
