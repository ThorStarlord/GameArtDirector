# Ideogram 4 Regional Prompting and JSON Layout Guide

**Status:** Production guidance with provisional empirical notes. Official schema details come from Ideogram's public documentation; behavior notes come from this repository's controlled prompt tests.

This guide documents how to use Ideogram 4 structured JSON captions and bounding-box regional prompting for game-art assets, VN backgrounds, posters, logos, social templates, product shots, and infographics.

Sources:
- Ideogram technical blog: <https://ideogram.ai/blog/ideogram-4.0/>
- Official Ideogram 4 prompting guide: <https://github.com/ideogram-oss/ideogram4/blob/main/docs/prompting.md>
- Ideogram Describe API docs: <https://developer.ideogram.ai/api-reference/api-reference/describe>
- Ideogram V4 Magic Prompt API docs: <https://developer.ideogram.ai/api-reference/api-reference/magic-prompt-v4>

---

## Architectural Model

Ideogram 4 is a 9.3B, 34-layer, single-stream Diffusion Transformer trained from scratch. It uses a frozen Qwen3-VL-8B-Instruct text encoder and feeds hidden states from 13 intermediate Qwen layers into the DiT conditioning stream.

The important prompting implication is that text tokens, image latent tokens, and structured layout supervision are handled inside a shared multimodal generation context. Ideogram 4 uses shared multidimensional rotary position embeddings (MRoPE), so element descriptions and image positions can attend through a shared spatial frame.

### Why JSON prompts work better than prose

Ideogram 4 was trained on structured JSON captions. Matching that caption shape reduces train/eval mismatch and gives the model explicit grounding fields:

- `high_level_description`: global scene intent.
- `style_description`: aesthetics, lighting, medium, photo/art_style, palette.
- `compositional_deconstruction.background`: global stage and perspective.
- `compositional_deconstruction.elements`: typed local regions.
- `bbox`: normalized spatial region for an element.
- `text`: literal text to render for `type: "text"`.
- `desc`: local visual instructions for that element.
- `color_palette`: global or element-level color steering.

Natural language can say "put the title near the top"; JSON can bind a concrete `text` element to `[60, 90, 220, 910]`. The latter is a stronger grounding signal.

### Repo-specific palette rule

The official Ideogram schema describes uppercase hex values in `color_palette`. This repository intentionally does **not** use hex values in Ideogram `color_palette` arrays because local tests found weak-to-no output influence from hex palettes.

Use named color descriptions instead:

```json
"color_palette": [
  "deep navy-black background",
  "warm ivory typography",
  "muted brass accent",
  "icy cyan signal glow"
]
```

If exact hex references matter for a downstream brand system, put them in prose outside `color_palette`, for example: `brand reference: deep navy approximately #080A0E`. Do not put hex strings in Ideogram `color_palette` arrays for this repo's production prompts.

---

## Coordinate System

Ideogram 4 bounding boxes use normalized `0-1000` coordinates with origin at the top-left.

The official order is:

```text
[y_min, x_min, y_max, x_max]
```

Do not use `[x_min, y_min, x_max, y_max]`.

```text
0,0 ┌──────────────────────────────┐ 0,1000
    │                              │
    │                              │
    │                              │
    │                              │
1000,0 └───────────────────────────┘ 1000,1000
```

Example:

```json
"bbox": [100, 200, 400, 800]
```

Means:

```text
top edge:    y = 100
left edge:   x = 200
bottom edge: y = 400
right edge:  x = 800
```

Visualized:

```text
0,0 ┌──────────────────────────────┐
    │                              │
100 │      ┌────────────────┐      │
    │      │    element     │      │
400 │      └────────────────┘      │
    │                              │
1000└──────────────────────────────┘
     x=0  x=200          x=800 x=1000
```

Bounding boxes are soft semantic regions, not exact masks. Leave gutters between elements and avoid overlaps unless blending is desired.

---

## Common Layout Matrices

### Rule of thirds

```text
x:        0          333          666         1000
          │           │            │            │
y=0      ┌───────────┬────────────┬────────────┐
         │ top-left  │ top-center │ top-right  │
y=333    ├───────────┼────────────┼────────────┤
         │ mid-left  │ center     │ mid-right  │
y=666    ├───────────┼────────────┼────────────┤
         │ bot-left  │ bot-center │ bot-right  │
y=1000   └───────────┴────────────┴────────────┘
```

