# Ideogram Regional Prompting for VN Backgrounds

**Status:** Provisional — based on ~5 controlled tests, needs validation via swap and camera-isolation tests.

This file documents how Ideogram's Prompt Builder workflow and regional prompting system behave for environment generation, distinct from the Z-Image Turbo findings in other references.

---

## Workflow Architecture

The Ideogram workflow uses two subsystems:

### 1. LLM Prompt Builder

Converts two inputs into structured JSON:

| Input field | Content |
|-------------|---------|
| High Level Description | Genre, setting, location identity |
| Background Description | Camera, perspective, composition |

Outputs a JSON payload containing:
- `high_level_description` — scene identity
- `style_description` — art direction block
- `background` — environment details
- `elements` — object array

The Prompt Builder itself is an LLM wrapper. The embedded example prompt (NSFW character) is a leftover test case and does not affect behavior.

### 2. Regional Object System

Ideogram exposes object-level JSON regions visually as colored boxes:

```json
{
  "type": "obj",
  "bbox": [x1, y1, x2, y2],
  "desc": "architectural mass description"
}
```

Each region constrains what the model places in that bounding box area.

---

## Observed Hierarchy Model

From sequential tests, the influence order appears to be:

| Level | Input | Controls | Strength |
|-------|-------|----------|----------|
| 1 | Regional object descriptions | Semantic zone occupancy (what goes where) | Strongest |
| 2 | Background description | Camera, perspective, vanishing point | Moderate |
| 3 | High-Level Description | Genre, setting, scene identity | Weakest |

**Key finding:** Regional descriptions can overpower the high-level description. A corridor test with command-center regions produced a command center, not a corridor. Every region must reinforce the intended scene type.

---

## Regional Prompting Behavior

### Semantic Occupancy, Not Layout

Regions do not give precise pixel-level control. Instead they assign *semantic zones*:

```
Region [bottom] → "empty polished floor"
```

means:

```
This area should primarily contain floor-related concepts
```

The model then builds a coherent scene around those anchors.

### Architectural Masses Over Individual Props

| Works well | Works poorly |
|------------|--------------|
| "registration counters" | "keyboard" |
| "observation windows" | "camera" |
| "industrial ceiling" | "light fixture" |
| "sealed security doors" | "handle" |
| "analyst workstations" | "monitor" |

Regions respond to **architectural modules** — spatially significant elements that define a room's structure. Individual props are ignored or placed inconsistently.

### Perspective is Global, Not Regional

A corridor is:

> all elements converge toward one vanishing point

Regions cannot describe this. Perspective must come from the **background description** layer. Tests splitting scenes into left-wall/right-wall regions produced flat front-facing rooms, not corridors.

---

## Style Field Configuration

The workflow exposes these style fields. Defaults inherited from the NSFW example prompt are actively harmful for VN backgrounds.

### Recommended VN Background Profile

| Field | Setting | Rationale |
|-------|---------|-----------|
| style | `artstyle` | Activates style block |
| artstyle | `Japanese visual novel background` | Triggers VN training data, shifts from photorealism |
| photo | *(blank)* | Avoid photographic realism bias |
| aesthetics | `Japanese visual novel background art, institutional architecture, environmental storytelling` | Replaces character-focused defaults |
| lighting | `cold institutional lighting, cool white LED, deep shadows` | Retained from original — actually useful |
| medium | `digital illustration` or `background illustration` | Overrides `photography` default |

**Evidence:** Changing medium from `photography` to `digital illustration` and aesthetics from character descriptions to architectural language produced a detectable shift toward anime VN style across all tested scenes.

---

## Recommended Region Strategies

### Strategy A: Structural Zones (Recommended for rooms)

Divide the canvas into architectural planes — works for any room-based scene.

