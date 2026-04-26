# 12 Standards Checklist

For each standard, check every item. A standard is PASS only if all items check out.

---

## Standard 1: Description Four Elements

Check these in the skill's frontmatter `description` field:

- [ ] Starts with "Use when..." (third person)
- [ ] Includes the trigger condition (scenario, symptom, timing)
- [ ] Includes the user-facing function or purpose of the skill
- [ ] Includes the skill's boundary or scope
- [ ] Includes exclusion conditions (explicit "NOT when" or an equally precise exclusion)
- [ ] Does NOT mention process steps, phases, workflow details, or implementation mechanics
- [ ] Character count <= 500 (total frontmatter <= 1024)

**How to test:** Read only the description. Can you tell when to use it, what it is for, where its boundary is, and what it excludes? Good. Can you infer specific execution steps? If yes -> FAIL.

**Common failure pattern:** Description includes trigger + outcome but omits boundary/exclusion, or smuggles in workflow details ("runs four phases", "dispatches subagents", "checks every file before scoring").

---

## Standard 2: Cognitive Structure

- [ ] Skill type correctly identified (rule-execution vs tool/workflow)
- [ ] Template matches the type:
  - Rule-execution: Iron Law -> Process/Gate -> Red Flags -> Rationalization Table
  - Tool/workflow: Core Principle -> When to Use/NOT -> Decision Tree -> Quick Reference -> Common Mistakes
- [ ] Section order matches Claude's thinking sequence (decide first, then execute)
- [ ] No section mixes decision information with execution details
- [ ] First section states the core principle or iron law

**How to test:** List all section headings in order. Does each heading answer the next logical question Claude would ask?

---

## Standard 3: Flowchart Usage

- [ ] Flowcharts only appear at genuine decision points where Claude could choose wrong
- [ ] Flowcharts use Graphviz dot syntax (NOT Mermaid)
- [ ] Linear steps use numbered lists (not flowcharts)
- [ ] Reference lookups use tables (not flowcharts)
- [ ] Any flowchart with a loop/cycle is justified (loops are hard to express in text)

**N/A when:** Skill has no decision points requiring flowcharts, and correctly uses lists/tables instead.

---

## Standard 4: Iron Law + Anti-Rationalization (Rule-Execution Type Only)

- [ ] Has a visually prominent Iron Law (uppercase, bold, or boxed)
- [ ] Includes "Violating the letter of the rules is violating the spirit of the rules" (or equivalent)
- [ ] Has Rationalization Table with excuse -> reality pairs
- [ ] Rationalization entries reflect real Claude behaviors (not hypothetical)
- [ ] Has Red Flags / STOP section
- [ ] Red Flags section ends with explicit "stop and start over" instruction

**Bonus:** Uses `<HARD-GATE>` XML tags for the most critical prohibitions.

**N/A when:** Skill is tool/workflow type, not rule-execution.

---

## Standard 5: Body Conciseness

- [ ] SKILL.md body <= 500 lines (<= 200 for high-frequency skills)
- [ ] No code blocks longer than 20 lines in the body unless the block is a load-bearing output contract that the reviewer must see at first pass
- [ ] No API parameter lists or detailed reference material in the body
- [ ] Execution details live in `references/` or `scripts/`
- [ ] Body contains clear pointers to moved content ("Read references/X.md", "Run scripts/Y --help")

**How to test:** Count lines. If over 500, identify what's execution-detail vs decision-info.

---

## Standard 6: Negative Constraints

- [ ] Has a dedicated section for what NOT to do (Common Mistakes / What to Avoid / When NOT to Use)
- [ ] Uses ❌/✅ contrast format (scannable, not buried in paragraphs)
- [ ] Most critical prohibitions use **NEVER** or bold formatting
- [ ] Prohibited items are Claude's actual default behaviors (things that WILL happen without the constraint)

**How to test:** Remove the negative constraint section. Would Claude's default behavior produce the prohibited outcomes? If yes -> constraints are load-bearing and must stay.

---

## Standard 7: Skill Interconnection

- [ ] Has an Integration section
- [ ] Specifies "Called by" (who triggers this skill)
- [ ] Specifies "Pairs with" or "Calls" (dependencies and downstream skills)
- [ ] Uses a consistent, resolvable reference format (skill name or `namespace:skill-name`) rather than file paths
- [ ] Referenced skills have reciprocal references back (check if feasible)

**N/A when:** Skill is truly standalone with no workflow connections. (Rare - most skills connect to something.)

---

## Standard 8: Activation Announcement

- [ ] Has a clearly labeled activation announcement line with a user-facing message (e.g. `**Announce at start:**` or a localized equivalent)
- [ ] Announcement tells the user WHAT skill activated and WHY

**N/A when:** Skill is a pure rule/constraint that operates silently (e.g., verification-before-completion).

---

## Standard 9: Embedded Verification

- [ ] Verification steps are INSIDE the process flow (not in an appendix or "recommendations")
- [ ] Has "Do not declare success until..." or equivalent mandatory gate
- [ ] Has a self-review checklist Claude must complete before finishing
- [ ] If there's a user review gate, it includes "Wait for the user's response"

**How to test:** Can Claude complete the skill's process without hitting any verification step? If yes -> verification is not truly embedded.

---

## Standard 10: Example Quality

- [ ] Examples are complete and runnable (not pseudocode)
- [ ] Has Good/Bad (❌/✅) comparison pairs
- [ ] Differences between Good/Bad reflect real behavioral differences (not just style)
- [ ] Only the most relevant example is shown (not 5 language variants)
- [ ] Examples come from realistic scenarios

**N/A when:** Skill is purely procedural with no code or output examples needed.

---

## Standard 11: Internal Consistency

- [ ] Description trigger, function, boundary, and exclusion claims agree with the body
- [ ] Exclusion conditions in the description are reinforced, not undermined, by the body
- [ ] Integration / Distinct from / When NOT to Use language matches the stated boundary
- [ ] Examples and templates do not contradict the rules they are supposed to illustrate
- [ ] Terminology, counts, and standard numbering stay consistent across the skill and its references

**How to test:** Make a claim table from description, body, examples, and references. If two parts of the skill would lead Claude to different conclusions, this standard FAILs.

---

## Standard 12: Operational Completeness

- [ ] Every substantive claim in the description is operationalized somewhere in the body, references, or scripts
- [ ] If the description promises an output, the skill explains how Claude should produce or validate it
- [ ] If the description includes exclusions or boundaries, the body tells Claude how to enforce them
- [ ] The review or workflow covers all substantive description claims, not just the main function
- [ ] Evidence requirements and completion gates are sufficient to support the promised function

**How to test:** Underline each substantive verb/noun phrase in the description and map it to a concrete instruction, gate, example, or reference. If a claim has no operational home, FAIL.

---

## Priority Order for Fixes

When multiple standards fail, fix in this order (highest impact first):

1. **Standard 1** (Description four elements) - wrong trigger/purpose/boundary = skill activates incorrectly
2. **Standard 6** (Negative constraints) - missing constraints = Claude repeats default bad behaviors
3. **Standard 12** (Operational completeness) - unfulfilled promises = description overclaims what the skill can do
4. **Standard 2** (Structure) - wrong structure = Claude can't follow the cognitive path
5. **Standard 11** (Internal consistency) - contradictions make the skill unreliable even when each part looks reasonable alone
6. **Standard 5** (Conciseness) - too long = Claude skips or summarizes content
7. **Standard 9** (Verification) - no verification = Claude declares success prematurely
8. Standards 4, 3, 7, 8, 10 - important but lower blast radius