```json
{
  "top_left": [0, 0, 333, 333],
  "top_center": [0, 333, 333, 666],
  "top_right": [0, 666, 333, 1000],
  "middle_left": [333, 0, 666, 333],
  "middle_center": [333, 333, 666, 666],
  "middle_right": [333, 666, 666, 1000],
  "bottom_left": [666, 0, 1000, 333],
  "bottom_center": [666, 333, 1000, 666],
  "bottom_right": [666, 666, 1000, 1000]
}
```

### Centered hero

```text
0,0 ┌──────────────────────────────┐
    │                              │
150 │        ┌────────────┐        │
    │        │  hero obj  │        │
850 │        └────────────┘        │
    │                              │
1000└──────────────────────────────┘
```

```json
"bbox": [150, 250, 850, 750]
```

Use for product bottles, character busts, logo marks, hero icons, and isolated props.

### Top banner

```text
0,0 ┌──────────────────────────────┐
    │        top banner text       │
180 ├──────────────────────────────┤
    │                              │
1000└──────────────────────────────┘
```

```json
"bbox": [40, 80, 180, 920]
```

Use for title cards, event posters, store graphics, and social headers.

### Footer signature

```text
0,0 ┌──────────────────────────────┐
    │                              │
850 ├──────────────────────────────┤
    │       footer signature       │
1000└──────────────────────────────┘
```

```json
"bbox": [850, 80, 960, 920]
```

Use for dates, subtitles, brand tags, platform labels, and small CTA text.

### VN background safe zones

```text
0,0 ┌──────────────────────────────┐
    │ background architecture      │
250 ├──────────────────────────────┤
    │ midground identity anchors   │
650 ├──────────────────────────────┤
    │ clear sprite/dialogue floor  │
1000└──────────────────────────────┘
```

```json
{
  "background_identity": [0, 0, 650, 1000],
  "clear_foreground_floor": [650, 0, 1000, 1000]
}
```

Use this for visual novel backgrounds where the lower frame must remain available for sprites, dialogue UI, or menu overlays.

---

## Valid JSON Layout Example

This example follows this repo's production palette rule: named color descriptions, not hex values.

```json
{
  "high_level_description": "A premium launch poster for a fictional audio brand called NOCTURNE, featuring a flat vector crescent-wave logo separated from large typography, with a dark editorial layout and precise spatial hierarchy.",
  "style_description": {
    "aesthetics": "premium, minimal, editorial, high-contrast, geometric, calm",
    "lighting": "even flat graphic lighting with subtle vignette depth and no realistic cast shadows",
    "medium": "graphic_design",
    "art_style": "flat vector poster design with crisp edges, controlled negative space, refined serif and grotesk typography",
    "color_palette": [
      "deep blue-black poster field",
      "warm ivory typography",
      "muted antique-gold accent",
      "icy cyan signal glow",
      "cool slate-blue shadow detail"
    ]
  },
  "compositional_deconstruction": {
    "background": "Deep blue-black poster field with a faint radial gradient behind the central mark, very subtle paper grain, clean empty margins, no extra symbols, no decorative border, no watermark.",
    "elements": [
      {
        "type": "obj",
        "bbox": [170, 320, 520, 680],
        "desc": "Flat vector crescent-wave logo mark, centered in the upper-middle poster area, composed of one muted antique-gold crescent curve intersecting three icy cyan sound-wave arcs, crisp geometric construction, isolated from all typography with clear negative space.",
        "color_palette": [
          "muted antique-gold crescent",
          "icy cyan sound-wave arcs",
          "deep blue-black negative space"
        ]
      },
      {
        "type": "text",
        "bbox": [560, 120, 700, 880],
        "text": "NOCTURNE",
        "desc": "Large uppercase luxury serif wordmark, centered horizontally, high contrast strokes, generous letter spacing, warm ivory ink, perfectly straight baseline.",
        "color_palette": [
          "warm ivory letterforms",
          "muted antique-gold edge warmth"
        ]
      },
      {
        "type": "text",
        "bbox": [715, 210, 815, 790],
        "text": "SPATIAL AUDIO\nFOR QUIET ROOMS",
        "desc": "Two-line compact grotesk subtitle, centered, small uppercase letters, clean tracking, cool slate-blue text with a subtle icy cyan accent on the second line.",
        "color_palette": [
          "cool slate-blue subtitle",
          "icy cyan accent"
        ]
      },
      {
        "type": "obj",
        "bbox": [825, 80, 930, 920],
        "desc": "Thin horizontal footer rule and small abstract calibration ticks spread across the lower margin, restrained technical detailing, not text, not a barcode.",
        "color_palette": [
          "cool slate-blue rule",
          "muted antique-gold ticks"
        ]
      },
      {
        "type": "text",
        "bbox": [935, 290, 980, 710],
        "text": "2026 PRODUCT SYSTEM",
        "desc": "Tiny centered footer signature in narrow sans-serif capitals, low contrast slate-blue color, aligned to the bottom safe zone.",
        "color_palette": [
          "cool slate-blue footer text"
        ]
      }
    ]
  }
}
```

