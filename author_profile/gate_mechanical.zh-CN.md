# 机械门 —— 唯一的可 grep 风格审查

> 对照译本。规则编号、grep 模式、示例中的英文串**一律保留原文**。

> ## ⚠️ 全文件总注：本文件的每一条规则都作用于**英文正文**
>
> 这里的 M1–M18 不是通用写作建议，而是**英文模式匹配规则**。Part C 的 grep 脚本直接在
> `sections/*.tex` 里搜英文字串——`\bnovel\b` 找的是 "novel" 这个词，"对偶句式"找的是
> `X, not Y` 这种英文构造。
>
> **中文写作不能照搬**：
> - 禁用的形容词表（M5）是**英文词表**，中文要另建一份
> - 被动语态检测（M11）匹配英文 `be + 过去分词`；中文"被"字句的频率与语用完全不同
> - 破折号禁令（M1）针对英文排版的 em-dash；中文用的是 `——`（双破折号），是另一套排版惯例
> - 清嗓子开场（M6）、冗词（M12）、弱限定词（M13）都是**英文字串清单**
>
> 下面只在非显然处加单点标注。**默认假设：每条都作用于英文。**

---

> 唯一的、集中存放所有机械可检测风格规则的地方。合并了原先分散在三份 grep 脚本
> （de-AI、Elements of Style、hardening）里的检查，加上基础声音档案的禁用词表。在基础门里对
> 每次 `.tex` 编辑运行，并在红队 / loop 环节再跑一次。**报告 grep 计数，不是"已审查"。**
> 每个命中要么修掉，要么在编辑报告完成前显式论证。**脑内过一遍不算运行。**

这里放的是正则能抓的检查。需要读者判断的检查（先定义后使用、可跟随性、连贯性、有据）在
`gate_semantic.md`。正向的"该写什么"指导在 `craft_reference.md`。

---

## Part A —— 禁用构造（每个命中都要修）

每条规则先陈述禁令，再给"错→对"示例。

**M1. 破折号 —— 禁用，无例外。** 不许 `---`（em）、不许 ` -- `（当破折号用的 en）、不许
`—`/`–`（Unicode）。这是机器写作最强的指纹。改用逗号、冒号、括号或句号。插入语 → 括号；
同位语 → 逗号；戏剧性揭示 → 新句子。

- ✗ `One process, the Core --- parses the intent.` → ✓ `One process, the Core, parses the intent.`
- 唯一例外：外部来源逐字引用内部的破折号。

> **【英文专属】** 这是英文排版的破折号惯例。中文的 `——` 是标准标点，不受此禁令约束。

**M2. 对偶 / 把否定当修辞 —— 作为装饰时禁用。** `X, not Y` · `not X but Y` ·
`X rather than Y` · `less X than Y` · `not only X but` · `X is what Y does not satisfy` ·
`more than just X` · `not in competition with X` · `treat X as Y rather than as Z` ·
`on one hand … on the other hand`（只做了一边时）。

- **保留还是删除的判据**：删掉否定、改成正向陈述后，句子会丢事实内容吗？会 → 否定在做实事，
  保留。不会 → 它是修辞，删掉并断言正向。
- ✗ `measured, not asserted` → ✓ `measured on real studies`
- ✗ `Cross-layer coupling, not any single constraint, makes it hard.` → ✓ `Cross-layer coupling makes it hard.`
- **该保留的事实性否定**："students cannot derive…"（真实的能力缺口）、"the system fails when…"
  （真实的失效模式）、"39 of whom had taken…"（定量事实）。

**M3. 说教 / 推销式收尾 —— 禁用。** 唯一职责是告诉读者该对上一句作何感受的句子：
`the saving is the point` · `is not real` · `this is the empirical content of the claim` ·
`a testament to X` · `the kind of Y that…` · `a concrete demonstration of`。陈述事实，信任它。

**M4. 空洞强调词与炒作 —— 禁用。** `in effect` · `in a sense` · `at its core` ·
`at its heart` · `in essence` · `truly` · `genuinely` · `simply` · `essentially` ·
`fundamentally`（作填充时）· `real(ly)` · `in fact` · `indeed` · `actually` · `exactly` ·
`precisely because` · `armed with` · `seamless(ly)`。删掉，或换成精确的指称对象。

**M5. 禁用形容词 —— 禁用（换成数字或删掉）。** `novel` · `significant` · `substantial` ·
`impressive` · `promising` · `comprehensive` · `robust` · `powerful` ·
`state-of-the-art`（指名具体已有系统时除外）· `paradigm` · `leverage`（动词）· `utilize`。
它们断言质量而不陈述质量。

> **【英文专属】** 这是英文词表。中文科技写作有对应的空话（"显著提升""鲁棒性强""具有重要
> 意义"），但**不是这批词**，grep 抓不到，需要另建规则。

**M6. 清嗓子开场 —— 禁用。** 以这些词开头的句子：`Moreover` · `Furthermore` ·
`Additionally` · `Notably` · `Importantly` · `Indeed` · `Ultimately` · `Crucially` ·
`In turn` · `That said` · `It is worth noting that` · `It should be noted that`。直接用主张开场。
（语域例外见 `craft_reference.md`：在叙事 / 定位语域中，只有当后面跟着真实逻辑推进时才保留
话语标记。）

