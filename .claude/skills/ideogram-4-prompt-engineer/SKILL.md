---
name: ideogram-4-prompt-engineer
description: Write valid Ideogram 4 JSON captions with optional regional (bbox) prompting. Use when generating images via Ideogram 4's import_json, or when user mentions ideogram JSON, IDE-4, import_json, regional prompting, or image generation captions.
---

# Ideogram 4 Prompt Engineer

## Quick start

Convert a rough image idea into a valid Ideogram 4 JSON caption. Output JSON only — no markdown fences, no explanations.

```json
{
  "high_level_description": "Concise scene description",
  "style_description": {
    "aesthetics": "Japanese visual novel background art",
    "lighting": "Cold institutional lighting, precise architectural LEDs",
    "medium": "digital illustration",
    "art_style": "semi-realistic painterly",
    "color_palette": ["#0B1020", "#1A2E3C", "#DCE8F0"]
  },
  "compositional_deconstruction": {
    "background": "Global scene, camera, perspective, environment, exclusions as positive constraints",
    "elements": []
  }
}
```

## Project context

Before drafting, read the project's art direction documentation for domain-specific vocabulary, palette, style anchors, and prohibited phrasing. The skill handles JSON structure and prompt engineering — domain content lives in the project docs.

## Schema rules

### Required structure
- `high_level_description` — concise scene summary
- `style_description.aesthetics`, `.lighting`, `.medium`, `.art_style`, `.color_palette`
- `compositional_deconstruction.background` — global environment, camera, perspective
- `compositional_deconstruction.elements` — array of regional bbox objects

### Prohibited top-level fields
Do not use: `composition`, `environment`, `visual_focus`, `negative_emphasis`, `negative_prompt`, `global_directive`, `background` (top-level), `left_wall`, `right_wall`, `floor`, `architecture_rules`, or any custom key outside the schema.

### Element rules
- `type` must be `"obj"` or `"text"` only (no `"bg"`)
- Bbox format: `[y_min, x_min, y_max, x_max]` with normalized `0–1000` values
- Keep descriptions concise, visually drawable, using architectural mass nouns not individual props
- Use bboxes only when spatial control matters
- Max 5 elements per caption
- For text elements: include `text` field with exact visible text, escaped `\n` for line breaks

### Color rules
- Uppercase hex only in `color_palette`, e.g. `"#0B1020"`
- Global palette: max 16 colors
- Element palette: max 5 colors
- Natural-language color descriptions go in `desc`, `background`, `lighting`, or `aesthetics`, never in `color_palette`

### Exclusion pattern
Embed exclusions as positive constraints inside `compositional_deconstruction.background`, e.g. "no readable signage, no logos, no text, no characters, no silhouettes, no watermark".

## VN background defaults
- `medium: "digital illustration"`
- `aesthetics: "Japanese visual novel background art"`
- Do not include a `photo` field
- Preserve clear foreground floor space (~30%) for character sprites and dialogue UI

## Style mode
Default to `STYLE` mode for illustrated/VN work. Use `REALISM` for photographic output. Do not include mode in JSON — set it in the UI before importing.

## Scene-Type Bbox Templates

Select a layout template based on the scene description. These define semantic occupancy zones, not precise object placement. Intentional overlap at zone boundaries is correct for corner-type layouts.

### Corridor (one-point perspective)

```
elements:
  - type: obj
    bbox: [0, 0, 820, 420]       # Left wall mass in perspective
    desc: "Left wall region, perspective-angled surface"
  - type: obj
    bbox: [0, 580, 820, 1000]     # Right wall mass in perspective
    desc: "Right wall region, perspective-angled surface"
  - type: obj
    bbox: [120, 380, 840, 620]    # Central traversable route
    desc: "Central corridor route receding into depth"
  - type: obj
    bbox: [780, 0, 1000, 1000]    # Foreground floor surface
    desc: "Foreground floor plane, sprite staging zone"
```