Notes:
- Use `obj` for marks, products, environment structures, rules, dividers, and non-text graphic details.
- Use `text` only for literal text that should appear in the image.
- Multi-line text must use escaped `\n`.
- Keep element descriptions focused on the content inside that element's box.
- Keep `background` responsible for global perspective and stage logic.

---

## Production Recipes

### Brand identity and logos

Goal: isolate flat vector graphics from typography so the symbol does not absorb letters or the wordmark.

Rules:
- Symbol mark is `type: "obj"`.
- Wordmark is `type: "text"`.
- Boxes do not overlap.
- Say "no letters inside the symbol" when the mark must stay abstract.
- Use large negative space between mark and typography.

```json
{
  "high_level_description": "A clean brand identity lockup for a fictional botanical technology company called VERDANT NODE, with the symbol separated from the typography.",
  "style_description": {
    "aesthetics": "minimal, precise, modern, botanical, technical",
    "lighting": "flat even graphic lighting with no shadows",
    "medium": "graphic_design",
    "art_style": "flat vector logo presentation on a plain background, crisp edges, professional brand board",
    "color_palette": [
      "warm off-white brand board",
      "dark forest-green wordmark",
      "fern-green symbol planes",
      "pale leaf-green highlight",
      "muted ochre accent"
    ]
  },
  "compositional_deconstruction": {
    "background": "Warm off-white brand board background with no texture, no mockup, no paper folds, no watermark.",
    "elements": [
      {
        "type": "obj",
        "bbox": [190, 350, 470, 650],
        "desc": "Abstract flat vector logo mark: a hexagonal node shape formed by three green leaves and three small circuit dots, centered above the wordmark, no letters, no initials, no text inside the symbol.",
        "color_palette": [
          "dark forest-green outline",
          "fern-green leaf planes",
          "pale leaf-green highlight",
          "muted ochre circuit dots"
        ]
      },
      {
        "type": "text",
        "bbox": [540, 180, 650, 820],
        "text": "VERDANT NODE",
        "desc": "Large uppercase geometric sans-serif wordmark, dark forest-green, wide tracking, perfectly centered, clean straight baseline.",
        "color_palette": [
          "dark forest-green wordmark"
        ]
      },
      {
        "type": "text",
        "bbox": [675, 270, 730, 730],
        "text": "BIOLOGICAL INTERFACES",
        "desc": "Small uppercase subtitle, muted green, narrow sans-serif, centered below the brand name.",
        "color_palette": [
          "muted green subtitle"
        ]
      }
    ]
  }
}
```

### Social media template framework

Goal: reserve stable text zones across multiple generations while the central visual changes.

Rules:
- Keep the same headline, subject, and footer boxes across the series.
- Change only `background`, subject `desc`, and palette.
- Keep literal text short.
- Use wide text boxes with enough height for line breaks.

