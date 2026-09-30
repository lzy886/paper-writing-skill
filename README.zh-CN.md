# paper-writing skill 中文对照译本

> **用途**：本文档集是 `paper-writing` skill 的中文对照，供审查这个 skill **究竟向上下文注入了什么**。
>
> **摆放位置**：每份译本都放在它所对照的原文件**旁边**，便于对照阅读。例如 `SKILL.zh-CN.md` 紧挨 `SKILL.md`，`author_profile/gate_mechanical.zh-CN.md` 紧挨 `author_profile/gate_mechanical.md`。
>
> **不改规则**：一切语法、句法、风格约束**仍然指向英文写作**。译本只做说明，不改变任何执行行为，也不覆盖任何源文件。
>
> **保留英文原文的部分**：规则编号（`M1`–`M18`、`S1`–`S31`）、文件名、shell 命令、grep 模式、LaTeX 代码、YAML frontmatter。中文引号里出现的英文串，就是 grep 实际要匹配的字符串。

## 标注约定

| 标记 | 含义 |
|---|---|
| **【英文专属】** | 该规则作用于**英文特有的语言现象**，中文里没有对应物。见到此标记，说明译文里的说法**不能**类推到中文写作。 |
| **【译注】** | 一般性说明。多数是"此条与语言无关"。 |

**为什么要标【英文专属】**：译本用中文写，但描述对象始终是英文论文。不标注的话，读者容易把"平均 21 词""被动语态零容忍"这类规则误当成通用的写作建议。

**标注密度按文件的语言相关性浮动**，不是一个模子：

| 文件 | 标注方式 |
|---|---|
| `gate_mechanical` | **全文件一条总注**——它整份都是英文模式匹配，逐条标注只是重复 |
| `gate_semantic` | 单点标注 5 处（31 条里只有 5 条扎在英文语法上） |
| `craft_reference` | 密集标注——**受语言影响最严重的一份** |
| `editorial_principles` / `compression_patterns` / `intervention_types` / `rhetorical_moves` | 几乎不标——它们是方法论，与语言无关 |

## 进度

| 源文件 | 译本 | 状态 |
|---|---|---|
| `SKILL.md` | `SKILL.zh-CN.md` | ✅ 完成 |
| `author_profile/editorial_principles.md` | `author_profile/editorial_principles.zh-CN.md` | ✅ 完成 |
| `author_profile/craft_reference.md` | `author_profile/craft_reference.zh-CN.md` | ✅ 完成 |
| `author_profile/gate_mechanical.md` | `author_profile/gate_mechanical.zh-CN.md` | ✅ 完成 |
| `author_profile/gate_semantic.md` | `author_profile/gate_semantic.zh-CN.md` | ✅ 完成 |
| `author_profile/compression_patterns.md` | `author_profile/compression_patterns.zh-CN.md` | ✅ 完成 |
| `author_profile/rhetorical_moves.md` | `author_profile/rhetorical_moves.zh-CN.md` | ✅ 完成 |
| `author_profile/intervention_types.md` | `author_profile/intervention_types.zh-CN.md` | ✅ 完成 |
| `writing_checklists/`（4 份） | — | ⬜ 待译 |
| `section_rhetorical_moves/`（4 份） | — | ⬜ 待译 |
| `brainstorming_guide.md` | — | ⬜ 待译 |
| `figure_synthesis_guide.md` + `figure_templates/` | — | ⬜ 待译 |
| `red_team_protocol.md` | — | ⬜ 待译 |
| `loop_mode.md` | — | ⬜ 待译 |

**`author_profile/` 已全部完成**——那是每次调用 skill 都会载入的部分（约 1020 行），既占上下文开销的大头，也装着全部实际执行的规则。

## 两种读法

**先读 `SKILL.zh-CN.md`** —— 它开篇有一张**注入结构总览**表，一眼看清哪些内容常驻、哪些按需载入、各占多少行。想快速理解这个 skill 的工作方式，从这里开始。

**再读 `author_profile/` 的译本** —— 想审查**具体执行了哪些检查**，看这部分。规则编号与 grep 模式原样保留。

## 三个诚实的提醒

1. **规则编号不可改动。** `M11`、`S13` 是审计账本和红队 findings 的**溯源键**。若你据此译本做中文版规则，编号要沿用，否则溯源链断掉。
2. **【英文专属】的条目不是"不需要"，是"需要重新推导"。** 例如被动语态禁令在中文里对应的是**另一套问题**（无主句、被字句的语用），不是"可以随便用被动"。
3. **本译本不参与执行。** skill 运行时读的是英文原文。

   这一点靠一条机制保证，而不是靠自觉：`SKILL.md` 第 2 步的读取指令已写成「读 `author_profile/` 下所有文件，**但跳过 `*.zh-CN.md`**」。**没有这条排除，译本会被当成规则一起载入**——`author_profile/` 会从 1029 行涨到 2272 行，并注入一份重复的规则副本。

   若你以后把译本挪到别处，或者给其他目录（`figure_templates/`、`section_rhetorical_moves/` 等）也加译本，要检查对应的读取指令是否也需要同样的排除。目前 `author_profile/` 是唯一使用「读目录下全部文件」写法的，其余目录都按文件名逐个读取，不受影响。
