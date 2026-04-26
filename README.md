# Skill Review（Skill 评审）

[English](README.en.md) | 中文

用于评估、审查、审计和改进其他 Claude Code Skill 质量的 Claude Code Skill。

---

## 概述

本 Skill 提供了一套结构化、基于证据的评审框架，专门用于评审 Claude Code Skill。它不会给出模糊的印象分，而是依据 **12 条具体的质量标准** 逐条打分，每条评分都必须附带引用和具体修复方案。

**适用场景：**
- 你写完了一个 Claude Code Skill，想要一个客观的质量评估
- 在发布或分享前想确认 Skill 是否存在结构性问题
- 怀疑某个 Skill 存在目标漂移或隐藏的不一致
- 想知道最值得优先修复的前 3 个问题

**不适用：**
- 安全审查（请使用 `skill-vetter`）
- 从零开始编写新 Skill（请使用 `skill-creator`）

---

## 12 条质量标准

| # | 标准 | 关注点 |
|---|------|--------|
| 1 | 描述四要素 | 触发条件、功能、边界、排除项 |
| 2 | 认知结构 | 规则执行型 vs 工具/工作流型架构 |
| 3 | 流程图使用 | 仅在真正的决策点使用 |
| 4 | 铁律 + 反合理化 | 硬性约束和评审者陷阱 |
| 5 | 正文简洁性 | 长度限制和信息层级 |
| 6 | 负向约束 | ❌/✅ 明确禁止的行为 |
| 7 | Skill 互联 | 上游/下游依赖关系 |
| 8 | 激活宣告 | 面向用户的触发通知 |
| 9 | 嵌入式验证 | 流程中的门禁和自检 |
| 10 | 示例质量 | 好/坏对比示例 |
| 11 | 内部一致性 | 描述与正文交叉验证 |
| 12 | 操作完整性 | 每个承诺都有具体实现 |

> **铁律：** 每条评判都必须引用被评审 Skill 中的具体证据。没有引用或行号引用，任何标准都不得给 PASS。

---

## 仓库结构

```
.
├── SKILL.md                          # 核心 Skill 定义（英文）
├── SKILL.zh.md                       # 中文翻译
├── README.md                         # 英文说明
├── README.zh.md                      # 本文件
├── references/
│   ├── standards-checklist.md        # 12 条标准的详细评审清单
│   └── standards-checklist.zh.md     # 中文翻译
└── .gitignore                        # 排除仅本地保留的文件
```

### 关键文件

- **`SKILL.md`** — Skill 本体。包含 5 阶段评审流程、评分规则、报告模板、合理化表和红旗清单。
- **`references/standards-checklist.md`** — 每条标准的权威参考。当你想精确理解某条标准的 PASS/PARTIAL/FAIL 含义时查阅此文件。
- **`references/skill-foundry-modification-plan.zh.md`** *（仅本地，被 `.gitignore` 排除）* — 更高级别的 Skill 重构工作流（"Skill 炼金坊"）蓝图。`skill-review` 在该工作流中担任结构评审模块。由于尚不完善，此文件仅在本地保留。

---

## 使用方法

### 在 Claude Code 中

1. 将本 Skill 放入你的 Claude Code Skill 目录（例如 `~/.claude/skills/skill-review/`）
2. 让 Claude 评审你写的 Skill
3. Claude 会自动加载本 Skill 并输出结构化评审报告

### 示例指令

```
请评审我位于 ~/.claude/skills/my-skill/ 的 Skill，按质量标准打分。
```

### 预期输出

一份完整的 **Skill 评审报告**，包含：
- 每条适用标准的 PASS / PARTIAL / FAIL 评分
- 每条评分的具体证据（引用 / 行号）
- 每条非 PASS 标准的具体前后对比改写方案
- 前 3 个优先修复项

---

## 质量理念

本 Skill 将 Skill 写作视为工程学科，而非艺术创作。一个关于无聊话题的 Skill 可以是优秀的；一个关于激动人心话题的 Skill 也可能是残缺的。这些标准衡量的是**工艺**——结构完整性、认知清晰度和操作完整性。

---

## 相关项目

- **`skill-creator`** — 创建 → 评审 → 修订 循环中的上游 Skill
- **`skill-vetter`** — 安全导向的评审（与质量导向的评审不同）


---

## 许可证

见 [LICENSE](./LICENSE)。