**M7. 三项排比 / 三元装饰 —— 慎用。** 当成员近乎同义、属于填充，或者 `X does A, B, and C`
这种句式连续出现三句时，三段平行列表读起来像生成的。三个成员都独特且承重时才保留三元组。

**M8. 宏大空洞的铺垫句 —— 禁用。** announce 重要性却推迟兑现的句子：`What sets X apart is…` ·
`The key insight is…` · `Two commitments no predecessor made…`。直接陈述洞见。

**M9. 品牌化 / 煽情隐喻填充 —— 禁用。** 听起来唬人但没有操作内容的隐喻：
`the machine that runs any spec` · `draw their power from` · `under the hood` ·
`X meets Y`（`where structure meets scale`）。用机制本身。

**M10. 模糊机制 / 未来学炒作动词 —— 审查（换成陈述的机制）。** `keeps X from Y` ·
`stands in the way of` · `unlocks` · `powers` · `promises to` · `stands to` ·
`is poised to` · `opens the door to` · `is set to` · `has the potential to`。每一个都藏着一个
未陈述的机制。换成机制、一个具名的问题、或置信度分级的断言。

**M11. 被动语态 —— 禁用，零例外，包括方法与评估。** `X was achieved by Y` → `Y achieves X`；
`experiments were conducted` → `we evaluate`；`data was collected` → `we collect`。绝不生成
被动的技术文字。每个被动命中要么修掉，要么显式论证。

> **【英文专属】** 检测模式是 `\b(is|are|was|were|be|been|being)\s+([a-z]+ed|done|made|...)`，
> 匹配英文 `be + 过去分词`。**中文的"被"字句频率低得多、语用也不同，此禁令不能照搬**；
> 中文在"施动者不明"时用无主句更自然。

**M12. 冗词 —— 删。** `the fact that` · `the question as to whether` → `whether` ·
`in order to` → `to` · `there is no doubt but that` → `no doubt` · `he/she is a X who` → `X` ·
`the reason … is because` → `because` · `owing to the fact that` → `since` ·
`in a <adj> manner` → `<adv>ly` · `this is a subject that` → `this subject` · `in terms of` ·
`one of the most` · `in the last analysis`。

**M13. 弱限定词 —— 删。** `rather` · `very` · `pretty` · `little`（作强调）· `quite` ·
`somewhat` · `fairly` · `certainly`（作强调）。改用本身就有力的词。

**M14. 生造副词与假序数 —— 修。** 不许 `thusly` · `muchly` · `overly` ·
`firstly/secondly/thirdly`（用 `first/second/third`）· `-wise` 伪后缀（`taxwise`）。

**M15. 感叹号与游离反问句 —— 禁用。** 技术文字中不用 `!`。反问句仅限引言，至多一个。

**M16. 浮夸 / 含糊的单词 —— 审查（另见 M5）。** `finalize` · `-oriented` · `factor` ·
`feature` · `meaningful` · `insightful` · `prestigious` · `possess`（→ `have`）· `impact`
（动词；优先 `affect`）· `contact`（动词）· `currently`（与现在时重复）。

**M17. 花哨 / 炫技 / 比喻性动词 —— 审查（优先用平实的词）。** 存在平实动词时，花哨的那个读起来
像作者在表演。只有当花哨动词是精确的领域术语、没有平实等价物时才保留（`amortize` 一项成本、
`traverse` 一张图、`spawn` 一个进程）。

- `pit (X against Y)` → `run` · `dispatch` → `send` · `chip away at` → `narrow`/`reduce` ·
  `marshal` → `gather` · `orchestrate` → `coordinate`/`run` · `wrangle` → `manage` · `harness` →
  `use` · `forge` → `build` · `weave` → `combine` · `usher in` → `start` · `delve into` →
  `examine` · `grapple with` → `address` · `surface`（动词）→ `reveal` · `unpack`（比喻）→ `explain`
  · `anew`/`afresh` → `again`。
- 多义词（`bind`、`surface`、`run`、`chip`）只做**判断**：读它有没有比喻义，不要一律替换。

**M18. 无信息开场 —— 禁用。** `In this paper, we…` 和 `In this section, we describe…`。
删掉，用主张开场（见 `craft_reference.md` 关于用主张做路标）。

---

## Part B —— 精确词对（逐个命中检查有没有用错那一个）

不禁用，但常被误用。标出并检查：`affect`/`effect` · `imply`/`infer` ·
`comprise`（避免 "is comprised of"）· `data is`（data 是复数）· `fewer`/`less` ·
`farther`/`further` · `that`/`which`（限定 vs 非限定）· `unique`（没有 "very unique"）·
`different than`（→ `different from`）· `due to`（状语用法中 → `because of`）。

> **【英文专属】** 整节都是英文语法点（含冠词与单复数），中文没有对应形式。

---

## Part C —— grep 脚本（**跑它**，不要用眼睛看）

