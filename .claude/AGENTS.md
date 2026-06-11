# GameArtDirector — AGENTS.md

## Available agents

| Agent | Skills | When to use |
|---|---|---|
| `task` (default) | All skills, direct work | Default for all implementation, editing, and generation work |
| `explore` (read-only) | Codebase discovery | Mapping unknown parts of the repo, collecting context before generating |

## Skill-to-agent mapping

### game-asset-prompt-generator

- **Agent**: `task`
- **Trigger**: User requests asset prompts for sprites, icons, enemies, props, buildings, environments, UI, or VFX
- **Pre-step**: Read project docs for art direction context
- **Output**: 4 prompt variants per asset request
- **References**: `.claude/skills/game-asset-prompt-generator/`

### ideogram-4-prompt-engineer

- **Agent**: `task`
- **Trigger**: User wants Ideogram 4 JSON captions, `import_json` output, or regional bbox prompting
- **Pre-step**: Read `docs/ideogram-4-caption-spec.md` and project art direction for domain vocabulary
- **Output**: Valid Ideogram 4 JSON (no fences, no explanations)
- **References**: `.claude/skills/ideogram-4-prompt-engineer/`

### Planned agents (not yet created)

| Skill | Agent type | Notes |
|---|---|---|
| `game-art-bible` | `task` | Art direction reference generator |
| `game-character-designer` | `task` | Character concept prompt engineering |
| `game-ui-designer` | `task` | UI element prompt engineering |
| `game-vfx-designer` | `task` | Visual effects prompt engineering |

## Agent dispatch rules

1. **Match skill to agent** — Use the table above. If no skill matches, use `task`.
2. **Read project docs first** — Every skill invocation MUST start with reading relevant `docs/` and skill `references/`. The skill file itself references what to read.
3. **Skill over inline knowledge** — If a skill exists for the task, invoke it. Do not rely on training data or general knowledge.
4. **Parallel when independent** — Multiple asset requests for different categories can be dispatched in parallel via `task` subagents.

## Skill / documentation contract

Skills and project docs have a hard boundary (see `docs/SKILL_BOUNDARY.md`):

- Skills = format rules, schema constraints, validation, geometry templates, word budgets
- Project docs = domain vocabulary, palettes, style anchors, prohibited phrasing
- Skills reference docs; they do not inline domain content
- When a skill produces schema-valid but canonically wrong output, the fix is in the project docs or the skill's "read docs" directive — not in embedding domain content into the skill
