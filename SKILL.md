---
name: skill-review
description: Use when a user asks to evaluate, review, audit, or improve the quality of a Claude Code skill they have written or are considering, focusing on skill design and execution guidance rather than security review or greenfield creation. NOT for security vetting (use skill-vetter) or for writing new skills from scratch (use skill-creator).
---

# Skill Review

**Announce at start:** "I'm using the skill-review skill to evaluate this skill against the 12 quality standards so I can give you evidence-backed scores and fixes."

## The Iron Law

EVERY JUDGMENT MUST CITE SPECIFIC EVIDENCE FROM THE SKILL BEING REVIEWED.

No standard gets a passing score without a quote or line reference. No failing score without showing exactly what's wrong and how to fix it.

Violating the letter of the rules is violating the spirit of the rules.

## The Process

### Phase 1: Read Everything

Read the **complete** skill - SKILL.md, all `references/`, all `scripts/`. Do not evaluate until every file is read.

### Phase 2: Classify

- **Rule-execution** (blocks behavior): standard 4 applies, 10 may not
- **Tool/workflow** (guides behavior): standard 4 may not apply, 10 likely applies

### Phase 3: Evaluate Against 12 Standards

Read `references/standards-checklist.md` for detailed criteria. For each standard, assign:

| Score | Meaning |
|-------|---------|
| PASS | Fully meets the standard with no issues |
| PARTIAL | Meets the intent but has specific fixable issues |
| FAIL | Missing or fundamentally wrong |
| N/A | Does not apply to this skill type |

### Phase 4: Produce the Report

Reproduce this template verbatim - it is a load-bearing output contract, not an example to compress.

```
SKILL REVIEW REPORT
══════════════════════════════════════════
Skill: [name]
Type: [Rule-execution / Tool-workflow]
Body line count: [number] (target: <500)
──────────────────────────────────────────
STANDARD                          SCORE
──────────────────────────────────────────
1. Description four elements      [PASS/PARTIAL/FAIL]
2. Cognitive structure            [PASS/PARTIAL/FAIL]
3. Flowchart usage                [PASS/PARTIAL/FAIL/N/A]
4. Iron Law + anti-rationalization[PASS/PARTIAL/FAIL/N/A]
5. Body conciseness               [PASS/PARTIAL/FAIL]
6. Negative constraints           [PASS/PARTIAL/FAIL]
7. Skill interconnection          [PASS/PARTIAL/FAIL]
8. Activation announcement        [PASS/PARTIAL/FAIL/N/A]
9. Embedded verification          [PASS/PARTIAL/FAIL]
10. Example quality               [PASS/PARTIAL/FAIL/N/A]
11. Internal consistency          [PASS/PARTIAL/FAIL]
12. Operational completeness      [PASS/PARTIAL/FAIL]
──────────────────────────────────────────
OVERALL: [X/12 applicable standards passing]
══════════════════════════════════════════
```

For every applicable standard, provide: **Evidence** (quote/line ref). For each non-PASS standard, additionally provide: **Fix** (before/after rewrite). For standards 11 and 12, explicitly cite the description claim and the matching or conflicting body passage.

### Phase 5: Top 3 Priorities

FAIL before PARTIAL. Within the same level, prioritize standards 1, 6, 12, 2, 11, 5, 9, then the rest.

## Self-Check Before Delivering

Do not deliver the review until all of these are true:
- [ ] Every applicable standard has a quoted evidence line from the skill
- [ ] Every FAIL/PARTIAL has a concrete before/after fix
- [ ] Description claims and exclusions were cross-checked against the body
- [ ] Top 3 priorities are identified and ordered
- [ ] No standard was skipped

## Red Flags - STOP

If you catch yourself doing any of these, **stop and start over from Phase 1. Do not patch the review in place.**

- Giving PASS without quoting evidence from the skill
- Giving generic advice ("could be improved") without a specific rewrite
- Skipping a standard because "it's obvious"
- Checking only the main function while ignoring boundary or exclusion claims
- Assuming the description and body are aligned without cross-checking them
- Writing a review longer than the skill itself
- Rating based on topic rather than craft (a skill about a boring topic can still be excellent)

## Rationalization Table

Reviewer self-justifications that produce bad reviews. If any of these thoughts surface, treat them as a red flag, not a shortcut.

| Excuse | Reality |
|---|---|
| "I read SKILL.md, that's enough - I don't need the references/." | The Iron Law requires evidence for every score. Without reading references/, you do not know the actual criteria you are scoring against. |
| "I have a general impression, that's good enough for PASS." | PASS still requires a quote or line ref. No citation, no PASS - even when the skill is obviously good. |
| "'Roughly aligned' is the same as 'meets the standard'." | It is not. PARTIAL exists for that gap. Forcing it into PASS hides the exact issue the user needs to fix. |
| "The description says NOT for X, so I don't need to verify that the body reinforces it." | Standard 11 requires cross-checking claims across the whole skill. If the body weakens or ignores an exclusion, the skill is internally inconsistent. |
| "Common Mistakes already covers anti-rationalization." | Different layers: Common Mistakes guards output quality; this table guards the reviewer's own shortcuts. Both must be checked. |
| "This standard is obviously fine, I can skip writing it up." | The report contract requires every applicable standard. Skipping erodes the user's ability to act on the review. |

## Common Mistakes

**NEVER** give vague feedback. Every critique must include a concrete fix.

❌ "Use when reviewing Claude Code skills."
✅ "`skill-review` description should cover all four elements without leaking workflow: `Use when a user asks to evaluate, review, audit, or improve the quality of a Claude Code skill they have written or are considering, focusing on skill design and execution guidance rather than security review or greenfield creation. NOT for security vetting (use skill-vetter) or for writing new skills from scratch (use skill-creator).`"

❌ "Use when reviewing Claude Code skills by reading everything, classifying the skill, and scoring it against 12 standards."
✅ "Keep the description at the contract level. Reading files, classifying the skill, and scoring against 12 standards belong in the body, not the description."

❌ "The description covers the core function, so the body only needs the report template."
✅ "If the description promises evaluation, audit, and improvement guidance, the body must operationalize all of them: evidence-backed scores, concrete before/after rewrites, and a priority order for fixes."

❌ "The description says NOT for security vetting, but the body doesn't need to repeat that distinction."
✅ "Cross-check exclusion claims against the body and Integration section so the reviewer can reliably distinguish `skill-review` from `skill-vetter` at decision time."

## Integration

**Called by:** Users who have written or are iterating on a skill
**Pairs with:** `skill-creator` (create -> review -> revise cycle)
**Distinct from:** `skill-vetter` (security focus, not quality focus)