```bash
# ── M1: Em-dashes (must return nothing) ──
grep -rn -- '---' sections/*.tex ; grep -rn $'—\|–' sections/*.tex

# ── M11: PASSIVE VOICE (also runs in the base per-edit gate). Over-catches on purpose. ──
grep -rnoE "\b(is|are|was|were|be|been|being)\s+([a-z]+ed|done|made|shown|given|taken|held|built|drawn|chosen|written|known|found|seen|set|put|sent|kept|met|run|used|based)\b" sections/*.tex

# ── M2: Antithesis / negation-as-rhetoric (inspect each; "not" branch matches digit/letter/\macro) ──
grep -rnoE ",? not [0-9A-Za-z\\]|not only .* but|rather than|less .* than|is the point|whatever it is|means nothing|more than just|not in competition with|on one hand|on the other hand" sections/*.tex

# ── M3/M4/M8/M9: editorializing, intensifiers, grandiose setups, metaphor filler ──
grep -rnoE "in effect|in a sense|at (its|the) (heart|core)|in essence|\btruly\b|\bgenuinely\b|\bindeed\b|\bin fact\b|precisely because|a testament to|the kind of .* that|exactly the kind|is the point|set(s)? .* apart|no (predecessor|one) .* (made|posed)|the key (insight|idea) is|the machine that|draw(s)? .* power from|under the hood|where .* meets" sections/*.tex

# ── M6: throat-clearing openers ──
grep -rnE "^(Moreover|Furthermore|Additionally|Notably|Importantly|Indeed|Ultimately|Crucially|In turn|That said)" sections/*.tex

# ── M5/M16: banned + pompous words ──
grep -rnoiE "\bnovel\b|\bsignificant\b|\bsubstantial\b|\bimpressive\b|\bpromising\b|\bcomprehensive\b|\brobust\b|\bpowerful\b|\bseamless|\bcrucial\b|\bparadigm\b|\bleverag|\butiliz|\bfinaliz|[a-z]+-oriented\b|\bfactor\b|\bfeature[ds]?\b|\bmeaningful\b|\binsightful\b|\bprestigious\b|\bpossess|\bcontact(s|ed|ing)?\b|\bcurrently\b|\bimpact(s|ed|ing)?\b" sections/*.tex

# ── M10: vague-mechanism / futurist hype verbs ──
grep -rnoiE "promises to|stands? to|is poised to|opens the door to|is set to|has the potential to|keeps .* from|stands? in the way|\bunlocks?\b" sections/*.tex

# ── M17: fancy / figurative verbs (inspect each; keep only precise domain jargon like "amortize") ──
grep -rnoiE "\b(pit(s|ted|ting)?|dispatch(es|ed|ing)?|chip(s|ped|ping)? (away )?at|marshal(s|led|ling)?|orchestrat(e|es|ed|ing)|wrangl(e|es|ed|ing)|harness(es|ed|ing)?|forge[sd]?|weav(e|es|ed|ing)|delv(e|es|ed|ing) into|usher(s|ed)? in|grappl(e|es|ed|ing) with|anew|afresh)\b" sections/*.tex

# ── M18: content-free openers ──
grep -rnE "In this (paper|section), we" sections/*.tex

# ── M12: needless words / wordiness ──
grep -rnoE "the fact that|the question (as to |of )?whether|as to whether|in order to|there is no doubt but|the reason .* is because|owing to the fact that|in a [a-z]+ manner|is a (subject|man|woman) (that|who)|in the last analysis|along these lines|in terms of|one of the most" sections/*.tex

# ── M13: weak qualifiers ──
grep -rnoiE "\b(rather|very|pretty|little|quite|somewhat|fairly|certainly)\b" sections/*.tex

# ── M14: coined adverbs / false ordinals ──
grep -rnoiE "\b(thusly|muchly|overly|firstly|secondly|thirdly)\b|[a-z]+wise\b" sections/*.tex

# ── M15: exclamation marks ──
grep -rn "!" sections/*.tex

# ── Part B: precision pairs (inspect for wrong member) ──
grep -rnoiE "\bcomprised of\b|\bdata is\b|different than|\bvery unique\b|\bdue to\b|\bless (than )?[0-9]" sections/*.tex

# ── Term/decomposition drift (see gate_semantic S8/S9): one name per concept ──
#   (a) For each canonical term in the project_context term-map, show every surface form + location,
#       then converge. Replace the list with the paper's real synonym clusters.
for t in "application workflow" "application behavior" "bottleneck conditions" "network conditions"; do
  echo "== $t =="; grep -rn "$t" sections/*.tex | grep -v '^\s*%'; done
#   (b) Decomposition cardinality: do all the "N <noun>" counts agree across sections?
grep -rnoE "\b(three|four|five|six|seven)\b (stages|concerns|axes|requirements|dimensions|systems)" sections/*.tex
```

在审查汇总里报告**逐类别的命中计数**。"干净"意味着每个命中都已修复或论证，**且计数已展示**。

> **【英文专属】** 整段脚本都是英文模式。它同时也是一份很好的**模板**：中文版需要把每条
> 规则的词表换成中文对应物，把被动语态检测换成中文的判断方式（更依赖读者判断，正则能做的
> 很有限）。
