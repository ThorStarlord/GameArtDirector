---
name: suno-audio-asset-generator
description: Create production-ready prompts and generation plans for game audio assets using Suno-style text-to-audio or text-to-music models. Use when users ask for background music, loops, ambience, stingers, jingles, combat music, menu music, character themes, adaptive layers, sound beds, or other game audio assets intended for Suno or a Suno API/provider workflow.
---

# Suno Audio Asset Generator

## Quick Start

Turn a rough game audio request into Suno-ready prompt variants, instrumental style strings, and a practical generation plan.

Default to Suno instrumental style writing for game music unless the user asks for lyrics, vocals, full prompt text, API calls, files, or provider integration. If provider/API execution is requested, first read [Suno provider notes](references/suno-provider-notes.md).

## Workflow

1. Identify the audio asset type: loop, full track, ambience bed, stinger, transition, UI jingle, trailer cue, character theme, location theme, adaptive layer, or source music.
2. Identify the in-game function: exploration, combat, menu, dialogue, victory, defeat, stealth, tension, shop, cutscene, location identity, faction identity, or feedback cue.
3. Infer production constraints only when useful: target duration, looping behavior, intensity, tempo range, genre, instrumentation, diegetic/non-diegetic role, and whether vocals are allowed.
4. Choose the correct output format:
   - Instrumental request: return `style` variants only, with `instrumental: true`.
   - Prompt-only request: return style-focused prompt variants unless the user asks for a fuller natural-language prompt.
   - Asset-plan request: return prompts plus generation, selection, editing, and export notes.
   - API/provider request: load provider notes, confirm provider-specific fields, then produce request payloads or implementation steps.
5. Write style strings or prompts that describe musical behavior, arrangement, mix priorities, game readability, and exclusions.
6. Include negative direction that prevents common drift: no vocals, no lyrics, no spoken words, no modern drums, no cinematic trailer boom, no harsh lead melody, no watermark, no abrupt ending, no tempo ramp, no busy mix.
7. For batches, create one shared audio identity anchor first, then adapt per-asset prompts from it.

## Suno Instrumental Style Mode

Use this as the default for game music and loops. Suno instrumental generation commonly uses `style` as the primary creative input, with no lyrics field and no long narrative prompt.

For instrumental output:

- Set or state `instrumental: true`.
- Return `style:` as the main generation text.
- Keep each `style` compact and comma-separated.
- Put genre, mood, instrumentation, tempo/energy, arrangement, loop behavior, and exclusions in the style string.
- Do not include lyrics, verse/chorus structure, singing direction, or vocal performance unless explicitly requested.
- Prefer "no vocals, no lyrics, no spoken words" inside the style string when instrumental output is required.

Example style shape:

```text
style: dark fantasy exploration loop, instrumental, 92 bpm, low frame drums, bowed strings, distant choir pads without vocals, sparse hammered dulcimer, tense but restrained, seamless 8-bar loop, game-ready mix, no lyrics, no spoken words, no final cadence
```

## Prompt Framework

Every generated style or prompt should cover:

- Asset type and game function
- Mood and narrative role
- Genre or reference palette without naming living artists
- Tempo or energy range
- Meter or rhythmic feel when relevant
- Instrumentation and sound sources
- Arrangement shape
- Looping or ending behavior
- Mix priorities
- Dynamic range and frequency-space guidance
- Gameplay readability
- Exclusions

Prefer concrete audio vocabulary over hype terms. Use phrases like "muted low taiko pulse", "distant bowed metal texture", "soft sidechained synth bed", "short unresolved cadence", and "loopable 8-bar structure" instead of generic quality claims.

## Variant Rules

### Production Safe

Most likely to generate a usable asset quickly. Keep it clear, short, and strongly constrained around function, duration, instrumentation, and exclusions.

### Art Director Version

Focus on narrative identity, faction or location cohesion, emotional arc, and how the audio supports the game world. Use coherent prose, not a keyword list.

### Loop/Edit Version

Optimize for game implementation. Specify loop points, stable tempo, restrained transients, no long reverb tails crossing loop boundaries, no vocals unless requested, and an ending that can be trimmed or crossfaded.

### Provider Payload Version

Use only when the user asks for API/provider execution or structured parameters. Read provider notes first, then map the prompt into the provider's current fields. Do not invent endpoint names or model IDs.

## Asset-Type Guidance

### Music Loops

Specify duration target, loopable bar count, stable tempo, no intro buildup unless requested, no final cadence, and a mix that leaves room for SFX and dialogue.

### Ambience Beds

Prioritize texture, sparse motion, and non-melodic identity. Specify seamless looping, low foreground event density, no rhythmic pulse unless requested, and avoid sudden one-shot sounds that repeat obviously.

### Stingers and UI Cues

Use short durations, clear transient identity, fast decay, and no melodic overhang unless the cue is meant to transition into music. Specify emotional function: confirm, deny, warning, unlock, reward, transition, or failure.

### Adaptive Music Layers

Generate layers with shared tempo, key center if needed, compatible instrumentation, and distinct frequency roles. Keep each layer musically useful alone but designed to stack without masking.

### Vocal or Lyrical Tracks

Only include vocals or lyrics when explicitly requested. For game assets, prefer non-lyrical vocals, syllables, chants, or texture unless the brief requires intelligible lyrics.

## Output Requirements

For a normal instrumental request, return these sections in order:

1. Context Assumptions, only if assumptions matter
2. Production Safe
3. Art Director Version
4. Loop/Edit Version
5. Generation Notes

For each instrumental variant, include:

- `style:` compact Suno instrumental style input
- `instrumental: true`
- `Avoid:` concise negative direction
- `Use case:` where it fits in game

For each non-instrumental or full-prompt variant, include:

- `Prompt:` natural-language Suno-ready prompt
- `Avoid:` concise negative direction
- `Use case:` where it fits in game

For batches, output a shared audio identity anchor first, then per-asset prompts.

For provider/API requests, include the provider name, model/version source, request shape, async/polling or webhook expectations, and output download/export steps. If current provider details are unknown, say what must be checked instead of guessing.

## References

Read [Suno provider notes](references/suno-provider-notes.md) when the user asks to use an API, provider, model ID, webhook, polling, generated file download, commercial/license assumptions, or any exact Suno integration detail.
