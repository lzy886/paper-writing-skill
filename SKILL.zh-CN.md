# SKILL.md 中文对照

> **用途**：本文是 `SKILL.md` 的中文对照，供你查看这个 skill 究竟向上下文注入了什么。
>
> **不改规则**：一切语法、句法、风格约束**仍然指向英文写作**。本文只做说明，不改变任何执行行为。
>
> **保留英文原文的部分**：规则编号（`M1`–`M18`、`S1`–`S31`）、文件名、shell 命令、grep 模式、LaTeX 代码、YAML frontmatter 一律保留原文。
>
> **标注约定**：
> - **【英文专属】** —— 该规则作用于英文特有的语言现象，中文里没有对应物。见到此标记，说明译文里的说法**不能**类推到中文写作。
> - **【译注】** —— 一般性说明。
> - **【注入时机】** —— 这段内容何时进入上下文。

---

## 注入结构总览（先读这个）

这个 skill 不会把 4314 行一次性塞进上下文，而是**分三层渐进注入**。所以"注入了什么"取决于你处在哪个阶段。

| 层 | 内容 | 注入时机 | 体量 |
|---|---|---|---|
| **常驻** | frontmatter 的 `description` | 每次会话开始，进入系统提示的技能列表 | 1 段 |
| **调用时** | `SKILL.md` 正文 | `/paper-writing` 被调用时 | 366 行 |
| **调用时** | `author_profile/` **全部 7 个文件** | SKILL.md 硬性要求 "MUST read ALL" | ~1020 行 |
| **按需** | `writing_checklists/` | 写完某一节后，读对应那一份 | ~173 行 |
| **按需** | `section_rhetorical_moves/` | 写某一节前，读对应那一份 | ~373 行 |
| **按需** | `brainstorming_guide.md` | 阶段 1，走 34 问时 | 180 行 |
| **按需** | `figure_synthesis_guide.md` + `figure_templates/` | 需要非数据图时 | ~890 行 |
| **特定模式** | `red_team_protocol.md` | 每次审查（自动） | 38 行 |
| **特定模式** | `loop_mode.md` | `/loop` 模式 | 56 行 |

【译注】`description` 是最关键的一段——它在**未被调用时**就已经在上下文里，负责决定这个 skill 会不会被触发。所以它也是唯一"常驻"的内容。

---

# 以下为 `SKILL.md` 正文对照

## 这个 skill 如何工作

