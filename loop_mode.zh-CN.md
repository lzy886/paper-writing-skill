# Loop 模式 —— 迭代式审查与修复（用于 `/loop` 功能）

> 对照译本。规则编号、命令、grep 模式保留原文。

> ## 语言相关性
>
> **本文件完全与语言无关。** 它是一份**流程规范**——读什么、按什么顺序、什么时候停。
> 唯一的英文是那条 findings 示例串（`fixed 2× M11 passive...`），它本身是规则编号的拼装。
>
> **这是全项目唯一规定"skill 被 loop 驱动时如何行为"的规范正文。** 但注意：`SKILL.md` 第 137 行
> 还有一段**路由**，把 `/loop` 指向本文件。换驱动方式时，两处都要改。

---

> **目的**：让用户执行 `/loop apply the paper-writing skill iteratively`（或 "audit with loop"），
> 然后 skill 就**不需要任何进一步指令**地跑一个**可恢复的**审查 → 红队 → 修复循环。每一轮迭代都是
> 一个自足单元：从磁盘读状态 → 做下一块工作 → 更新状态。这样**进度能在迭代之间的上下文丢失中
> 存活**。论文干净时循环自己结束。
> 添加于 2026-07，来源于一次实况审查（`notes/PAPER_WRITING_SKILL_GAPS.md`）。

## 用户输入什么（调用方式）

- `/loop apply the paper-writing skill iteratively` ← **自定节奏；推荐**
- `/loop audit with the paper-writing skill until clean`
- 限定范围：`/loop audit §2 and §3 with the paper-writing skill`

**不需要任何其他细节**——本文件提供了那些显然的部分（查什么、什么顺序、何时停止）。

## 持久账本（跨迭代的记忆）

在论文目录维护 `notes/AUDIT_LEDGER.md`：一节一行，一道门一列。

| Section | Mechanical (gate_mechanical) | Semantic (gate_semantic S1–S31) | Independent red-team | Status |
|---------|---------------------------|---------------------------------|----------------------|--------|

状态：`PENDING` / `FINDINGS(n)` / `CLEAN`。逐节记录**粘贴的 grep 计数**和最近一次红队的 findings，
并且**每条修复都要引用确切的规则编号**（`M#` / `S#`），这样账本能显示每一轮迭代处理了哪条规则
（例如 `§3.2: fixed 2× M11 passive, 1× M12 wordiness, 1× S13 gloss pile-up`）。
**规则编号就是溯源。** 这个账本**就是**循环的记忆——**每一轮开始时重新读它**。

## 一轮迭代（可重复的单元）

1. 读 `notes/AUDIT_LEDGER.md`（不存在就按章节列表创建）和 `project_context.md`（论文的统率思想）。
2. 选出下一个工作单元：**优先级最高、且尚未通过三道门达到 `CLEAN` 的那一节**（尊重用户的限定范围）。
3. 对该节跑三道门（见 `red_team_protocol.md`）：
   a. **机械门** —— 跑完 `author_profile/gate_mechanical.md` Part C 的**全部** grep；粘贴计数。
   b. **语义门** —— 施加 `author_profile/gate_semantic.md`（S1–S31）：可达性、连贯性、一致性、
      严谨性、诚实定位、图表。
   c. **craft 层** —— 对照 `author_profile/craft_reference.md` 检查改动过的文字（该语域的正向规则）。
   d. **独立红队** —— spawn 一个**全新的**审稿者（subagent），它**没有写过**这段文字；返回 findings。
4. **修复**该节文字里的每一条 finding。
5. **重跑**三道门。零残留 → 标记该节 `CLEAN`；否则 `FINDINGS(n)`。
6. **更新** `notes/AUDIT_LEDGER.md`（grep 计数、**修复的规则编号**（`M#`/`S#`）、findings、状态）。
   如果某一节变了，重建 PDF。
7. 输出**一行**迭代摘要（小节、修复的规则编号、findings 数、状态）。

## 停止条件（循环自己结束）

**当所有在范围内的节都通过三道门达到 `CLEAN`（账本全绿）时结束**：报告最终账本并停止。
**如果连续两轮迭代没有进展（同样的 findings 存活），停止并报告阻塞，而不是空转。**

## 规则

- **独立红队必须是独立的审稿者**（fresh subagent），**绝不能是作者自检**——自我审查正是这套协议
  要修的那个失败模式。
- **每轮迭代保持小**（一节，或大节的一道门），这样能塞进一个 loop tick。
- **对图表，"干净"要求看过渲染出来的图**，不是检查表过一遍（检查表全绿的图仍可能是看不清的一团
  ——**靠看**来验证）。
- **默认章节顺序 = 论文的顺序**；用用户的限定范围覆盖它。

---

## 附：这个文件在迁移到 Codex 时要动哪里

> 【译注】此节是译本补充的，源文件没有。放在这里是因为你正打算迁移，而**它是唯一需要改的正文**。

| 位置 | 现在写的 | 迁移后 |
|---|---|---|
| 标题 | "for the `/loop` feature" | for Codex goal-driven mode |
| **调用方式一节**（第 9–13 行） | 用户打 `/loop ...` | 用 `create_goal` 建一个 goal，objective 写明"把 §1–§5 审到 CLEAN 即 complete" |
| **停止条件一节** | "ledger all green → 报告并停止" | 账本全绿 → 调 `update_goal` 标记 `complete` |

**不用动的部分**（占全文约 85%）：

- 账本的结构与状态机（`PENDING` / `FINDINGS(n)` / `CLEAN`）
- 七步迭代单元
- 三道门 + 红队的顺序
- 那四条 Rules（独立审稿者、小步迭代、图要看渲染结果、默认章节顺序）
- 空转保护（连续两轮无进展则停）

**为什么账本留着**：Codex 的 `thread_goals` 表是一行一个 goal，只有 goal 级状态，**没有"一节一行、
一门一列"的粒度**。goal 负责**宏循环**（谁推动下一轮、何时停），账本负责**微进度**（哪一节到哪一门
干净了）。两者互补。

**还要记得改 `SKILL.md` 第 137 行**——那段把 `/loop` 路由到本文件的分发语句。
