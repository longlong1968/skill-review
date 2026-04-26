# 12 条标准清单

对每一条标准，逐项打勾。所有子项都过了，这条标准才能 PASS。

---

## 标准 1：Description 四要素

在 skill 的 frontmatter `description` 字段里检查以下几项：

- [ ] 以 "Use when..." 起头（第三人称）
- [ ] 包含触发条件（场景、症状、时机）
- [ ] 包含这个 skill 面向用户的功能或目的
- [ ] 包含 skill 的边界或作用范围
- [ ] 包含排除条件（显式 "NOT when" 或同等精度的排除表达）
- [ ] 完全不提流程步骤、阶段、工作流细节或实现机制
- [ ] 字符数 <= 500（整个 frontmatter <= 1024）

**测试方法：** 只读 description。你能看出什么时候该用、这个 skill 是干什么的、边界在哪里、排除了什么吗？可以。你还能顺手推出它具体会怎么执行吗？能 -> FAIL。

**常见失效模式：** description 写了触发和结果，但漏掉边界/排除；或者偷偷塞入流程细节（"runs four phases"、"dispatches subagents"、"checks every file before scoring"）。

---

## 标准 2：认知结构

- [ ] Skill 类型识别正确（rule-execution vs tool/workflow）
- [ ] 模板与类型匹配：
  - Rule-execution：Iron Law -> Process/Gate -> Red Flags -> Rationalization Table
  - Tool/workflow：Core Principle -> When to Use/NOT -> Decision Tree -> Quick Reference -> Common Mistakes
- [ ] 段落顺序符合 Claude 的思考顺序（先决策，再执行）
- [ ] 没有段落把决策信息和执行细节混在一起
- [ ] 第一段就陈述核心原则或铁律

**测试方法：** 按顺序列出所有段落标题。每个标题是否都在回答 Claude 下一个自然会问的问题？

---

## 标准 3：流程图用法

- [ ] 流程图只出现在真正的决策点 —— Claude 可能选错的地方
- [ ] 流程图使用 Graphviz dot 语法（不是 Mermaid）
- [ ] 线性步骤用编号列表（不用流程图）
- [ ] 查阅性信息用表格（不用流程图）
- [ ] 带环/循环的流程图必须有理由（循环在文字中难以表达）

**N/A 条件：** Skill 没有需要流程图的决策点，且已正确用列表/表格代替。

---

## 标准 4：Iron Law + 反合理化（仅 Rule-Execution 类型适用）

- [ ] 有视觉上醒目的 Iron Law（大写、加粗或方框）
- [ ] 包含 "Violating the letter of the rules is violating the spirit of the rules"（或等效表述）
- [ ] 有 Rationalization Table，含"借口 -> 真相"配对
- [ ] Rationalization Table 的条目反映 Claude 的真实行为（不是臆测）
- [ ] 有 Red Flags / STOP 段落
- [ ] Red Flags 段落以显式的 "stop and start over" 指令结尾

**加分项：** 对最关键的禁令使用 `<HARD-GATE>` XML 标签。

**N/A 条件：** Skill 是 tool/workflow 型，不是 rule-execution 型。

---

## 标准 5：正文精简

- [ ] SKILL.md 正文 <= 500 行（高频 skill <= 200 行）
- [ ] 正文中的代码块不超过 20 行，除非该代码块是评审者首屏必须看到的承重输出契约
- [ ] 正文中不出现 API 参数列表或详细参考资料
- [ ] 执行细节放在 `references/` 或 `scripts/`
- [ ] 正文给出清晰指针，指向已迁出的内容（"Read references/X.md"、"Run scripts/Y --help"）

**测试方法：** 数行数。超过 500 时，判断超出部分是执行细节还是决策信息。

---

## 标准 6：负向约束

- [ ] 有专门段落说明**不要做什么**（Common Mistakes / What to Avoid / When NOT to Use）
- [ ] 使用 ❌/✅ 对比格式（可快速扫读，不埋在段落里）
- [ ] 最关键的禁令用 **NEVER** 或加粗
- [ ] 禁止的事项是 Claude 真实的默认行为（没有这条约束就会发生的事）

