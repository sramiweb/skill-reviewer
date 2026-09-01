---
name: skill-creator
description: Use this skill when the user wants to create, scaffold, scope, refactor, or extend an Agent Skill (SKILL.md). Triggers on "cré«« un skill", "nouveau skill", "amliore ce skill", "refactor this skill", "restructure this SKILL.md", "create a skill", "scaffold a skill", "extend this skill". Do NOT use this skill to execute an existing skill's task, to troubleshoot application code unrelated to skill design, or to perform the domain task a skill describes (e.g. don't use this to run OCR — use it to design the OCR skill).
license: MIT
compatibility: opencode, claude-code, codex, cursor
metadata:
  author: S R
  version: "1.4.0"
  category: meta
---

# Skill Creator

## Purpose

Governs the full lifecycle of an Agent Skill: scoping a new one, refactoring an existing one, or extending one to cover a related trigger — all under the same rules. Per Hard rule 1's own test, authoring and refactoring a SKILL.md require the same expertise, so this stays one skill rather than splitting into "creator" + "refactorer."

## Hard rules

1. **One skill = one job.** Two conditions, BOTH required, for an internal step to deserve its own skill rather than staying inside the parent:
   - (a) **Independent invocability** — could this step be requested on its own as a standalone task?
   - (b) **Non-trivial guidance** — does doing it well require domain-specific procedure or judgment a general-purpose agent would not already apply correctly without this skill?
   Split only when both hold. Whether a given step (e.g. "send an email") satisfies (b) depends on the facts of that step — templates, compliance, approval, delivery verification make it non-trivial; a fixed one-line message does not. Never decide from the verb alone; ask if the facts aren't given. When either condition is ambiguous, say so rather than asserting a confident split or non-split.

2. **Duplicate detection:** same activation context AND same primary responsibility — never determined by output file format alone. Optional, non-decisive supporting signal: expected input domain/object. If responsibility is genuinely unclear, ask the user rather than inventing a similarity score.

3. Frontmatter requires only `name` and `description`; `license`, `compatibility`, `metadata` are optional. `compatibility` lists platforms this skill's rules are designed to work on **once installed at that platform's own path** (see Step 1.2) — it is not a claim that the file will be auto-discovered without correct placement.

4. `description` states WHEN to trigger AND when NOT to (negative scope). Enforced on this file's own frontmatter too, no exception.

5. Target a concise SKILL.md — prefer under ~200 lines when practical. Move bulk reference material to `references/`. Never split content purely to hit an arbitrary line count.

6. Every skill — including this one — ships 3 scenarios about its own behavior in `## Examples`: happy path, edge case, stress case. Each scenario records:
   - Input
   - Expected behavior
   - Actual result (from the validation level actually performed)
   - Status: PASS or FAIL
   - Validation level: L1 static or L2 runtime

   **Level 1 — static walk-through** is always required: follow the skill's own instructions step by step and record the result actually reached. **Level 2 — runtime execution** is required only when a compatible runtime is available: capture the real output or execution trace. Never label an L1 result as L2. An `Actual` field that just repeats `Expected` is not a valid pass. If Actual diverges from Expected, fix the skill and rerun validation before delivery.

## Workflow

### Step 1 — Platform, then discovery, in this order

1. **Determine target platform** — infer from context/tooling, or ask if ambiguous. Do not default to OpenCode.
2. **Determine that platform's supported skill locations:**
   - OpenCode: `.opencode/skills/`, `~/.config/opencode/skills/`
   - Claude Code / Claude skills: `.claude/skills/`
   - Codex / Cursor: that tool's own documented convention — don't assume it mirrors OpenCode's layout; consult its docs or ask if unknown, and say so if the check can't be completed
   - `.agents/skills/` if that convention applies to the confirmed platform