这个 skill 编码了 [Systems and Networking Lab (SNL)](https://github.com/SNL-UCSB) 的论文写作方法论，来源是对 6 篇论文（8 次投稿）、7600+ 次 Overleaf 编辑、100+ 个 tex 版本、5 轮同行评审的取证式分析。完整分析见 [*The Paper Behind the Paper*](https://sites.cs.ucsb.edu/~arpitgupta/blog/the-paper-behind-the-paper.html)。开箱即用，默认规则经过校准和实战检验。

### 三层结构

1. **流水线（固定）**：五阶段写作工作流。不随用户或论文改变。

2. **声音与编辑规则（提供默认值，可定制）**：句级风格、结构规则、压缩模式、章节检查表。出厂值是 SNL 实验室的规则。学生可以通过编辑 `author_profile/` 下的文件来定制，改什么见 README。

3. **项目上下文（每篇论文一份）**：身份句、目标会议、贡献声明、已锁定决策。存放在论文工作目录的 `project_context.md` 里。

### 这个 skill 如何接入科研流水线

它不孤立工作。它属于一个三技能家族，另外两个技能的产物是写作流程的直接输入。

**来自 [literature-survey-skill](https://github.com/SNL-UCSB/literature-survey-skill)：**

- **缺口分析** → 喂给头脑风暴阶段 1（问题发现）。综述识别出的缺口（缺失的象限、失效的共享假设、未被探索的组合）就是驱动你论文的结构性局限。
- **写作技艺提取**（Pass 3+）→ 喂给架构阶段和分节起草。你从本领域最好的论文里提取的引言解剖、评估架构、设计手法，是你自己论文结构的模板。
- **竞争定位** → 喂给头脑风暴阶段 4。综合分析产出的不变式矩阵和依赖图，精确显示你的论文相对已有工作的位置。

**来自 [data-visualization-skill](https://github.com/SNL-UCSB/data-visualization-skill)：**

- **探索**（`exploration_log.md`）→ 喂给头脑风暴阶段 3（评估设计）。探索迫使你在形成假设之前从多个角度看数据。你发现的意外（没想到的分布、行为不同的子群）塑造了哪些主张站得住、真正的贡献在哪里。
- **头脑风暴**（`braindump.md`）→ 喂给图表计划。每份 braindump 都说明了一张图回答什么问题、你预期看到什么、什么会让你意外。这些就是你的评估必须验证的假设。
- **计划 + 执行**（`plot_context.md`）→ 喂给架构阶段的图表计划。每份 `plot_context` 记录了意图、变量映射、图型理由和设计决策，是论文图表计划的现成条目。
- **分析**（WALTER 叙述）→ 喂给评估动作 4（Takeaway 综合）。WALTER 的 Result（"结论是什么？它接回假设了吗？"）就是那个实验群组的 Takeaway 段的初稿。

三个技能构成闭环：文献综述揭示缺口并教你领域内被录用的论文如何表达；数据可视化迫使你理解证据到底说明了什么、验证了哪些假设；论文写作把两者变成可发表的论证。**如果学生有另外两个技能的产物，Claude 必须载入它们。**

### 这个 skill 被触发时，Claude 必须：

1. 读本文件 `SKILL.md`（已载入）
2. 读 `author_profile/` 下的**全部**文件——这些是编辑规则的唯一事实来源
3. 询问用户在做哪篇论文
4. 在该论文的工作目录查找 `project_context.md`
5. 若找到，读取并作为**约束**对待
6. 若未找到，运行下面的**结构化头脑风暴**流程来创建它
7. 检查兄弟技能的产物——带技艺提取的综述笔记、`exploration_log.md`、`braindump.md`、`plot_context.md`、WALTER 叙述。若存在，作为对应流水线阶段的参考材料载入

【译注】第 2 步是硬性的（"MUST read ALL"）。这意味着**每次调用 skill，`author_profile/` 那 7 个文件、约 1020 行都会进上下文**。这是这个 skill 上下文开销的主要来源。

---

## 结构化头脑风暴 —— 这个 skill 的核心

**学生最大的障碍不是写作，而是他们的想法以未结构化的直觉形式存在。** 他们知道某件事有意思，但说不清是什么、为什么。头脑风暴流程把散乱的思考变成精确的项目上下文，驱动论文的每一节。

### 它如何运作

Claude 必须读 `brainstorming_guide.md`，并带领学生交互式走完它的 6 个阶段：

| 阶段 | 焦点 | 关键产出 |
|---|---|---|
| 1. 问题发现 | 谁在受苦、什么坏了、为什么**结构上**坏 | 开篇的利害关系 + 问题缺口 |
| 2. 贡献结晶 | 核心主张、头条数字、关键抽象的名字 | 身份句 + 贡献列表 |
| 3. 评估设计 | 基线、指标、数据集、实验到主张的映射 | 约束引言能承诺什么的评估计划 |
| 4. 定位与定框 | 会议匹配、竞争定位、造类别 vs 拼类别 | 相关工作的定位句 |
| 5. 架构与约束 | 设计流程、已锁定决策、开放问题 | 设计章节的结构 + 项目边界 |
| 6. 叙事主线 | 故事弧、"必然性"时刻、推文长度的电梯陈述 | 串联全篇的线索 |

### 运行头脑风暴的规则

- **一个阶段一个阶段走。** 不要跳。阶段 1（问题）必须在阶段 2（贡献）之前清楚。
- **"我不知道"是有效答案。** 标记为开放问题继续走。现在发现的空洞改起来便宜，评审阶段发现的很贵。
- **追问含糊的回答。** "更快" → "对谁更快？快多少？什么负载下？" 每个答案都应具体到能写进论文。
- **区分结构性与定量。** "现有工具不够准"是定量的——它驱动更多实验。"现有工具假设平稳性，在突发数据上失效"是结构性的——它驱动新方法。**论文需要结构性缺口。**
- **走完全部阶段后生成 `project_context.md`**，用 `examples/project_context.md` 的模板。真实完整样例见 `examples/netburst_project_context.md`。

【译注】阶段表格与规则本身与语言无关，写英文论文或中文论文都适用。

---

## 声音与编辑规则

Claude 必须读本 skill 目录下的这些文件。它们包含带示例的详细规则。

| 文件 | 控制什么 |
|---|---|
| `author_profile/editorial_principles.md` | 14 条跨论文原则及证据（引言写两遍、具体名词优于含糊词、what→why→so-what 标题、先扩写后压缩等） |
| `author_profile/craft_reference.md` | **怎么写（正向层）。** 句级风格（约 21 词均值、主张先行、主动语态、具体名词优于含糊词、无填充词）、行文技艺，以及三种语域：base/简洁、conceptual/定位、warm/叙事。起草段落时读它。 |
| `author_profile/gate_mechanical.md` | **唯一的机械 grep 门（M1–M18）。** 破折号、对偶句式、强调词、禁用 + 浮夸 + 花哨动词、清嗓子开场、被动语态、冗词、限定词、术语漂移，一个 grep 脚本。每次编辑 tex 都跑；被动语态扫描也在基础门里跑。 |
| `author_profile/gate_semantic.md` | **唯一的读者判断门（S1–S31）。** 先定义后使用、可跟随性、解析可达性、与论点挂钩、词汇与拆解一致性、非重复、严谨性/有据、诚实定位、图表、以及**闭合门**。在红队/loop 环节跑。 |
| `author_profile/compression_patterns.md` | 7 个压缩操作，带前后对照示例和量化基准 |
| `author_profile/rhetorical_moves.md` | 跨节动作序列：引言（6 步）、设计（5 步）、评估（6 步）、相关工作（3 步） |
| `author_profile/intervention_types.md` | 7 种导师干预类型——用它来模拟导师对草稿的反馈 |
| `red_team_protocol.md` | **独立对抗红队**（证据门控）：机械审查之后，由**没有写过这段文字**的审稿者重跑 `gate_mechanical.md` 并施加 `gate_semantic.md`，必须返回 CLEAN 才能放行。 |
| `loop_mode.md` | 可恢复的 `/loop` 审查与修复协议：磁盘账本、一轮一节、全绿自停。 |

### 速查：不可谈判的声音规则

以下摘自上面那些详细文件。若冲突，以详细文件为准。

- **平均句长约 21 词。上限约 40 词（仅贡献列表）。**
  > **【英文专属】** "词数"是英文的度量单位。中文的字/词边界不同，"21 词"这个目标值**不能**直接搬到中文写作上。
- 主题句必须断言主张。绝不用背景或语境开段。
  > 【译注】与语言无关。
- 零对冲。"We show" 而不是 "We believe"；"X reduces Y by 13×" 而不是 "X may help reduce Y"。
  > **【英文专属】** 举的例子是英文动词形式。规则精神（不用模糊的情态动词）通用，但具体词汇表是英文的。
- **全文主动语态，无例外。** 被动语态掩盖施动者、削弱文字。
  > **【英文专属】** 英文用 `be + 过去分词` 构成被动，`gate_mechanical.md` 的 M11 就用这个模式匹配。**中文的"被"字句频率与语用完全不同，此禁令不能照搬。**
- 不用填充形容词：绝不用 "novel"、"significant"、"state-of-the-art"、"comprehensive"、"robust"、"substantial"、"promising"、"impressive"。换成具体数字或删掉。
  > **【英文专属】** 这是一份**英文词表**——`gate_mechanical.md` 的 M5 直接 grep 这些英文单词。中文写作需要另建一份对应清单。
- **用主张做路标**：节的开启句可以陈述本节结论（"This section shows that X reduces Y by 13×"），但绝不能用无信息占位（"In this section, we describe..."）。判据：这个开启句告诉跳读者本节**得出什么结论**了吗？
  > 【译注】判据与语言无关；例句是英文。
- 无感叹号。引言之外不用反问句。
- 段落 4–6 句。每段只做一件事：提出主张、呈现证据、或综合 Takeaway。
- **标题是主张，不是话题。** "Event-centric decomposition reduces error 13×" 而不是 "Experimental Results"。
  > 【译注】与语言无关，是这条原则最通用的部分之一。
- **具体名词优于含糊词**：每个机制、基线、指标都必须有专名。如果一个术语能套用到本领域任何论文上，它就不属于这篇论文。
  > 【译注】与语言无关。
- **解释图表，不是引用图表。** "Figure 3 shows that X, confirming Y" 而不是 "See Figure 3"。
  > 【译注】与语言无关。
- 每个评估小节以 **Takeaway 段**结尾。
  > 【译注】与语言无关。
- 每个设计选择**立刻**说明理由。不是 "we use X" 而是 "we use X because Y"。
  > 【译注】与语言无关。

### 会议适配

- **系统类会议（NSDI、SIGCOMM、CoNEXT、IMC）**：用 `\smartparagraph{}` 标签。系统类评估（延迟、吞吐、内存）。贡献框定为运营影响。相关工作放在评估之后。
- **机器学习会议（NeurIPS、ICLR、ICML）**：不用 `\smartparagraph`。冒号式副标题。可复现性检查表。框定为方法论推进。相关工作整合进正文。
- **Workshop / 短文（HotNets、ANRW）**：一切压缩 50%。以智识挑衅开场。

【译注】会议适配与语言无关，是关于**结构惯例**的。

---

## 强制风格审查（门 —— 适用于**所有** tex 修改）

**在呈现或提交任何新增或修改的 tex 内容之前，Claude 必须运行句级风格审查。** 这不是可选项、不由用户触发、也不限于整节草稿——它适用于**每一次**编辑，包括段落级改动、新增小节、以及总览重写。

审查对照 `author_profile/gate_mechanical.md`、`author_profile/compression_patterns.md`、`author_profile/craft_reference.md` 检查每一个改动过的句子。具体扫描并修复：

0. **机械门（`gate_mechanical.md`）——先跑，跑它的 grep。** 破折号（`---`、`—`、` -- `）禁用。对偶/镜像式修辞（"X, not Y"；"whatever it is called"）、说教式收尾（"the saving is the point"、"is not real"）、空洞强调词（"in effect"、"at its core"）、三项排比装饰、清嗓子开场（"Moreover"、"Notably"）、禁用/浮夸/花哨词汇、以及无信息开场（"In this paper, we…"）全部禁用。目标是平实、简短、陈述式的语域（见 `craft_reference.md`）。**不跑该文件里的 grep 门，就不要报告审查通过。**
   > **【英文专属】** 这一整条是**英文模式匹配**：破折号规则针对的是英文排版里的 em-dash；对偶、清嗓子开场、禁用词都是英文短语。**译文里出现的英文引号内容，就是 grep 实际要找的字符串**，不是给中文写作的禁令。
1. **否定先行的构造**：本该断言某物**是**什么的地方用了 "not X" 或 "rather than X"。改成正向陈述。
   > **【英文专属】** 判据（何时该正向断言）通用；例句是英文。
2. **清嗓子**："We address this problem by"、"To address this issue"、"In order to"、"It should be noted that"、"Note that"。删掉，直接用动作开场。
   > **【英文专属】** 这些是英文字串。中文写作有对应的冗余开场（"为了……我们……"），但**不是同一批词**，grep 抓不到。
3. **对冲**："can potentially"、"can be expected to"、"may help reduce"、"it is possible that"。换成断言语气（"produces"、"reduces"、"achieves"）。
   > **【英文专属】** 同上。
4. **泛化形容词**："significant"、"substantial"、"highly desirable"、"novel"、"robust"、"comprehensive"。换成具体数字或删掉。
   > **【英文专属】** 英文词表。
5. **句长**：标记任何超过 40 词的句子。拆分或压缩。
   > **【英文专属】** 40 词是英文的度量。
6. **被动语态**："accuracy was achieved by X" → "X achieves"。"Experiments were conducted on X" → "We evaluate on X"。全文主动语态，无例外，包括方法与评估。在本基础门里运行 `author_profile/gate_mechanical.md`（Part C, M11）的被动语态检测 grep；修复或论证每一个命中。
   > **【英文专属】** 模式匹配的是英文 `be + 过去分词`。
7. **缺失引用**：从其他章节复述的技术主张必须携带原有的引用（原则 14）。
   > 【译注】与语言无关。

**流程**：写完之后，(a) 运行 `gate_mechanical.md` Part C 的 grep 门并修复每个命中，然后 (b) 逐行读改动过的文字检查上面各项。报告一张"发现并修复的违规"汇总表（类别、计数），**包含 grep 计数**（破折号、修辞手法）——不能只写"已审查"。绝不允许仅凭脑内过一遍就宣称审查通过。然后 (c) 运行**独立对抗红队**（`red_team_protocol.md`）：由**没有写过这段文字**的审稿者重跑 `gate_mechanical.md` 并以新读者视角施加 `author_profile/gate_semantic.md`（先定义后使用、可跟随性、与论点挂钩、词汇与拆解一致性、非重复、可映射性、诚实定位、以及闭合门），返回**问题列表**，不是「是/否」。只有通过 (a) + (b) + (c) 且**粘贴了 grep 输出作为证据**的文字，才可以呈现或提交。依据 `gate_semantic.md` 的**闭合门**（S31），每次实质性改动后迭代红队，直到最终闭合审稿者返回零 CRITICAL/MAJOR；绝不把残留项当作"已完成"搁置。

这个门与下面的结构检查表是**分开的、额外的**。

**Loop 模式。** 当通过 `/loop` 调用时（例如 "apply the paper-writing skill iteratively"、"audit with loop"），遵循 `loop_mode.md`：一个可恢复、以账本支撑的审查 → 红队 → 修复循环，每轮处理一节，当所有在范围内的节都干净时自行停止。用户无需指定跑哪些检查、何时停止——协议自带。

---

## 章节检查表

生成**任何**章节草稿后，Claude 还必须读对应那份结构检查表并运行它：

| 章节 | 检查表文件 |
|---|---|
| 引言 | `writing_checklists/intro_questions.md` |
| 评估 | `writing_checklists/evaluation_questions.md` |
| 设计 / 方法 | `writing_checklists/design_questions.md` |
| 相关工作 | `writing_checklists/related_work_questions.md` |

在呈现草稿之前标出每一个违规。严重度分级：CRITICAL（结构性——会导致拒稿）、MAJOR（评审可见）、MINOR（打磨级）。

【译注】检查表与语言无关。

## 章节动作序列

每节类型的动作序列细节见 `section_rhetorical_moves/`：

| 章节 | 文件 | 关键动作 |
|---|---|---|
| 引言 | `section_rhetorical_moves/introduction.md` | 利害关系 → 问题缺口 → 关键抽象 → 设计直觉 → 贡献 → 结果预览 |
| 评估 | `section_rhetorical_moves/evaluation.md` | 环境锚定 → 正面对比 → 深入剖析 → Takeaway 综合 → 消融 → 鲁棒性 |
| 设计 | `section_rhetorical_moves/design.md` | 引入抽象 → 论证设计 → 组件架构 → 关键设计决策 → 鲁棒性 |
| 相关工作 | `section_rhetorical_moves/related_work.md` | 按类聚类 → 逐类局限 → 定位句 |

这些包含可操作的指导，附带录用与被拒论文中的正反例。

【译注】动作序列与语言无关——它是**论证的功能结构**，不是措辞。

---

## 五阶段流水线

每篇论文按顺序走这些阶段。Claude 识别用户处在哪个阶段，并执行那个阶段的规则。

### 阶段 1：结构化头脑风暴 → 创建项目上下文

**门槛**：用户必须有一句话的身份陈述，以及写**成结果形态**的贡献声明。若没有，读 `brainstorming_guide.md` 并带用户交互式走完全部 6 个阶段。不要赶——这是最重要的阶段。含糊的项目上下文产出含糊的论文。

头脑风暴之后，用 `examples/project_context.md` 的模板生成 `project_context.md`，存放在论文的工作目录。真实样例见 `examples/netburst_project_context.md`。

**重要**：创建 `project_context.md` 后，把它加进项目的 `.gitignore`（不存在就创建该文件）。这个文件包含战略定框笔记和导师意见，默认不应提交到共享仓库。

### 阶段 2：架构

**门槛**：章节大纲、主张归属、每节的叙事弧、图表计划、评估结构、页数预算。

**技艺参考**：如果学生做过文献综述（用 [literature-survey-skill](https://github.com/SNL-UCSB/literature-survey-skill) 或手工做），检查 Pass 3+ 的论文笔记里有没有写作技艺提取——引言解剖、评估架构、设计章节结构、以及本领域最强论文的图表设计选择。作为架构的参考材料载入。**目标会议上最好那篇论文的章节结构，比通用模板更适合作为你大纲的起点。** 阅读与写作互相促进：深读时提取的技艺模式直接喂养你自己论文的架构。

**来自可视化产物的图表计划**：如果学生一直在用 [data-visualization-skill](https://github.com/SNL-UCSB/data-visualization-skill)，检查 `plot_context.md` 和 WALTER 叙述。每份 `plot_context.md` 记录了图表的意图、变量映射、图型理由和设计决策——是下面图表计划的现成条目。每段 WALTER 叙述（Hypothesis → Axes → Look here → Trend → Exception → Result）直接映射到伴随该图的评估文字。学生在 viz skill 里做的迭代——探索数据说明了什么、形成预测、面对意外——已经决定了哪些图承载论证。架构应当反映这一点。

输出一张结构化表格：

| 章节 | 页数 | 关键主张 | 图表 |
|---------|-------|-----------|----------------|
| ... | ... | ... | ... |

**非数据图规格**：对计划中每一张**非**数据图（架构图、流水线示意图、概念图、对比示意图），读 `figure_synthesis_guide.md` 并运行 spec 模式产出 `figure_spec.md`。数据图（CDF、柱状图、热力图、散点图）应转给 [data-visualization-skill](https://github.com/SNL-UCSB/data-visualization-skill)。边界很清晰：**渲染它需要实验数据 → 走 `/viz`；它说明结构、流程或概念 → 走图表合成。**

### 阶段 3：分节草稿

**强制顺序**：Draft 0 引言 → 评估 → 设计/方法 → 背景 → 相关工作 → 最终引言 → 摘要。

引言要写**两遍**。这是整个系统中影响力最大的原则（原则 1）。

**Draft 0 引言**最先写。它是一个定框脚手架——利害关系、问题缺口、粗略的贡献声明——为评估设定护栏。Draft 0 澄清论文**试图**证明什么。它是**显式可抛弃的**：它大概率不会活到终稿。写作是思考工具，不只是沟通工具——Draft 0 迫使学生在设计实验之前把定框外化。

**评估**紧随其后，被 Draft 0 的护栏约束。然后是设计、背景、相关工作。

**最终引言**在评估完成之后**从零重写**。它只承诺证据支撑得起的东西，不多不少。Draft 0 是参考资料，**不是**编辑的起点。如果用户想跳过 Draft 0 直接写评估，解释：没有定框护栏的评估会做出一堆无法汇聚成统一论证的实验。

**每节的脚手架**：写任何一节的完整正文之前，先只写主题句。按顺序读一遍——它们本身就该构成连贯的论证。主题句读不通，段落也读不通。只在主题句序列站得住之后，才填入完整段落。LaTeX 里的实用技巧是在写正文前用注释标注每段的意图：

```latex
\section{Introduction}
% Stakes: who suffers and why the domain matters.
...
% Problem gap: structural limitation of current approaches.
...
% Key abstraction: named concept that captures our insight.
...
% Contributions: numbered, claim-first list.
...
```

每条注释是一份契约：后面的段落必须兑现它。如果某段对不上任何注释，要么这段不该存在，要么漏了一条注释。

**每节审查**：生成草稿后，读并运行对应的检查表。在呈现草稿前标出每一个违规及其严重度。

### 阶段 4：整合

跨节一致性检查：

- **术语漂移**：同一个概念是否处处用同一个名字？
- **主张-证据映射**：引言的每条主张是否都映射到评估的某个小节？
- **关键抽象传播**：引言动作 3 里命名的概念，是否出现在设计动作 1、评估环境和相关工作定位里？
- **标题一致性**：章节标题是否反映引言的贡献顺序？
- **身份稳定性**：读每一节的第一句——它们讲的是连贯的故事吗？
- **流动审查**：通读全篇，读第 N 段的最后一句和第 N+1 段的第一句。每个过渡都成立吗？**行文**（读者能跟随的逻辑流）是技术写作最重要的方面。读者原谅不完美的语法，但无法跟随断掉的逻辑推进。结构产生流动，但结构本身不保证流动——结构单元之间的衔接才是关键。
- **路标检查**：每一节的开场句是否承载主张、告诉跳读者本节得出什么结论？引言末尾是否有概述段？图表是否分布在各页而非挤在一处？
- **视觉平衡（版面）**：检查图表在各页的分布、段落长度的变化、留白。用路标、图表、段落小标题打断大段文字。避免章节标题孤零零留在页底。

### 阶段 5：压缩

7 个具体操作见 `author_profile/compression_patterns.md`。按顺序施加：

1. 缩短句子（删从句、限定词、清嗓子的话）
2. 合并段落（同一论点的多个例子 → 只留最好的一个）
3. 删除泛化形容词（"significant" → 具体数字或直接删）
4. 删除教程内容（目标会议读者早就知道的东西）
5. 主张前置（把埋在后半段的改写成一开头就说结论）
6. 插入 Takeaway（实验群组之后补综合段）
7. 图表提升（密集的数值对比从正文挪进图表）

目标：比初稿减少 **30–50%**。报告压缩前后的字符数。

**不要为了凑页数注水。** 压缩后不足页数限制，没问题。内容得当的短论文胜过硬撑到页数上限的注水论文。注水引入填充物，削弱论证。

### 投稿前机械检查（自动执行）

压缩之后、投稿之前，Claude 必须用 shell 命令在论文的 `.tex` 和编译出的 `.pdf` 上自动运行这些检查。**不要让学生手动跑——直接执行并报告结果。**

**1. 页数。** 从编译后的 PDF 提取页数，与会议限制比对（来自 `project_context.md`）。标出参考文献/附录是否计入——各会议规则不同。
```bash
pdfinfo paper.pdf | grep Pages
```

**2. 断引用。** 在 `.tex` 源文件中查找会渲染成 `[?]` 或 `??` 的未解析引用。同时检查 `.log` 文件里关于未定义引用和引文的 LaTeX 警告。
```bash
grep -n "LaTeX Warning.*undefined" paper.log
grep -rn '\\cite{' *.tex | grep -v '%' # list all citations for cross-check
```

**3. 字体嵌入。** 验证所有字体都已嵌入。未嵌入的字体会导致跨机器渲染差异，并被部分投稿系统拒收。
```bash
# $(NF-4) counts from the end: the type column holds one or two words, so fixed column numbers shift.
pdffonts paper.pdf | awk 'NR>2 { if ($(NF-4)!="yes") print "NOT EMBEDDED: " $0; if ($0 ~ /Type +3/) print "TYPE 3 (venue reject): " $0 }'
```
若任何字体在 `emb` 列显示 `no`，标出并建议加 `\usepackage[T1]{fontenc}` 或用 `GS_OPTIONS=-dPDFSETTINGS=/prepress` 编译。

**Type 3 字体的 `emb` 是 `yes`，所以只看 `emb` 的检查会放行它们。** IEEE 和 ACM 拒收 Type 3，而且它们缺少 Unicode 映射，会导致 PDF 文字不可复制、不可搜索。也要检查 `type` 列。Matplotlib 默认输出 Type 3。保存图之前设置 `pdf.fonttype = 42` 和 `ps.fonttype = 42`，`/viz` 的 `matplotlib_defaults.py` 现在已经这么做了。

**4. 图片质量。** 检查所有引用的图片文件是矢量格式（PDF/EPS）还是高分辨率位图。列出源码中引用的全部图并确认存在。
```bash
grep -rn '\\includegraphics' *.tex  # list all figure references
file figures/*.pdf figures/*.png 2>/dev/null  # check file types
```
标出任何 PNG/JPG 图——除非是照片或截图，否则应为矢量。

**5. 匿名化（双盲时）。** 在所有 `.tex` 文件中搜索作者名、单位名、基金号、致谢段、以及可能泄露身份的自引。
```bash
grep -rni 'AUTHOR_NAMES_HERE\|INSTITUTION_HERE\|\\thanks\|acknowledgment' *.tex
```
运行前把 `AUTHOR_NAMES_HERE` 和 `INSTITUTION_HERE` 换成 `project_context.md` 里的真实姓名。

**6. 分栏平衡。** 检查是否加载了 `balance` 包（用于双栏格式）。若没有，建议在 `\bibliography` 之前加 `\usepackage{balance}` 和 `\balance`。
```bash
grep -rn 'balance' *.tex
```

**7. 常见 LaTeX 问题。** 扫描常被漏掉的机械问题：
```bash
grep -rn '\\cite{.*}' *.tex | grep -v '~\\cite'  # missing ~ before \cite (dangling references)
grep -rn 'et al\.' *.tex | grep -v '~'  # missing ~ after "et al."
grep -rn '\\label{' *.tex | sort | uniq -d  # duplicate labels
```

**报告格式**：跑完所有检查后，呈现一张汇总表：

| 检查 | 状态 | 详情 |
|-------|--------|---------|
| 页数 | ✓ 或 ✗ | N 页（限制：M） |
| 断引用 | ✓ 或 ✗ | 未定义引用列表 |
| 字体嵌入 | ✓ 或 ✗ | 未嵌入或 Type 3 字体列表 |
| 图片质量 | ✓ 或 ✗ | 位图列表 |
| 匿名化 | ✓ 或 ✗ | 发现的泄露列表 |
| 分栏平衡 | ✓ 或 ✗ | 包存在/缺失 |
| LaTeX 问题 | ✓ 或 ✗ | 悬空引用、重复标签计数 |

能自动修的自动修（例如补 `~`），需要学生决策的标出来（例如把位图换成矢量图）。

【译注】整个投稿前检查与语言无关——它检查的是 LaTeX 与 PDF 的机械问题。

---

## 如何回应常见请求

### "帮我写论文 Y 的第 X 节"
1. 载入该论文的 `project_context.md`
2. 读全部 `author_profile/` 文件
3. 读该节的 `section_rhetorical_moves/`
4. 判断论文处在哪个阶段——执行引言写两遍的顺序（Draft 0 引言 → 评估 → 设计 → 背景 → 相关工作 → 最终引言 → 摘要）
5. 先写主题句；在填段落之前验证它们构成连贯论证
6. 按动作序列生成草稿
7. 运行该节检查表并标出违规及严重度

### "评审 / 批判这份草稿"
1. 载入 `project_context.md`
2. 读 `author_profile/intervention_types.md` 了解 7 种干预类型
3. 按顺序施加：定框 → 结构重写 → 背景删除 → 评估强化 → 术语收紧 → 主张前置 → 压缩
4. 给出编号反馈，带严重度（CRITICAL / MAJOR / MINOR）、违反的具体原则（`editorial_principles.md` 里的编号）、以及具体改写

### "压缩 / 精简这一节"
1. 读 `author_profile/compression_patterns.md`
2. 按顺序施加 7 个操作
3. 给出前后对照及字符数
4. 正常压缩为 30–50%。超过 50% 说明是定框问题，不是啰嗦问题。

### "我想找人看这份草稿"
1. **第一遍阅读是宝贵的。** 一个人只能第一次读你的作品一次。不要同时把初稿发给所有人——把反馈串成链。先给一个读者，吸收他们的反馈，再把修订版给下一个。每一轮都让草稿更强，再到达下一双新眼睛。
2. 帮学生规划反馈链：谁先读（领域外的人，看框架是否清晰）、谁第二（领域专家，看技术是否正确）、谁最后（导师，看到最强的版本）。
3. 向读者要反馈时，告诉他们该关注什么："引言的叙事清楚吗？"比"有什么想法吗？"有用得多。没有焦点的反馈浪费一次第一遍阅读。
4. 用 Claude 的评审模式（`author_profile/intervention_types.md`）**在**消耗一个真人读者的第一遍阅读**之前**，先模拟一轮反馈。

### "帮我回复审稿人"
1. 载入 `project_context.md` 和审稿意见
2. 按严重度分类每条关切：定框（最危险）→ 设计 → 范围 → 严谨性（最好处理）
3. 对每条：承认 → 说明改了什么 → 指向具体章节/图
4. 绝不防御。绝不轻视。如果审稿人误解了，那是**写作**的失败要修，不是审稿人的失败要批评。

### "我要开始一篇新论文——从哪开始？"
1. 欢迎他们。解释五阶段流水线——尤其是引言写两遍原则。
2. 读 `brainstorming_guide.md`，带他们走完 6 个阶段创建第一份 `project_context.md`。
3. 强调：头脑风暴阶段最重要。精确的项目上下文能省掉后面几周的返工。
4. 头脑风暴之后：写一份 **Draft 0 引言**——一个可抛弃的定框脚手架（利害关系、问题缺口、粗略贡献），为评估设护栏。然后指向 `section_rhetorical_moves/evaluation.md`——他们接下来写评估，被 Draft 0 约束。
5. 提醒他们："你的初稿应该求全——什么都写进去。压缩是后来的事。目标是让材料落到纸上，不是简洁。Draft 0 引言大概率不会存活——这就是重点，它澄清你的思考。"

### "我需要一张非数据图（架构图、流水线图等）"
1. 载入论文的 `project_context.md`
2. 读 `figure_synthesis_guide.md` 和 `figure_templates/` 中的相关文件
3. 按原型分类（架构总览、流水线流程、组件细节、概念示意、对比示意、分类矩阵、部署图）
4. 选择生成后端：AI 图像生成（视觉丰富的图，如架构总览和概念示意）或 TikZ（需要精确结构的图，如流水线和分类矩阵）。指南给出每种原型的默认值，学生可以覆盖。
5. 运行 spec 模式——走原型专属问题，产出 `figure_spec.md`
6. 运行 generate 模式——组装 AI 提示词或 TikZ 代码，产出图
7. 运行 critique 模式——对照主张、会议格式和设计原则检查，并执行标题简洁性（G7：一个加粗的 takeaway 加至多一个从句；标出超过约 3 行的标题），**并且通过查看渲染出的图来验证可读性**（一张检查表全绿的图仍可能是看不清的一团）
8. **注意**：数据图（CDF、散点图、柱状图、热力图）应走 `/viz`，不走图表合成

【译注】以上七类常见请求的应对流程与语言无关，是**工作流**层面的。

---

## 附：本文覆盖范围的说明

本文只对照了 `SKILL.md`。这个 skill 的完整注入内容还包括：

- `author_profile/` 下 7 个规则文件（约 1020 行）——**每次调用都载入**
- `writing_checklists/`、`section_rhetorical_moves/`（按需）
- `brainstorming_guide.md`、`figure_synthesis_guide.md` + `figure_templates/`（按需）
- `red_team_protocol.md`、`loop_mode.md`（特定模式）

若要继续，按同一体例逐文件铺开即可。
