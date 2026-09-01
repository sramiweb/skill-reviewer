# skill-reviewer

Portable skill audit tool for Agent Skills (SKILL.md) — works with OpenCode, Claude Code, Codex, Cursor.

## Installation

### Option 1: Global install via npx skills

```bash
npx skills add sramiweb/skill-reviewer --agent opencode
```

Or for Claude Code:

```bash
npx skills add sramiweb/skill-reviewer --agent claude-code
```

### Option 2: Manual install

Copy the `SKILL.md` file to your project's skills directory:

- **OpenCode (project):** `.opencode/skills/skill-reviewer/SKILL.md`
- **OpenCode (global):** `~/.config/opencode/skills/skill-reviewer/SKILL.md`
- **Claude Code:** `.claude/skills/skill-reviewer/SKILL.md`

## Usage

Once installed, activate by asking:

- "Review this skill" (with a SKILL.md attached or in context)
- "Audit this SKILL.md for compliance"
- "Refactor ce skill pour le rendre conforme"
- "Skill audit on <skill-name>"

## What it checks

| Rule | Severity | Description |
|------|----------|-------------|
| R1 — Scope unique | CRITICAL | Single job only, no bundled capabilities |
| R2 — Pas de doublon | WARNING | No duplicate skills with same triggers + responsibility |
| R3 — Frontmatter minimal | INFO | Only allowed fields in YAML frontmatter |
| R4 — Negative scope | CRITICAL | Description must include "Do NOT use for..." |
| R5 — Conciseness | WARNING | Body under ~200 lines (externalize to `references/` if longer) |
| R6 — Scenarios | CRITICAL | 3 scenarios (happy/edge/stress) with Input/Expected/Actual/Status/Level |

## Output format

The skill returns a structured audit report:

```markdown
## Audit Report: <skill-name>

| Rule | Status | Finding |
|------|--------|---------|
| R1 | PASS | Single job confirmed |
| R4 | CRITICAL | Missing negative scope in description |
...

## Critical Fixes Required
1. Add negative scope to description: "Do NOT use for..."
...
```

## Examples

The `examples/` directory contains:

| Example | Purpose |
|---------|---------|
| `compliant-skill/SKILL.md` | Fully compliant skill structure (all 6 rules PASS) |
| `non-compliant-skill/SKILL.md` | Intentional violations for learning (R3, R4, R5, R6) |
| `skill-creator-reference/SKILL.md` | skill-creator v1.4.0 as reference for creating new skills |

Use `compliant-skill` as a reference for what a correct skill looks like.
Use `non-compliant-skill` to see what violations look like when audited.
Use `skill-creator-reference` to create new skills from scratch.

## Workflow complet

1. **Cré«« un skill** → utilise `skill-creator` (dans `examples/skill-creator-reference/`)
2. **Valide le skill** → utilise `skill-reviewer` (ce repo)
3. **Itre** → corrige les CRITICAL/WARNING signals
4. **Rutilise** → installe globalement pour tous tes projets

## License

MIT