3. **Discover existing skills** only in the locations relevant to the confirmed platform.
4. **Compare** each hit against Hard rule 2's axes.
5. **Decide:** CREATE (no relevant match) / EXTEND (same job, missing capability — bump `metadata.version`, state what was added) / REFACTOR (same job, existing file needs restructuring — preserve `name`, bump version) / propose SPLIT (the request bundles multiple jobs per Hard rule 1's test).

### Step 2 — Scope interrogation

- What is the single recurring task this skill should own?
- What triggers it? (in every language the user actually works in)
- What should it explicitly NOT do? (mandatory, Hard rule 4)
- Which tools/stack does it need to know about?

If Step 1's comparison or Hard rule 1's test surfaces multiple independent jobs, stop and propose a split before proceeding.

### Step 3 — Frontmatter generation

```yaml
---
name: <kebab-case-name>
description: Use this skill when <trigger condition>. Triggers on <keywords/phrases, every relevant language>. Do NOT use for <negative scope>.
license: MIT
compatibility: <only the targets confirmed relevant — don't copy this file's compatibility list blindly>
metadata:
  author: <user>
  version: "1.0.0"
  category: <one short lowercase word, e.g. meta, coding, ops, data, writing, design — not a closed list>
---
```

### Step 4 — Body structure and resource layer

`## Purpose` → `## When to use / When NOT to use` → `## Workflow` → `## Rules` → `## Examples` (3 scenarios, Hard rule 6) → `## References` (optional). If it needs large reference docs, scripts, or templates, split them out — only `SKILL.md` loads by default:
```
<skill-name>/SKILL.md, references/<topic>.md, scripts/<helper>.sh, templates/<file>.template
```

### Step 5 — Validation checklist

- [ ] Single job per Hard rule 1's two-condition test
- [ ] Negative scope present — checked on this file's own frontmatter
- [ ] No unrelated capabilities bundled in
- [ ] Body concise per Hard rule 5
- [ ] All 3 scenarios include Input, Expected behavior, Actual result, Status, and Validation level — Actual is not a copy of Expected
- [ ] Duplicate check run per Hard rule 2's two axes, not a similarity percentage
- [ ] Output path and discovery locations both match the platform confirmed in Step 1

### Step 6 — Output

Write to the Step 1.2 path only if a write tool is available AND the user authorized file creation. Otherwise, output the full file content and state plainly it has **not** been written to disk — never claim a write without an actual successful result.

Always report: action (CREATED/UPDATED/CONTENT_ONLY), target platform and path, validation level achieved (L1/L2), remaining unverified items. When a write occurred, verify with the target platform's own listing command — don't propose an OpenCode-specific command for a Claude/Codex/Cursor target.

## Anti-patterns rejected

God skills bundling unrelated jobs · vague descriptions ("helps with X") · deciding a split from the verb alone instead of condition (b)'s facts · Actual fields that just restate Expected · claiming a write or a test that didn't happen · discovering skills before the platform is known.

## Examples

**Splitting logic (illustrative, Hard rule 1):**
"Deploy the app, run tests, create the release, then notify the team" → whether "notify the team" stays inside `deploy-release` or becomes its own skill is not decided by the verb "notify" alone. If it's a fixed one-line message, condition (b) fails → stays inside. If it needs templates, recipient/compliance resolution, or delivery verification, condition (b) holds too → treat it as its own skill. Ask which case applies rather than assuming either.

**Required self-test for `skill-creator` (Hard rule 6):**

### Happy path
- **Input:** "Cre un skill Zabbix pour diagnostiquer les proxies."
- **Expected:** platform+locations (Step 1) resolved before scope questions (Step 2); asks only for missing scope info.
- **Actual:** order followed correctly; one question asked about missing negative scope.
- **Status:** PASS · Level: L1 static (no runtime available)

### Edge case
- **Input:** "Amliore cette requte PostgreSQL."
- **Expected:** does NOT activate — not a skill-design request.
- **Actual:** matched no trigger phrase, matched the negative scope ("perform the domain task"); no activation proposed.
- **Status:** PASS · Level: L1 static

### Stress case
- **Input:** "Cre un skill qui analyse Zabbix, dploie Kubernetes, envoie des emails et gnre des factures."
- **Expected:** 3 skills proposed (Zabbix-analysis, Kubernetes-deploy, invoice-generation); "envoie des emails" not resolved from the verb alone.
- **Actual:** 3 skills proposed; email step flagged with one clarifying question instead of an assumed answer.
- **Status:** PASS · Level: L1 static
