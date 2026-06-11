# GameArtDirector — CLAUDE.md

## Project conventions

### Skill / documentation boundary

- Skills (`.claude/skills/`) define **how** to produce output — format rules, schema constraints, validation patterns.
- Project docs (`docs/`) define **what** to produce — domain vocabulary, palette, style anchors, prohibited phrasing.
- A skill's first step MUST be: read the relevant project documentation for domain context.
- Skills MUST NOT embed project-domain content. If a skill needs domain vocabulary, reference the docs — do not inline it.

### Adding a new skill

1. Write the skill as format-agnostic prompt engineering rules first.
2. Add a "Project context" section telling the agent to read relevant docs.
3. Keep bbox templates, schema rules, validation, and word budgets in the skill.
4. Keep palette, vocabulary, style anchors, and prohibited phrasing in `docs/`.

### Handling skill failures

1. Capture the failure as a structured handoff document (what went wrong, root cause chain, what was missing).
2. Use the handoff to update the skill — not to bloat it with domain content.
3. If the handoff reveals missing project documentation, create or update `docs/` files instead.

### Current skills and their project-doc dependencies

| Skill | Reads from |
|---|---|
| `ideogram-4-prompt-engineer` | `docs/ideogram-4-caption-spec.md`, project art direction docs |
| `game-asset-prompt-generator` | `docs/` (various references) |

### Process over blame

Failures in agent output are process failures: missing docs, unclear boundaries, insufficient validation. Fix the process, not the person.
