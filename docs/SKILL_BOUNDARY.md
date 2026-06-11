# Skill / Documentation Boundary

## Why this exists

This document defines what belongs in a skill (`.claude/skills/`) vs what belongs in project documentation (`docs/`). It exists because an earlier version of `ideogram-4-prompt-engineer` embedded project-domain content (game vocabulary, palette, style anchors) directly in the skill — making it bloated, repo-specific, and wrong for use outside this project.

## The boundary

### Skills own: how

- Format and schema rules (JSON structure, bbox format, allowed fields)
- Model-specific constraints (medium, aesthetics defaults, prohibited top-level fields)
- Template geometry (bbox layouts per scene type — these are prompt engineering, not domain knowledge)
- Validation structure (what to check, not what the values should be)
- Word budgets and counting rules
- Process flow (which steps in what order)
- An instruction to read project docs for domain context

### Project docs own: what

- Domain vocabulary and material descriptors
- Color palettes and their rationale
- Style anchors and art direction references
- Prohibited phrasing specific to the project
- Architecture and environment descriptions
- Batch/environment-specific details (e.g. "AEGIS Facility" content)

## The contract

A skill MUST include a "Project context" section (or equivalent) that instructs the agent to read relevant project documentation before generating output. A skill without this section is incomplete.

Project docs MUST NOT duplicate the skill's schema rules or validation logic. If a doc describes a format, it should reference the skill rather than restate it.

## Adding a new skill

1. Identify what is format/model knowledge vs what is project content.
2. Write the skill with format rules, templates, validation, and word budgets.
3. Add a "Project context" section pointing to relevant `docs/` files.
4. Ensure no project-domain content (palette, vocabulary, anchors) is inlined.

## When skills fail

When a skill produces output that is schema-valid but canonically wrong:

1. **Is the missing knowledge format-related?** → Update the skill.
2. **Is the missing knowledge domain-related?** → Update project docs; the skill already references them.
3. **Did the skill fail to read project docs?** → The skill needs a stronger "Project context" directive.
4. **Do project docs not exist for this domain?** → Create them. The skill did its job by flagging the gap.

## Precedent

`ideogram-4-prompt-engineer` was refactored from an all-in-one skill (schema + domain content) to a format-only skill that references project docs. This boundary document is the direct result of that refactor.
