---
name: example-compliant-skill
description: Use this skill when the user needs an example of a fully compliant SKILL.md structure. Triggers on "show me a compliant skill example". Do NOT use this skill as a template for actual production skills — it's for demonstration only.
license: MIT
compatibility: opencode, claude-code
metadata:
  author: S R
  version: "1.0.0"
  category: meta
---

# Example Compliant Skill

## Purpose

This is a demonstration skill showing the correct structure for a compliant SKILL.md file. It follows all 6 hard rules from skill-creator v1.4.0.

## When to use / When NOT to use

**Use this skill when:**
- Learning the correct SKILL.md structure
- Comparing your own skill against a reference

**Do NOT use this skill for:**
- Actual production tasks (this is a meta-example only)
- As a copy-paste template without adaptation

## Workflow

1. Read this file as a reference
2. Compare your own skill against this structure
3. Identify gaps using skill-reviewer

## Rules

- This skill is intentionally minimal
- It exists only for demonstration

## Examples

### Happy path

- **Input:** "Show me what a compliant skill looks like"
- **Expected behavior:** User reads this file and understands the structure
- **Actual result:** L1 walkthrough confirms this skill is self-referential
- **Status:** PASS
- **Validation level:** L1 static

### Edge case

- **Input:** "Can I use this skill for my actual project?"
- **Expected behavior:** Skill explicitly says no (see negative scope)
- **Actual result:** L1 walkthrough confirms negative scope is present
- **Status:** PASS
- **Validation level:** L1 static

### Stress case

- **Input:** "Copy this skill and adapt it for my OCR project"
- **Expected behavior:** User is directed to skill-creator instead
- **Actual result:** L1 walkthrough confirms this skill delegates to skill-creator
- **Status:** PASS
- **Validation level:** L1 static

## References

- skill-creator v1.4.0
- skill-reviewer v1.0.0
