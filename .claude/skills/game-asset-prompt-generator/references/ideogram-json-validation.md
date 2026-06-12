# Ideogram JSON Validation

Use this checklist before returning production Ideogram 4 JSON captions, especially for VN backgrounds, regional prompts, and batch-consistent environments.

## Source Anchor Pass

Before drafting or finalizing JSON, check whether the request implies any stronger source than the current rough prompt:

1. User-provided prompt or explicit correction in the current request.
2. Approved project prompt files, prompt corpus entries, or orchestrator prompts.
3. Master style guides or canon art-direction documents.
4. Batch, faction, location, or environment anchors.
5. Skill references and general model-behavior notes.

Preserve source anchors by distributing them into the right JSON fields:
- `style_description.aesthetics`: medium, genre, craft reference, audience-facing style.
- `style_description.art_style`: illustration lineage and rendering mode.
- `style_description.lighting`: lighting behavior and institutional mood.
- `style_description.color_palette`: uppercase hex values only.
- `compositional_deconstruction.background`: camera, perspective, stage logic, exclusions.
- Element `desc` fields: local architectural masses and continuity anchors.

Do not append large raw anchor quotations to the JSON. Compress them into field-native language.

## Word Budget

Target 150-350 words across generated prose fields. Treat 400 words as a hard ceiling unless the user explicitly asks for an exploratory draft.

Count:
- `high_level_description`
- all string fields inside `style_description`
- `compositional_deconstruction.background`
- all element `desc` fields

Do not count field names, `type` values, bbox arrays, or color palette arrays. Literal `text` values should stay short and exact when requested, but do not use them to carry hidden prompt detail.

If over budget, compress by:
- removing repeated material lists from regional descriptions
- moving global perspective and lighting out of elements and into `background` or `style_description`
- reducing the number of elements before shortening every sentence equally
- replacing abstract context labels with fewer concrete visible nouns

## Scene-Type Pass

Classify the layout before choosing regional prompting:

- Long corridor or transit spine: prefer no regions when depth is the priority; otherwise use foreground, midground, and background regions. Put vanishing point and camera in `background`.
- Flat architectural elevation: left/right wall regions are allowed because flatness is intended.
- Enclosed room: use structural zones such as floor, ceiling, left architecture, right architecture, and center mass.
- Exterior: use broad spatial layers such as foreground staging, midground structure, background skyline, and sky/weather.
- Poster, logo, UI, product, or infographic: use bboxes for stable text/object separation and short literal text.

Every element description must reinforce the same scene type as the high-level description. Region descriptions override vague high-level labels, so do not mix corridor regions with command-center, lab, or office vocabulary unless that is the requested room type.

## Validation Checklist

Structural checks:
- JSON uses the expected top-level order: `high_level_description`, `style_description`, `compositional_deconstruction`.
- No prohibited top-level prompt fields such as `negative_prompt`, `composition`, `prompt`, `quality`, or model UI settings.
- `style_description` uses exactly one of `photo` or `art_style` when present.
- VN backgrounds use ART_STYLE mode: omit `photo`, set `medium` to `digital illustration` or `background illustration`, and include `Japanese visual novel background art` in the style language.
- `compositional_deconstruction.background` appears before `elements`.
- Element `type` is only `obj` or `text`.
- Element order follows the spec: `type`, `bbox`, `text` if needed, `desc`, `color_palette`.
- Bboxes use `[y_min, x_min, y_max, x_max]`, normalized integer coordinates from 0 to 1000.
- Bboxes avoid unplanned overlap. Use overlap only when the scene strategy intentionally blends zones.
- Keep elements to 3-5 major regions for regional prompts unless the request requires a text-heavy layout.
- Global palettes use uppercase `#RRGGBB`; prefer 5-12 colors and never exceed 16. Element palettes never exceed 5 colors.

Semantic checks:
- Master style and batch anchors are present when source files or user context provide them.
- The prompt uses project-approved material ethos, palette logic, and architectural vocabulary before inventing new terms.
- `background` owns camera, perspective, vanishing point, and global stage logic.
- Element descriptions own local visible masses, not global lore.
- Regions are architectural or graphic masses, not individual small props.
- Each key object has a clear positive identity and a negative boundary when drift is likely.
- Exclusions are present for unwanted generated content: no characters, no readable signage, no logos, no UI, no watermark, unless explicitly requested.
- Prohibited or drift-prone project phrases from source notes have been replaced with approved vocabulary.

Corridor-specific checks:
- Do not default to left/right wall regions for a depth corridor.
- Preserve a clear passage axis with repeated depth cues such as panel rhythm, access-reader cadence, floor safety-strip continuity, or light-pool rhythm.
- Keep foreground floor usable for VN sprites or dialogue overlay when relevant.
- Avoid adding desks, chairs, loose monitors, instrument clusters, or workstation interiors unless the requested room is a lab, control room, or office.
- Behind-glass areas should read as abstract glow or broad silhouettes unless specific room detail is requested.

## Post-Generation Loop

When validating generated images, compare the output against the requested source anchors:
- What source vocabulary appeared visually?
- What source vocabulary was ignored or replaced by generic genre language?
- Did region descriptions override the intended room type?
- Did anchors survive as visible materials, lighting, architecture, or only as invisible lore?

Record the attempt in `result-analysis.md` and successful corrections in `prompt-corpus.md`.