| Region | Position | Description style |
|--------|----------|-------------------|
| Floor | Bottom 35-40% | "empty polished floor" — reserves sprite space |
| Left wall | Left third | Wall-specific architecture (doors, partitions, counters) |
| Right wall | Right third | Complementary architecture (windows, glass, displays) |
| Ceiling | Top 15-20% | Infrastructure (lights, vents, beams) |
| Center/Background | (from bg description) | Vanishing point, depth, focal architecture |

### Strategy B: Depth Layers (Experimental)

Divide by foreground/midground/background instead of left/right. Untested.

| Region | Position | Description style |
|--------|----------|-------------------|
| Foreground | Bottom 35% | Empty floor, no objects |
| Midground | Middle 40% | Primary architecture, furniture, equipment |
| Background | Top 25% | Windows, displays, distant architecture |

### Strategy C: Functional Zones

Divide by room function. Good for multi-use spaces (command centers, intake halls).

| Region | Position | Description style |
|--------|----------|-------------------|
| Entry/Floor | Bottom | Entry area, empty floor |
| Processing | Left | Workstations, counters |
| Operations | Center | Central command, main equipment |
| Oversight | Right | Observation, displays |
| Infrastructure | Top | Ceiling, lighting, gallery |

---

## What Regions Do Well

| Feature | Result |
|---------|--------|
| Reserving sprite space (empty floor) | Strong — bottom region consistently respected |
| Wall-type differentiation (doors vs glass) | Strong — left/right regions obeyed |
| Ceiling placement | Strong — top region consistently rendered |
| Architectural consistency between rooms | Promising — same region layout reused across scenes creates visual continuity |
| Preventing object clutter | Strong — confining equipment to specific zones |

## What Regions Do Poorly

| Feature | Result |
|---------|--------|
| Perspective / vanishing point | Weak — needs background description, not regions |
| Individual prop placement | Weak — architectural masses only |
| Text/signage control | Moderate — Ideogram may invent signage from security language |
| Long corridors | Weak — regions produce front-facing rooms |

---

## Region Description Style Guide

### DO

- Use architectural mass nouns: "counters", "partitions", "workstations", "windows"
- Add material qualifiers: "reinforced observation glass", "matte charcoal panels"
- Keep to 3-6 words per region
- Every region must reinforce the same scene type

### DON'T

- Use individual props: "keyboard", "monitor", "camera", "handle"
- Mix scene types across regions (corridor regions + command center regions = confused model)
- Use abstract concepts: "oversight", "containment", "authority"

### Example: Good vs Bad

| Bad (props) | Good (architectural masses) |
|-------------|----------------------------|
| security camera on wall | observation glass partitions |
| door handle and lock | sealed security doors |
| desk with computer | analyst workstation row |
| ceiling light | industrial ceiling with LED channels |

---

## Open Questions

These need controlled tests before promoting findings to SKILL.md:

1. **Swap test:** Swap region descriptions while keeping everything else fixed — does the model follow regions or background description?
2. **Camera isolation test:** Same regions + different background descriptions — does camera change?
3. **Zero-region baseline:** Same scene with and without regions — what does each layer contribute?
4. **5-region room template:** Can a shared 5-region layout be reused across intake lobby, briefing room, command center, laboratory to create visual consistency?
5. **Corridor region strategy:** Can regions be used for corridor depth if structured as foreground/midground/background instead of left/right?
6. **Text suppression:** Does adding "no visible signage, no text" to regions suppress Ideogram's text-generation tendency?

---

## Relationship to Z-Image Turbo Principles

The Z-Image Turbo principles were developed for a model that:
- Does not support negative prompts (guidance_scale=0)
- Responds best to security infrastructure > architecture > materials > lighting
- Requires all constraints in positive prompt

Ideogram's regional system offers capabilities the Z-Image Turbo principles don't cover:
- Explicit floor reservation (sprite space)
- Left/right wall differentiation
- Ceiling placement
- Multi-zone semantic control

However, the **visible consequences principle (P1)** and **architectural rhythm principle (P2)** likely transfer — they describe how models in general respond to concrete physical elements over abstract concepts.
