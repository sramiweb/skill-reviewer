---
name: example-non-compliant-skill
description: Use this skill when the user needs an example of a SKILL.md with common problems. This skill intentionally violates multiple rules for demonstration purposes.
license: MIT
compatibility: opencode, claude-code, codex, cursor
metadata:
  author: S R
  version: "1.0.0"
  category: meta
  extra_field_that_should_not_exist: "this is an INFO violation"
---

# Example Non-Compliant Skill

## Purpose

This skill demonstrates common mistakes that skill-reviewer will flag. It violates R3 (extra frontmatter field), R4 (no negative scope), R5 (too long if we add more content), and R6 (incomplete scenarios).

## When to use

Use this skill when learning what NOT to do.

## Workflow

1. Read this file
2. Run skill-reviewer on it
3. See what gets flagged

## Rules

Don't copy this structure.

## Examples

### Happy path

- **Input:** "Show me a bad skill example"
- **Expected behavior:** User sees what violations look like
- **Actual result:** (intentionally left vague — this is an R6 violation)
- **Status:** PASS
- **Validation level:** L1 static

### Edge case

- **Input:** "Is this skill compliant?"
- **Expected behavior:** skill-reviewer flags multiple CRITICAL issues
- **Actual result:** (missing — R6 violation)
- **Status:** PASS

### Stress case

- **Input:** "Fix all the problems in this skill"
- **Expected behavior:** User rewrites it following skill-creator rules
- **Actual result:** (missing fields — R6 violation)
- **Status:** PASS

## References

- skill-creator v1.4.0 (read this instead!)
- skill-reviewer v1.0.0 (run it on this file to see violations)

## Additional Padding Section

This section exists solely to make the file longer and trigger R5 (conciseness warning) if combined with more content. In a real skill, this material should be externalized to `references/` if it grows beyond ~200 lines total.

More padding to push the line count up. This is exactly the kind of content that should be in a separate `references/long-explanation.md` file instead of bloating the main SKILL.md.

Even more padding. This is exactly the kind of content that should be in a separate `references/long-explanation.md` file instead of bloating the main SKILL.md.

Final padding paragraph. At this point, a real skill would definitely need to split content into `references/`.
