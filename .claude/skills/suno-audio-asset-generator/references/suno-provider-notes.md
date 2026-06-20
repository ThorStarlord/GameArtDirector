# Suno Provider Notes

Use this reference before writing provider-specific code, payloads, or operational instructions.

## Provider Reality Check

Suno model access is commonly exposed through third-party providers and wrappers. Before implementation, verify the current provider documentation instead of relying on memory. Confirm:

- Whether the provider is official or third-party.
- Current model IDs and supported versions.
- Authentication method and required environment variable names.
- Generation endpoint, request fields, and response shape.
- Whether generation is synchronous or asynchronous.
- Polling endpoint, webhook format, task states, and failure states.
- Output URLs, expiration behavior, and supported formats.
- Rate limits, credit cost, commercial-use terms, watermark policy, and content restrictions.

Do not claim an official Suno API unless the provider's current documentation explicitly says so.

## Common Request Fields To Look For

Map the skill's style or prompt into provider fields such as:

- `style`: primary creative input for Suno instrumental music generation when the provider supports style-only instrumental mode
- `prompt`: main natural-language music/audio direction only when the provider supports or requires it
- `tags`: compact genre, mood, instrumentation, and energy tags when separate from `style`
- `title`: short asset name
- `instrumental`: true unless lyrics or vocals are requested
- `lyrics`: only when the user supplies or requests lyrics
- `model` or `version`: provider-specific model identifier
- `duration`, `continue_at`, `callback_url`, or `webhook_url`: only when supported

Treat these as likely concepts, not guaranteed field names.

For Suno-style instrumental generation, prefer a compact `style` field over a long prose prompt when the provider exposes both. Leave `lyrics` empty or omitted, and set `instrumental` to true if the provider supports that flag.

## Game Audio Delivery Checks

After generation, assess each candidate for:

- Loopability: stable tempo, no obvious intro-only structure, no hard final cadence, and manageable reverb tail.
- Mix space: dialogue and SFX remain readable; avoid excessive sub-bass, harsh highs, or constant full-spectrum density.
- Editability: clean sections, stable bars, limited tempo drift, and obvious trim/crossfade points.
- Identity: matches faction, location, state, or UI action without overpowering gameplay.
- Repetition risk: ambience should not contain frequent distinctive events that become annoying when looped.
- Legal/brand risk: no artist-name imitation, copyrighted melody references, protected character voices, or lyrics copied from existing songs.

## Suggested Post-Processing Workflow

1. Generate 2-4 candidates per prompt.
2. Pick the candidate with the best function fit, not the most impressive standalone track.
3. Trim intro or ending if needed.
4. Create loop with equal-power crossfade or bar-aligned edit.
5. Export implementation formats requested by the project, commonly WAV for source and OGG/MP3/AAC for runtime.
6. Normalize loudness consistently across the asset family.
7. Name files with game state, location/faction, intensity, version, and loop/stinger marker.

## Payload Safety

When writing code:

- Keep API keys in environment variables.
- Do not commit generated secrets, raw tokens, or private output URLs.
- Save provider responses with task IDs and timestamps for traceability.
- Handle queued, processing, complete, failed, expired, and rate-limited states explicitly.
- Download generated files only after checking content type and HTTP status.
