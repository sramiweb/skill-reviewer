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

See `examples/` directory for:
- `compliant-skill/SKILL.md` — a fully compliant skill
- `non-compliant-skill/SKILL.md` — a skill with common issues

## License

MIT
