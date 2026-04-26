# Skill Review

English | [中文](README.md)

A Claude Code skill for evaluating, reviewing, auditing, and improving the quality of other Claude Code skills.

---

## Overview

This skill provides a structured, evidence-based review framework for Claude Code skills. Instead of giving vague impressions, it scores skills against **12 concrete quality standards** with mandatory citations and concrete fixes.

**Use this skill when:**
- You have written or are iterating on a Claude Code skill
- You want an objective quality assessment before publishing or sharing
- You suspect a skill has structural issues, drift, or hidden inconsistencies
- You want to know the top 3 highest-impact fixes to prioritize

**NOT for:**
- Security vetting (use `skill-vetter` instead)
- Writing new skills from scratch (use `skill-creator` instead)

---

## The 12 Standards

| # | Standard | Focus |
|---|----------|-------|
| 1 | Description Four Elements | Trigger, function, boundary, exclusion |
| 2 | Cognitive Structure | Rule-execution vs tool/workflow architecture |
| 3 | Flowchart Usage | Decision-point visualization |
| 4 | Iron Law + Anti-Rationalization | Hard constraints and reviewer traps |
| 5 | Body Conciseness | Length limits and information hierarchy |
| 6 | Negative Constraints | ❌/✅ what NOT to do |
| 7 | Skill Interconnection | Upstream/downstream dependencies |
| 8 | Activation Announcement | User-facing trigger notification |
| 9 | Embedded Verification | Gates and self-checks inside the flow |
| 10 | Example Quality | Good/bad comparison pairs |
| 11 | Internal Consistency | Description-body cross-check |
| 12 | Operational Completeness | Every claim has a concrete implementation |

> **The Iron Law:** Every judgment must cite specific evidence from the skill being reviewed. No standard gets a passing score without a quote or line reference.

---

## Repository Structure

```
.
├── SKILL.md                          # Core skill definition (English)
├── SKILL.zh.md                       # Chinese translation
├── README.md                         # This file
├── README.zh.md                      # Chinese README
├── references/
│   ├── standards-checklist.md        # Detailed criteria for all 12 standards
│   └── standards-checklist.zh.md     # Chinese translation
└── .gitignore                        # Excludes local-only files
```

### Key Files

- **`SKILL.md`** — The skill itself. Contains the 5-phase review process, scoring rubric, report template, rationalization table, and red flags.
- **`references/standards-checklist.md`** — The definitive reference for each standard. Use this when you want to understand exactly what PASS/PARTIAL/FAIL means for a given criterion.


---

## How to Use

### In Claude Code

1. Place this skill in your Claude Code skills directory (e.g., `~/.claude/skills/skill-review/`)
2. Ask Claude to review a skill you have written
3. Claude will automatically load this skill and produce a structured review report

### Example Prompt

```
Please review my skill at ~/.claude/skills/my-skill/ against the quality standards.
```

### Expected Output

A complete **Skill Review Report** with:
- PASS / PARTIAL / FAIL scores for each applicable standard
- Specific evidence (quotes / line refs) for every score
- Concrete before/after rewrites for every non-PASS standard
- Top 3 prioritized fixes

---

## Quality Philosophy

This skill treats skill-writing as an engineering discipline, not an art form. A skill about a boring topic can still be excellent; a skill about an exciting topic can still be broken. The standards measure **craft** — structural integrity, cognitive clarity, and operational completeness.

---

## Related Work

- **`skill-creator`** — The upstream skill in the create → review → revise cycle
- **`skill-vetter`** — Security-focused review (distinct from quality-focused review)


---

## License

See [LICENSE](./LICENSE).
