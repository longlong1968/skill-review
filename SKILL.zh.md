---
name: skill-review
description: Use when a user asks to evaluate, review, audit, or improve the quality of a Claude Code skill they have written or are considering, focusing on skill design and execution guidance rather than security review or greenfield creation. NOT for security vetting (use skill-vetter) or for writing new skills from scratch (use skill-creator).
---

# Skill 评审

**启动时声明：** "我正在使用 skill-review skill，按 12 条质量标准评估这个 skill，这样我才能给出有证据支撑的评分和修改建议。"

## 铁律

每一项判断都必须引用所审 skill 的具体证据。

PASS 必须有引文或行号，FAIL 必须指出错在哪、怎么改。

违反规则的字面，就是违反规则的精神。

## 流程

### Phase 1：通读全部

读完整的 skill - SKILL.md、`references/` 下所有文件、`scripts/` 下所有文件。任何一个文件没读完，就不能开始评分。

### Phase 2：分类

- **规则执行型 rule-execution**（约束行为）：标准 4 适用，标准 10 可能 N/A
- **工具流程型 tool/workflow**（引导行为）：标准 4 可能 N/A，标准 10 多半适用

### Phase 3：按 12 条标准打分

详细判据见 `references/standards-checklist.zh.md`。每条标准给一个分级：

| 分级 | 含义 |
|-------|---------|
| PASS | 完全达标，无瑕疵 |
| PARTIAL | 大方向对，但有具体可修复的问题 |
| FAIL | 缺失或方向错误 |
| N/A | 不适用于该 skill 类型 |

### Phase 4：产出报告

下面这段模板必须**逐字复刻** - 它是承重的输出契约，不是可压缩的示例。

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

每一条适用的标准都要给出 **Evidence**（引文 + 行号）。每一条非 PASS 的标准还要附 **Fix**（改前 -> 改后）。对标准 11 和 12，要显式引用 description 中的声明，以及正文中与之匹配或冲突的段落。

### Phase 5：Top 3 优先级

FAIL 优先于 PARTIAL；同级别中，按标准 1、6、12、2、11、5、9，再到其他标准的顺序优先处理。

## 交付前自检

下面这些条件全部为真，才能交付评审：
- [ ] 每一条适用标准都有来自所审 skill 的引文行
- [ ] 每条 FAIL/PARTIAL 都给了具体的改前/改后写法
- [ ] 已把 description 的声明和排除条件与正文做过交叉核对
- [ ] Top 3 优先级已识别并排序
- [ ] 没有跳过任何一条标准

## 危险信号 - 立即停手

如果发现自己在做下面任何一件事，**立刻停下，从 Phase 1 重新开始。不要在已污染的判断路径上打补丁。**

- 给了 PASS 却没引证据
- 给了泛泛建议（"可以再优化一下"）却没给具体改写
- 因为"显而易见"就跳过某条标准
- 只检查核心功能，不检查边界或排除条件
- 默认 description 和正文一致，却没做交叉核对
- 评审写得比所审的 skill 还长
- 按主题给分而不是按手艺给分（写无聊主题的 skill 也可以是优秀 skill）

## 自我开脱借口表

下面是评审者最常见的自我合理化借口。一旦冒出这些念头，把它当成危险信号，不是捷径。

| 借口 | 真相 |
|---|---|
| "我看了 SKILL.md 就够了，不用再看 references/。" | 铁律要求每个分数都有证据。不读 references/，你根本不知道在按什么标准打分。 |
| "我有大致印象，给个 PASS 就行。" | PASS 也必须有引文或行号。没引证，就不能 PASS - 哪怕这个 skill 明显是好的。 |
| "'大体对齐'就是'达到标准'。" | 不是。PARTIAL 就是给这种差距留的位置。强行塞成 PASS 会掩盖用户真正需要修的地方。 |
| "Description 里已经写了 NOT for X，正文不用再核对了。" | 标准 11 要求跨整份 skill 做一致性检查。如果正文削弱、忽略或绕开了这个排除条件，这个 skill 就是内部不一致。 |
| "Common Mistakes 已经管反合理化了。" | 两层不同：Common Mistakes 防的是输出层面的坏答案；本表防的是评审者自己的偷懒借口。两者都要查。 |
| "这条标准明显没问题，我可以省掉不写。" | 报告契约要求覆盖每一条适用的标准。省掉会让用户失去据此行动的能力。 |

## 常见错误

**绝不**给模糊反馈。每一条批评都必须含具体的改法。

❌ "Use when reviewing Claude Code skills."
✅ "`skill-review` 的 description 应该一次交代四要素，而且不能泄露流程：`Use when a user asks to evaluate, review, audit, or improve the quality of a Claude Code skill they have written or are considering, focusing on skill design and execution guidance rather than security review or greenfield creation. NOT for security vetting (use skill-vetter) or for writing new skills from scratch (use skill-creator).`"

❌ "Use when reviewing Claude Code skills by reading everything, classifying the skill, and scoring it against 12 standards."
✅ "把 description 保持在契约层。通读文件、分类 skill、按 12 条标准打分，这些都属于正文流程，不属于 description。"

❌ "Description 说明了核心功能，所以正文只要给报告模板就够了。"
✅ "如果 description 承诺了 evaluation、audit 和 improvement guidance，正文就必须把三者都操作化：要有证据支撑的评分、具体的改前/改后重写，以及修改优先级。"

❌ "Description 已经写了 NOT for security vetting，正文不必再重复这个区别。"
✅ "把排除条件和正文、Integration 一起交叉核对，这样评审者才能在决策阶段稳定地区分 `skill-review` 和 `skill-vetter`。"

## 关联

**被谁调用：** 写完或正在迭代某个 skill 的用户
**搭配：** `skill-creator`（创建 -> 评审 -> 修订循环）
**区别于：** `skill-vetter`（关注安全，不关注质量）