**Rules:**
- Left/right wall bboxes intentionally overlap at x-boundaries (wall meets center route)
- Floor bbox is strictly below all others (y 780+) — do not merge with wall regions
- Max 5 elements total. Fifth slot for a specific object (door, panel, column) if needed
- Perspective goes in `compositional_deconstruction.background`, not in wall-plane element regions
- Prefer no regions when depth is the highest priority
- Avoid left/right wall regions unless the goal is a flat architectural elevation

### Room (enclosed space)

```
elements:
  - type: obj
    bbox: [0, 0, 1000, 400]      # Far wall / focal surface
    desc: "Far wall surface, primary focal plane"
  - type: obj
    bbox: [0, 0, 300, 1000]      # Left wall area
    desc: "Left wall surface, side architectural detail"
  - type: obj
    bbox: [700, 0, 1000, 1000]   # Right wall area
    desc: "Right wall surface, side architectural detail"
  - type: obj
    bbox: [750, 0, 1000, 1000]   # Foreground floor for sprite staging
    desc: "Foreground floor plane, character staging zone"
```

**Rules:**
- Far wall covers full x-span at the top third
- Left/right wall regions cover tall vertical strips at x-edges
- Floor zone overlaps the bottom of far wall — correct for camera looking slightly downward
- 4 elements; add a fifth for a focal object (desk, terminal, panel) on the far or side wall

### Open / Exterior

Not yet templated. Use a single background-only approach with 0–3 elements for key landmarks. Avoid region-heavy layouts that flatten outdoor depth.

### Misclassification guard

Some corridor-like spaces with wide cross-sections may be misclassified as rooms. When in doubt:
- Receding point visible → corridor
- All walls visible as planes → room
- Clarify the perspective choice in `background`.

## Word Budget Enforcement

After generating the JSON, calculate word count **before emitting**. If over budget, truncate `high_level_description` and element `desc` fields first, then `background`.

### Counting scope — sum word count across:
- `high_level_description`
- All `style_description.*` string fields (aesthetics, lighting, medium, art_style)
- `compositional_deconstruction.background`
- All element `desc` fields

### Excluded from count:
- Field names
- `bbox` arrays
- `color_palette` arrays
- `type` values
- `text` field on text elements

### Budget:
- Target: 150–350 words
- Hard ceiling: 400 words — truncate until under 350

## Validation Checklist

Run these checks before emitting JSON:

- [ ] Word count ≤ word budget (per counting scope above)
- [ ] Bboxes non-overlapping or intentionally overlapping per scene-type template
- [ ] Exclusion clauses present in `background` ("no characters, no text, no logos...")
- [ ] No prohibited top-level fields (`composition`, `negative_prompt`, `photo`, etc.)
- [ ] All hex uppercase (`#RRGGBB`)
- [ ] `medium` = `"digital illustration"` for VN backgrounds
- [ ] `aesthetics` contains `"Japanese visual novel background art"`
- [ ] No `photo` field for VN work
- [ ] No `STYLE`/`REALISM` mode in JSON (set in UI)
- [ ] Element `type` is `"obj"` or `"text"` only
- [ ] Max 5 elements, max 16 global colors, max 5 per-element colors
- [ ] JSON parses successfully
- [ ] Key ordering matches spec order (high_level_description → style_description → compositional_deconstruction)
- [ ] Bbox arrays use `[y_min, x_min, y_max, x_max]` format
- [ ] All bbox coordinates are integers 0–1000
- [ ] Region descriptions reinforce same scene type as `high_level_description`
- [ ] Corridor prompts do not default to left/right wall regions when depth is required

## Process Flow

1. **Read project context** — Load art direction documentation for vocabulary, palette, style anchors, prohibited phrasing
2. **Classify** — Determine scene type (corridor / room / exterior) from user description
3. **Select template** — Bbox layout from scene type (or skip regions entirely)
4. **Write draft** — Compose `high_level_description`, `style_description`, `compositional_deconstruction`
5. **Word budget check** — Count words; truncate if over
6. **Validation** — Run all checklist items
7. **Emit** — Output valid JSON, no fences, no explanations
