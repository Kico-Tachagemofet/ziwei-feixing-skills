# Finding Schema (v3.0)

> 从 SKILL.md v3.3 外置。Stage 2 finding 输出必须遵循此模板。
> 见 SKILL.md Phase A-D 流程了解各 section 如何填写。

```markdown
## F-<TOPIC>-<NNN>

Topic: [topic name]
象核 Reference: [属于象核哪个点: 主锚/凶锚/禄随忌走/用]
Main Taiji: [宫名(干支)] — **必须等于 lens Main Taiji**
Chain Group: [涉及的飞化链群，非单条链] — 列出所有相关 edges
Confidence: high / medium / low / insufficient-source
Question Answered: [这条 finding 回答 topic 的哪个具体问题]
Topic Relevance: direct_topic_chain / context_chain / global_pattern

### Deep Audit
需要 deep? [是/否]
如是: [哪些 deep card] — [已读/未读]
Confidence cap: [high/medium/low]

### Source Packet Refs
- [RC-ID: 一句话说明这条 source 支持什么]

### Phase A: 结构读取

太极点: [宫名]([干支])
静态: [主星/旺度/生年四化/自化]

飞宫链群(本 finding 涉及的全部链):
  [宫A]干飞忌([星]→[宫B]) — 因果組: [ID，如有]
  [宫C]干飞禄([星]→[宫D]) — 因果組: [ID，如有]
  ...

理则检查:
  理则三: [有无本命宫忌冲命?]
  理则四: [有无同类用冲体?]
  理则五: [有无忌冲元太极?]
  理则六: [多重身份时，优先身份] — 本命>大运>流年

双象检查(理则七八):
  [逐个检查飞化落点有无生年/自化象]
  来源区分: [先天共存→走 sihua-benyi.md §6 / 飞化碰既有→走双象六组]

自化: [有无] → [四种变量哪种] → [应期方向]

### Phase B: Lens Index 引用/核验

> 以下字段从 Lens Packet Palace Pair Index 引用，不自行收集。缺项 → stop rerun lens。

化星: [星名] — **主司: [官方身份]** — 属性域: [核心取象面关键词]
落宫: [宫名] — 类型: [人/事/物位] — **地支位: [驿马位/桃花位/库位/普通]**（必填）
同宫: [双星组合名] — 组合取象: [关键词]（**落宫组合必读**）
辅星: [列出] — 信号: [驿马/桃花/煞/...] — [wtz]小星(如有): [X]
结构: [生年X / 自化X / 入B / 冲C]
Lens核验: [Packet complete / missing-index → rerun lens]

### Palace Substance (宫态实质)

凶锚宫 (如本 finding 涉及):
  宫名: [X] — 组合: [双星名] — 旺度: [X] — 煞: [X]
  凶度: [高/中/低]
  其出度禄权科的判读: [代价/逃生口/转化/补偿]

用宫 (如本 finding 涉及):
  宫名: [X] — 组合: [双星名] — 旺度: [X]
  Combo deep 生活画面: [从 deep card 提取的断事级描述]
  用-identification: [命主做这件事看起来像什么]

### Phase C: 象核驱动解读

**C.0 象核归属**: [主锚/凶锚/禄随忌走/用] — 回应象核: "[一句话]"

**C.1 宫态吉凶**: [吉/凶/混合]
  判据: 星=[X] + 旺度=[X] + 煞=[X] → [结论]
  注: [如果宫态吉凶与飞化方向矛盾，以宫态为 prior]

**C.2 词条交叉**:

  C.2a 词条全列:
    [星A]: 主司=[X] / 化气=[X] / 属性域=[...] / 通论=[...]
    [星B]: 主司=[X] / 化气=[X] / 属性域=[...] / 通论=[...]
    [象X]: 本义=[...] / 条件义=[...]
    deep card 宫位范例: [该星在当前宫位/宫类的原书范例，如有]

  C.2b 语义交叉:
    共振(重叠): [哪些词条跨星/象语义相近 → 放大信号]
    分层(不同): [哪些词条只属一方 → 不同面向]

  C.2c 画面合成:
    主干(从共振): [一句]
    层次(从分层): [一句]
    合成画面: [可被真人回答"准/不准"的生活判断]
    人位宫三角度(如太极点=六亲宫):
      关系面: [感情好坏/缘分深浅/互动模式]
      实际利益面: [对命主事业/名声/财务/外在面的影响]
      人物画像面: [对宫组合+飞化化星画对方气质]

### Interplay Map (交互图)

本 finding 涉及的宫位间关系:
  [宫A] --忌→ [宫B] (因果組 #N)
  [宫A] --禄→ [宫C] (因果組 #N)
  [宫D] --权→ [宫B] (碰撞: 忌+权)
解读: [一句话说明这个交互图意味着什么]

### Interpretive Kernel（给 render 层的接口）

Main Scene: [主取场景，人话，不含术语]
Secondary Scene: [次取场景，人话]
Do Not Render: [这条 finding 不能说什么——来自象核排除误读 + boundaries]

### Phase D: 本义校验 + 断语 — 过 sihua-benyi.md §0

本义检查:
  这里的 ___化，按本义是 ___
  取用的义 [___] 符合本义因为 ___
  常见误读 ___ 不适用，因为 ___

Finding Statement:
[一句 source-level 中文判断，不写 reader prose。]
[说命主实际体验到的事，不说飞化机制术语。]

Boundaries:
- 这条 finding 不能推出: [明确说不能推什么]
- 象核排除误读相关: [引用象核第5点]
- Chain Group 层级: [Primary = 核心结论 / Secondary = 辅助方向 / Background = 全局背景]
- Source gaps: [如有]
```
