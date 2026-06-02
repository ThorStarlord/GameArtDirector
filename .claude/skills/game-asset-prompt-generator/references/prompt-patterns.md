# Prompt Patterns

## Output format

Start with a short context line only when inferred assumptions materially improve the result.

Then return these sections in order:

### Production Safe
[prompt]

### Enhanced Detail
[prompt]

### Art Director Version
[prompt]

### Asset Pipeline Version
[prompt]

Do not add extra variants unless the user asks.

## Construction pattern

Each prompt should naturally cover:
- subject
- function in game
- visual style
- shape language
- materials
- color palette
- lighting
- camera or view
- composition
- background rules
- game readability
- technical requirements

Each prompt should read like a concise art-direction brief in natural language, not a keyword pile.

## Example pattern

For `poison potion icon`, an Asset Pipeline Version should resemble this structure:

Create a 2D fantasy game inventory icon showing a small glass poison potion with a rounded body and narrow neck, filled with luminous green liquid and subtle suspended particles. Use a strong, readable silhouette with clean shape separation between glass, liquid, cork, and strap details. Center the potion with generous padding, controlled highlights, and high contrast for readability at 64x64 pixels. Transparent background. No text, no border, no extra objects. Designed for clean extraction and integration into a professional RPG inventory UI.