```json
{
  "high_level_description": "A reusable square social media announcement template for a design workshop, with fixed headline, central image zone, and footer call-to-action.",
  "style_description": {
    "aesthetics": "bold, editorial, modular, contemporary, high-readability",
    "lighting": "flat graphic lighting with soft background depth",
    "medium": "graphic_design",
    "art_style": "modern social media poster template with strong grid structure and controlled negative space",
    "color_palette": [
      "warm cream background",
      "inky black typography",
      "coral-red accent",
      "clear cobalt-blue panels",
      "sunlit yellow highlight",
      "clean white card surfaces"
    ]
  },
  "compositional_deconstruction": {
    "background": "Warm cream poster background with subtle geometric blocks and generous margins. The top and bottom zones remain clean for text.",
    "elements": [
      {
        "type": "text",
        "bbox": [60, 80, 230, 920],
        "text": "DESIGN\nSYSTEMS LIVE",
        "desc": "Large two-line uppercase headline, bold condensed sans-serif, black letters, tight leading, centered in the top safe zone.",
        "color_palette": [
          "inky black headline"
        ]
      },
      {
        "type": "obj",
        "bbox": [280, 140, 720, 860],
        "desc": "Central modular abstract illustration: layered UI cards, color swatches, layout grids, and cursor arrows arranged as a clean editorial collage, no readable text.",
        "color_palette": [
          "coral-red accent shapes",
          "clear cobalt-blue panels",
          "sunlit yellow highlights",
          "clean white card surfaces",
          "inky black grid lines"
        ]
      },
      {
        "type": "text",
        "bbox": [790, 100, 875, 900],
        "text": "JUNE 24 / ONLINE WORKSHOP",
        "desc": "Medium uppercase footer line, clean grotesk sans-serif, centered, black text with one coral-red slash accent.",
        "color_palette": [
          "inky black footer text",
          "coral-red slash accent"
        ]
      },
      {
        "type": "text",
        "bbox": [895, 250, 945, 750],
        "text": "REGISTER TODAY",
        "desc": "Small call-to-action in bold uppercase sans-serif, white text inside a coral-red pill-shaped button.",
        "color_palette": [
          "clean white button text",
          "coral-red button fill"
        ]
      }
    ]
  }
}
```

### Product photography and infographics

Goal: maintain asymmetrical balance and prevent feature labels from overlapping the product.

Rules:
- Product occupies one side.
- Labels occupy the opposite side.
- Connector lines are `obj`, not `text`.
- Each label gets its own `text` box.
- Keep label copy short.

```json
{
  "high_level_description": "A premium product infographic for a wireless speaker, with the product on the left and three clean feature labels on the right.",
  "style_description": {
    "aesthetics": "premium, technical, clean, asymmetrical, precise",
    "lighting": "soft studio lighting with controlled highlights and gentle contact shadow",
    "photo": "85mm product photography, sharp focus, high-end catalog lighting",
    "medium": "photograph",
    "color_palette": [
      "warm stone studio sweep",
      "matte charcoal product body",
      "brushed bronze control ring",
      "dark charcoal typography",
      "soft ivory highlight"
    ]
  },
  "compositional_deconstruction": {
    "background": "Warm stone-colored studio sweep with subtle gradient, clean negative space on the right side for labels, no extra props, no watermark.",
    "elements": [
      {
        "type": "obj",
        "bbox": [180, 80, 850, 470],
        "desc": "Matte charcoal cylindrical wireless speaker standing upright on the left side, premium fabric mesh texture, brushed bronze control ring at top, soft contact shadow.",
        "color_palette": [
          "matte charcoal fabric mesh",
          "brushed bronze control ring",
          "soft ivory edge highlight"
        ]
      },
      {
        "type": "obj",
        "bbox": [250, 470, 720, 620],
        "desc": "Three thin bronze connector lines extending from the speaker toward the right-side label area, clean infographic style, no arrowheads, no text.",
        "color_palette": [
          "brushed bronze connector lines"
        ]
      },
      {
        "type": "text",
        "bbox": [210, 650, 300, 940],
        "text": "360 SOUND",
        "desc": "Bold uppercase sans-serif feature label, dark charcoal text, aligned left inside the upper-right label zone.",
        "color_palette": [
          "dark charcoal feature text"
        ]
      },
      {
        "type": "text",
        "bbox": [430, 650, 520, 940],
        "text": "24H BATTERY",
        "desc": "Bold uppercase sans-serif feature label, dark charcoal text, aligned left inside the middle-right label zone.",
        "color_palette": [
          "dark charcoal feature text"
        ]
      },
      {
        "type": "text",
        "bbox": [650, 650, 740, 940],
        "text": "WATER RESISTANT",
        "desc": "Bold uppercase sans-serif feature label, dark charcoal text, aligned left inside the lower-right label zone.",
        "color_palette": [
          "dark charcoal feature text"
        ]
      }
    ]
  }
}
```