**测试方法：** 假设把负向约束段落删掉，Claude 的默认行为会不会产生被禁止的结果？会 -> 这些约束是承重的，必须保留。

---

## 标准 7：Skill 互联

- [ ] 有 Integration 段落
- [ ] 指明 "Called by"（谁会触发这个 skill）
- [ ] 指明 "Pairs with" 或 "Calls"（依赖项和下游 skill）
- [ ] 使用一致、可解析的引用格式（skill 名或 `namespace:skill-name`），而不是文件路径
- [ ] 被引用的 skill 也反向引用回来（如果可行就检查）

**N/A 条件：** Skill 是真正的孤岛，没有任何工作流联系。（少见 - 大多数 skill 都连到别的 skill。）

---

## 标准 8：激活声明

- [ ] 有明确标注的激活声明行，内含面向用户的消息（例如 `**Announce at start:**` 或其本地化等价形式，如 `**启动时声明：**`）
- [ ] 声明同时告诉用户：激活了**哪个** skill、**为什么**激活

**N/A 条件：** Skill 是纯规则/约束类，静默运行（例如 verification-before-completion）。

---

## 标准 9：验证嵌入流程

- [ ] 验证步骤在流程**内部**（不是附录或"建议事项"）
- [ ] 有 "Do not declare success until..." 或等效的强制门禁
- [ ] 有 Claude 必须在结束前完成的自审清单
- [ ] 如果有用户评审门禁，包含 "Wait for the user's response"

**测试方法：** Claude 能不能走完 skill 流程而完全不触及任何验证步骤？能 -> 验证没有真正嵌入。

---

## 标准 10：示例质量

- [ ] 示例完整可运行（不是伪代码）
- [ ] 有好/坏（❌/✅）对比配对
- [ ] 好/坏之间的差异反映真实行为差异（不只是风格）
- [ ] 只给最相关的那个示例（不要 5 种语言变体）
- [ ] 示例取自真实场景

**N/A 条件：** Skill 是纯流程性质，不需要代码或输出示例。

---

## 标准 11：内部一致性

- [ ] Description 的触发、功能、边界、排除声明与正文一致
- [ ] Description 里的排除条件在正文里被强化，而不是被削弱或绕开
- [ ] Integration / Distinct from / When NOT to Use 的语言与声明的边界一致
- [ ] 示例和模板不与它们试图说明的规则相冲突
- [ ] 术语、标准数量、编号在 skill 与 references 之间保持一致

**测试方法：** 把 description、正文、示例、references 里的关键声明列成表。只要其中两处会把 Claude 导向不同结论，这条标准就 FAIL。

---

## 标准 12：操作完备性

- [ ] Description 里的每个实质性声明，都能在正文、references 或 scripts 中找到操作化落点
- [ ] 如果 description 承诺了某种输出，skill 要说明 Claude 如何产出或验证它
- [ ] 如果 description 包含排除条件或边界，正文要告诉 Claude 如何执行这些限制
- [ ] 评审或工作流覆盖了 description 的全部实质性声明，而不只是核心功能
- [ ] 证据要求和完成门禁足以支撑 description 承诺的功能

**测试方法：** 把 description 里的每个实质性动词/名词短语划出来，逐一映射到具体指令、门禁、示例或 reference。只要有声明找不到操作归宿，就 FAIL。

---

## 修复优先级顺序

当多条标准 FAIL 时，按以下顺序修（影响最大的先修）：

1. **标准 1**（Description 四要素）- 触发/目的/边界错 = skill 会被错误激活
2. **标准 6**（负向约束）- 缺约束 = Claude 反复重演默认坏行为
3. **标准 12**（操作完备性）- 承诺没兑现 = description 夸大了 skill 实际能做的事
4. **标准 2**（结构）- 结构错 = Claude 无法沿认知路径走
5. **标准 11**（内部一致性）- 前后矛盾 = 每一段单看都对，组合起来仍然不可靠
6. **标准 5**（精简）- 太长 = Claude 会跳过或概括内容
7. **标准 9**（验证）- 没验证 = Claude 会提前宣告成功
8. 标准 4、3、7、8、10 - 重要，但影响面较小