---

## Regional Prompting Behavior

### Semantic occupancy, not pixel-perfect layout

Regions assign the dominant semantic content for a zone:

```text
bottom region -> "wide empty polished floor"
```

means:

```text
this area should primarily read as open floor
```

The model still builds a coherent image around the region. It may slightly overflow, resize, or reposition objects to satisfy the whole composition.

### Observed hierarchy

From this repo's Ideogram tests, the approximate influence order is:

| Level | Input | Controls | Strength |
|-------|-------|----------|----------|
| 1 | Regional element descriptions | What occupies each zone | Strongest |
| 2 | Background description | Camera, perspective, vanishing point | Moderate |
| 3 | High-level description | Genre, setting, scene identity | Weakest |

Every regional description must reinforce the same room or asset type as the high-level description. If the high-level description says "containment corridor" but the regions say "analyst workstations" and "tactical displays", the image may become a command center.

### Architectural masses beat individual props

| Works well | Works poorly |
|------------|--------------|
| "registration counters" | "keyboard" |
| "observation windows" | "camera" |
| "industrial ceiling" | "light fixture" |
| "sealed security doors" | "handle" |
| "analyst workstation row" | "single monitor" |

Regions respond to spatially meaningful modules. Use object vocabulary that defines the room's structure.

### Perspective is global

Perspective belongs in `compositional_deconstruction.background`, not in isolated regions. A corridor is a depth composition where the whole frame converges toward a vanishing point. Splitting a corridor into left-wall/right-wall/floor/ceiling regions can produce a flat front-facing room.

For corridors:
- Use no regions if depth is the highest priority.
- Or use foreground/midground/background regions.
- Put explicit depth in `background`: "centered vanishing point, long uninterrupted corridor axis, repeating floor markings leading forward."
- Avoid left-wall/right-wall regions unless the intended output is a flat architectural elevation.

---

## Region Strategies

### Strategy A: Structural zones for rooms

Use for offices, command centers, intake rooms, lobbies, labs, shops, social templates, and product layouts.

| Region | Box | Description style |
|--------|-----|-------------------|
| Floor | `[650, 0, 1000, 1000]` | "wide empty polished floor for sprite space" |
| Left architecture | `[180, 0, 760, 360]` | "sealed security doors and access-reader cadence" |
| Right architecture | `[180, 640, 760, 1000]` | "reinforced observation glass partitions" |
| Ceiling | `[0, 0, 180, 1000]` | "industrial ceiling with recessed LED channels" |
| Center mass | `[220, 300, 650, 700]` | "central intake counter block" |

Do not use this as the default for long corridors.

### Strategy B: Depth layers for corridors

Use when the scene must read as a passage, transit spine, or hallway.

| Region | Box | Description style |
|--------|-----|-------------------|
| Foreground | `[650, 0, 1000, 1000]` | "wide uncluttered foreground floor, no objects" |
| Midground | `[300, 80, 700, 920]` | "repeating corridor wall panels and access-door rhythm" |
| Background | `[80, 260, 380, 740]` | "distant corridor continuation fading into controlled darkness" |

Keep the background description responsible for vanishing point and camera.

### Strategy C: Functional zones for multi-use spaces

Use for command centers, screening halls, market stalls, crafting rooms, inventory UI backplates, and other layouts with distinct functional areas.

| Region | Box | Description style |
|--------|-----|-------------------|
| Entry/floor | `[680, 0, 1000, 1000]` | "clear entry floor and circulation lane" |
| Processing | `[250, 60, 680, 360]` | "secure processing counters" |
| Operations | `[220, 360, 650, 650]` | "central command table" |
| Oversight | `[220, 650, 680, 960]` | "reinforced observation windows" |
| Infrastructure | `[0, 0, 220, 1000]` | "ceiling beams and measured LED channels" |

Every functional zone must belong to the same room type.

---

## VN Background Profile

Use this style profile for Ideogram 4 VN backgrounds:

| Field | Setting | Rationale |
|-------|---------|-----------|
| `medium` | `digital illustration` or `background illustration` | Avoids photographic bias |
| `art_style` | `Japanese visual novel background art` | Targets VN background rendering |
| `photo` | omit | Do not include both `photo` and `art_style` |
| `aesthetics` | `Japanese visual novel background art, institutional architecture, environmental storytelling` | Replaces character-focused defaults |
| `lighting` | visible light behavior, not mood labels | More reliable than "cinematic" or "cool" alone |

Evidence: changing `medium` from `photography` to `digital illustration` and replacing character aesthetics with architectural language produced a detectable shift toward VN background style across tested scenes.

---

## Failure Modes and Corrections

| Failure | Cause | Correction |
|---------|-------|------------|
| Regional scene lock | Regions describe a different room type than the high-level description | Make every element reinforce the same scene identity |
| Flat corridor | Left/right wall regions turn the frame into a front-facing room | Use no regions or foreground/midground/background regions |
| Vertical overpowering depth | Strong vertical cues overwhelm corridor circulation | Maintain at least a 1:1 ratio of depth cues to vertical cues |
| Component collapse | Too many specific prop types in a large space become repeated generic terminals | Use fewer, larger architectural masses and spatial staging |
| Signage/text bleed | Security or institutional prompts trigger invented labels | End with "no text, no signage, no logos, no watermarks" unless text is requested |

For detailed scoring templates and examples, see `result-analysis.md`:
- CT-18: Regional Scene Lock
- CT-19: Flat Composition / No Perspective Depth
- CT-20: Vertical Signal Overpowering Circulation
- SP-09: Regional Zone Compliance

---

## Magic Prompt and Describe

### Magic Prompt

Use Ideogram V4 Magic Prompt to convert rough natural language into a first-pass structured JSON caption:

```text
POST /v1/ideogram-v4/magic-prompt
```

Use it for:
- Drafting a structured caption quickly.
- Discovering an initial element breakdown.
- Converting plain art direction into JSON.
- Getting a first-pass layout before manual bbox editing.

Do not treat Magic Prompt output as final for production. Manually inspect key order, element types, boxes, text strings, palette wording, and scene consistency.

### Describe

The official API docs expose Describe at:

```text
POST /describe
```

Set `describe_model_version` to `V_4` when reverse-engineering an image for Ideogram 4. The official docs do not list `/v1/ideogram-v4/describe` as the canonical path.

Use Describe for:
- Reverse-engineering an existing layout.
- Extracting visible composition from reference images.
- Producing raw prose that can be converted into structured JSON.

Recommended workflow:
1. Run Describe on the reference image.
2. Convert the result into JSON with `high_level_description`, `style_description`, `background`, and `elements`.
3. Add or correct `bbox` manually.
4. Replace hex palettes with named color descriptions for this repo.
5. Remove any accidental text, watermark, or brand leakage unless explicitly needed.

---

## Node Length and Conflict Management

Keep structured nodes short enough for the model to bind content to region:

| Node | Recommended length |
|------|--------------------|
| `high_level_description` | 1-2 sentences |
| `background` | 40-120 words |
| `obj.desc` | 25-90 words |
| `text.desc` | 12-45 words |
| literal `text` | short, exact, intentionally line-broken |

When a node exceeds roughly 150-160 words, split it into fewer stronger concepts or move global details to `background`. Long regional nodes dilute local control and increase noun conflicts.

Conflict rules:
- Element-level `bbox` + `desc` usually beats high-level prose.
- Literal `text` should match the requested typography and box size.
- `background` should own perspective and camera.
- `style_description` should own global medium, lighting, and palette.
- Do not ask a narrow box to contain long text.
- Do not describe a horizontal object inside a tall narrow box.
- Do not use overlapping boxes unless you want visual fusion.

---

## Relationship to Z-Image Turbo Principles

The Z-Image Turbo principles were developed for a model that:
- Does not support negative prompts (`guidance_scale=0`)
- Responds best to security infrastructure > architecture > materials > lighting
- Requires all constraints in positive prompt

Ideogram 4 regional prompting adds:
- Explicit semantic zone control through `bbox`
- More reliable text rendering via `type: "text"`
- Stronger layout decomposition for posters, templates, and infographics
- Better floor/safe-zone reservation for VN backgrounds

The transferable principle remains the same: concrete visible nouns drive geometry more reliably than abstract labels.
