# 紫微斗数飞星派 Pipeline 规范

> v3.2 (2026-05-22)
> 全流程规范：从排盘到 deliverable
> v1.0 来源：2026-05-20 pipeline 复盘六点校准
> v2.0 变更：chart-facts 萃取 + source-loader 确定性加载 + pattern-audit 层 + 五层加载架构
> v2.1 变更：R1 后移至宫象星合参之后 + Phase B.5 Deep Read Gate + 生年×自化矩阵提升 Tier 1.5
> v2.2 变更：**短卡全部开局加载** — Tier 2=短卡(开局), Tier 4=deep(按需); source-lookup 收窄为 deep 决策 [2026-05-21 校准: 首案例 漏读短卡+deep导致断语偏]
> v2.3 变更：R1 改为 debug-only 默认关闭；Stage 2 禁读 R1；Primary finding 强制 topic relevance + deep priority gate。
> **v3.0 变更：架构重写 — 象核 (structure kernel) 取代 per-chain finding 作为思考单位；新增 Step 0 (飞化交易象全列); Phase C 从五域共振改为象核驱动; 凶锚宫/来因宫=年干/topic scope 三道硬门禁 [2026-05-22 金标准测试]**
> **v3.1 变更：Phase C 词条交叉重写(删 C.2用-first/C.3方向叠加/C.4星性实质 → C.2词条交叉三步); Deep Gate b5f 同步 C.2a; 命迁线条件带入(收窄触发); feixing-framework.md 旧 Phase C 标 DEPRECATED 不可执行 [2026-05-22 case-01 交友宫断偏复盘+review]**

---

## Pipeline 总览

```
Stage 1:   排盘结构化 ─── ziwei-reader
  ↓
Stage 1.5: 全盘结构分析 + chart-facts 萃取 [+ R1 可选] ─── ziwei-feixing-core-v2 (structural mode)
  ↓
Step 0:    飞化交易象全列 ─── ziwei-feixing-core-v2 (产 sihua-transaction-map.md，纯 factual)
  ↓
Step 0.7:  先天体 ─── ziwei-feixing-core-v2 (产 innate-body.md: 命迁线deep+生年四化+来因宫+结构速写)
  ↓                                            ⚠️ 所有 topic 的共享输入，必须在 Stage 2 前完成
Stage 2:   Per-topic Finding Production (character 先行，其余可并行)
  ├─ 2a: Topic Lens ─── ziwei-topic-lens → Topic Lens Packet v3
  ├─ 2b: Source Loading ─── ziwei-source-lookup
  ├─ 2b.5: Pattern Audit ─── pattern-audit-checklist
  └─ 2c: 象核構建 + Finding ─── ziwei-feixing-core-v2 (引用 innate-body.md)
  ↓
Stage 3:   Cross-topic Composite ─── ziwei-feixing-core-v2 (composite mode)
  ↓
Stage 4:   Render ─── ziwei-render (纯翻译层，不做技术分析)
```

> ⚠️ Stage 3 在 core（技术层）不在 render（翻译层）。
> render context 混入技术分析 → 术语污染 → 散文退化。
> 希腊化 render v1.0→v1.1 的教训。

---

## 执行模式

| 模式 | 触发词 | 描述 |
|---|---|---|
| **生产模式** | "全自动跑" / "跑全流程" / 默认 | 1→1.5→Step 0→2(并行所有topic)→3→4，不跑 R1、不停点 |
| **培训模式** | "分步跑" / "一步一步" / "我来校准" | 每个 Stage 完成后暂停等校准 |

培训模式是教学/校准用的，不是固定流程。培训期间的校准结果先写入 case-calibration/notes；只有多次复现且语境明确时，才写回 skill rules。

---

## Stage 1: 排盘结构化

**Skill**: `ziwei-reader`

| 输入 | 输出 |
|---|---|
| 盘面数据（文墨天机/截图/手动） | `chart-stage1.md` — 结构化盘面 |
| | `chart-stage1-audit.md` — 审计报告 |
| | `subject-context.md` — 盘主背景（Stage 2 禁读） |

Reader 只做结构化 + 审计 + 隔离。R1 不在此阶段。

---

## Stage 1.5: 全盘结构分析 + chart-facts 萃取

**Skill**: `ziwei-feixing-core-v2` (structural mode)
**触发词**: "全盘分析" / "structural" / "Stage 1.5"
**前置**: chart-stage1 + audit PASS

### 1.5a: 全盘结构分析 + chart-facts 萃取

从全盘结构出发，不绑 topic。**R1 默认关闭**，只在 debug/training 明确要求时运行。

**Step 1-2: 飞化结构（纯代数）**

1. **生年四化全局定位** — 禄权科忌各落哪宫，先记录落宫不写断语
2. **来因宫方向** — 来因宫 + 对宫(生态空间) + 第六宫(格局鉴定)

**Step 3: 宫象星合参（短卡已开局加载，直接引用）**

3. **生年四化宫象星合参** — 对生年四化所在的 4 个宫位，引用已加载的:
   - `palace/{slug}.md`（宫义 + 四化含义）
   - `combo/{pair}.md`（组合取象——**组合先于单星**）
   - `star/{star}.md`（组合不足时补充）
   - 有生年+自化同宫 → 引用 `session-warm/shengnian-zihua-matrix.md`

   完成后，为每个生年四化写**宫象星定象**（宫+组合+化=一句人话）。

**Step 4-8: 理则审计 + 结构模式**

4. **理则三全盘审计** — 逐查十一宫: 宫干化忌是否冲命？记录所有触发 + 汇聚模式
5. **理则四审计** — 当前大运同名宫忌冲本命同名宫？
6. **理则五审计** — 大运/流年忌冲本命命宫？
7. **结构模式识别** — 忌汇聚？topic 聚类？吉凶底色？
8. **自化/视同自化全盘标记** — 变动信号 + 应期方向

**Step 9: chart-facts 萃取**

9. **chart-facts.yaml** — 从上述分析输出结构化数据

**输出**: `cross-topic-structural.md` + `chart-facts.yaml`

#### chart_facts.yaml schema

```yaml
native_transforms:
  - {transform: 忌, star: 天机, palace: 官禄}
  - {transform: 禄, star: 太阳, palace: 命}
  # ... 禄权科忌各一条
self_transforms:
  - {palace: 夫妻, transform: 忌, star: 贪狼}
flying_transforms:
  - {source_palace: 夫妻, transform: 忌, target_palace: 命, star: 太阴}
palace_stars:
  - {palace: 命, stars: [紫微, 贪狼]}
  # ... 每宫的主星列表
time_layers: [natal, major_cycle]  # 涉及哪些时间层
major_cycle_palace: 子女  # 大限命宫所在本命宫位（P03/P12 需要）
annual_palace: 财帛      # 流年命宫所在本命宫位（P03 需要，如涉及流年）
```

chart_facts 是 **确定性抽取**（查表/读盘面），不含判断。它供下游 stage 做加载决策，不供断语。

### 1.5b: R1 验前事（可选 / debug-only — 默认不跑）

> ⚠️ R1 默认关闭。R1 的断语会锚定后续 Stage 2 的 Phase C 推导——
> 模型在 Phase C 取象时会锚定 R1 已写过的结论，丧失独立推导能力。
> 
> **启用条件**: 用户明确说 "跑 R1" / "做验前事" / "training mode"
> **禁用条件**: 默认 / "全自动跑" / "生产模式"

如启用，从全盘结构分析挑 5 条高信号 claim。**不绑 topic。**

Claim 来源优先级：
1. 生年四化**宫象星定象**（Step 3 产出，有组合 grounding）→ 验人生核心方向
2. 理则三/四/五触发 → 验压力/冲击方向（理则=必凶，最容易验证）
3. 大运切入感 → 验当前十年主题
4. 自化触发 + **生年×自化矩阵**（Tier 1.5 已加载）→ 验变动模式
5. 断事级星性取象（来自 combo/star 短卡）→ 验质感

R1 = 飞化结构 + **星性/组合取象** + 理则 的综合。每条 claim 必须:
- 过 sihua-benyi.md §0 门禁
- 用人话写断语，术语挪到 `> 推导:` 行
- 有 source ref (至少标 combo/star 短卡来源)

**输出**: `pre-validation-R1.md`（标 `mode: debug/training`）
**停点**: 等盘主回答 → 记录校准 → 继续 Stage 2
**Stage 2 隔离**: R1 内容与 subject-context 同等隔离，Stage 2 禁读

---

## Step 0: 飞化交易象全列 (Stage 1.5 → Stage 2 桥梁)

**Skill**: `ziwei-feixing-core-v2` (Step 0 mode)
**前置**: Stage 1.5 完成 (cross-topic-structural.md + chart-facts.yaml)
**性质**: 纯 factual layer，等同于占星 aspect table。不做解读，只做结构记录。
**输出**: `sihua-transaction-map.md`

产出全盘飞化交易图: 48 edges + collision index + hub stats + 因果組 + 格局 candidates + 生年四化 overlay。
详见 `rules/step0-schema.md`。

---

## Step 0.7: 先天体 (Innate Body)

**Skill**: `ziwei-feixing-core-v2` (innate-body mode)
**前置**: Step 0 完成 (sihua-transaction-map.md)
**性质**: 半结构化描述层。读命迁 combo deep，生年四化落宫读短卡。描述性，不做断语。
**输出**: `innate-body.md`

先天体是**所有 topic 的共享输入**。回答"这个人先天带了什么"，不回答"这个人怎么样"。
产出: 命迁线画面(combo deep 必读) + 生年四化4落宫资源盘点(combo短卡+宫象合参) + 来因宫结构 + 结构速写(从交易图引用) + open threads。
详见 `rules/step07-innate-body.md`。

⚠️ **Step 0.7 必须在所有 Stage 2 topic 之前完成。** 后续 topic 引用 innate-body.md 的命迁线和生年四化，不重做。

---

## Stage 2: 象核驱动 Finding Production

> v3.0 变更: 不再 per-chain 产 finding。先建象核 (structure kernel)，再从象核产 finding。
> 思考单位 = 象核；输出单位 = finding。一条 finding 不能从单链产出。

四步串联。多 topic 可并行，但每个 topic 内部 2a→2b→2b.5→2c 必须串联。

### 2a: Topic Lens (v3.0+)

**Skill**: `ziwei-topic-lens`
**输入**: chart-stage1 + sihua-transaction-map + chart-facts.yaml + cross-topic-structural + topic name
**输出**: Topic Lens Packet v3（含 Palace Pair Index + 命主↔太极点交互 + 变动信号汇总 + topic_tag + source queries）

> v3.0 变更: Lens 从轻量 tag router 升级为完整索引层。输出包含宫对索引(以轴为最小单位)、命主交互双向关系、重要飞化落点、变动信号计数。Core Phase B 不再自行收集信号，只引用 Lens Packet。

`topic_tag` 值域: marriage / career / health / traffic / property / general
→ 查 `core-always/source-router.md` 映射表加载对应 `rules/topic-methods/*.md`（不可直拼 `{topic_tag}.md`，因 career→career-wealth.md 等不对称）

### 2b: Deep Decision + Coverage Audit

**Skill**: `ziwei-source-lookup`
**输入**: chart-facts.yaml + Topic Lens Packet v3 (含 topic_tag + source queries)
**前提**: 所有短卡已在开局通过 `_bundles/tier2-short-cards.md` 加载（Tier 2），不需要本步骤来加载短卡。

**本步骤职责**（v2.2 收窄）:
1. **Deep 决策**: 按 chart-facts + topic lens，检查哪些短卡的 needs_deep_when 被命中 → 列出需要追读的 deep 卡清单
2. **Coverage Audit**: 短卡覆盖检查 — chart_facts 有值但无对应短卡的字段标 gap
3. **Extractions Fallback**: 短卡 gap → 回退搜索 `extractions/` 原始蒸馏
4. **Deep Priority Gate**: 若 combo/star 用作 Primary、人物画像、主断语、吉凶修正，必须追读 combo deep + 相关 star deep；否则标 insufficient-source
5. **Provenance**: 分析框架纯许铨仁；紫云尚未接入，顾绍贤/qintian/nanpai 只做参考标注，不进推导链
6. **禁引盘主本案例**

**不再做**: 加载短卡（已在开局全加载）

**输出**: `topic-findings/<slug>-source.md`（含 deep_reads_needed[] + gaps[] + coverage audit）

### 2b.5: Pattern Audit

**不需要 invoke skill**，由 Stage 2c 的 finding agent 在进入 finding 前执行。
**输入**: chart-facts.yaml + Topic Lens Packet v3（P14 需 topic_tag / query flags）
**操作**: 读 `core-always/pattern-audit-checklist.md`，逐条检查 chart_facts + query flags 是否命中 14 个 pattern。
**输出**: `triggered_patterns` 列表 → 加载对应 `pattern-rules/*.md`

### 2c: 象核構建 + Finding Production

**Skill**: `ziwei-feixing-core-v2` (topic finding mode)
**输入**:
- chart-stage1 + lens packet
- **sihua-transaction-map.md** (Step 0 产出，必须存在)
- 已开局加载的短卡 bundle + source packet（2b 给出的 deep_reads_needed、gaps、coverage audit）
- topic-methods（从 Lens Packet topic_tag 触发的专题流程）
- pattern-rules（从 2b.5 triggered_patterns 加载的结构规则）

**Topic Palace Index 先行 (v3.2 新增)**: 进入象核之前，必须对本 topic 涉及的所有宫位做一次性完整信号盘点 (Topic Palace Index)。索引层与推演层分离——索引只列事实不判断，象核和 finding 只引用索引不重复收集。索引完成后才进象核。

**象核先行**: 进入 finding 之前，必须先从交易图+Palace Index构建象核 (structure kernel)。流程: Step 0.5 Topic Palace Index → Step 1a 提取相关 edges → Step 1b 太极点下 per-star aggregation（飞宫四化 scope 随发射宫×太极点关系变，不能在全局层聚合）→ Step 1c 构建象核五点。没有象核不产 finding。
**凶锚宫硬门禁**: 凶锚宫发出的禄权科，不直读正面供给。必须先判: 代价/逃生口/转化/补偿。
**来因宫=年干门禁**: 来因宫飞四化=生年四化时为体之体，飞四化不作后天交易 Primary；但来因宫本身的宫态/生年象/自化象可作 native body anchor Primary。
**Topic Scope 单一门禁**: 一个 topic 一个 scope，不混合。
**Phase B.5 Deep Read Gate**: 象核中用宫/凶锚强制读 combo deep；其他按 needs_deep_when 命中读。
**Topic Relevance Gate**: 每条 finding 必须写明 `question_answered` 与 `topic_relevance`。只有 direct_topic_chain 可作 Primary。

**输出**: `topic-findings/<slug>.md` (含象核 + findings)

### 并行规则

- **Step 0 必须在所有 Stage 2 之前完成**（交易图是共享输入）
- 多 topic 可并行 Stage 2（共享同一个 sihua-transaction-map.md）
- 每 topic 内部 2a→2b→2b.5→2c 串联
- 每个并行 agent 必须自己 invoke skill
- 完成后 git status/diff 自验证（subagent verify 铁律）

---

## Stage 3: Cross-topic Composite

**Skill**: `ziwei-feixing-core-v2` (composite mode)
**触发词**: "composite" / "cross-topic" / "Stage 3"
**对标**: 希腊化 natal-core Stage 3 chart-finding-index consolidation

> ⚠️ Composite 放在 core（技术层）而不是 render（翻译层）。
> 原因: render context 混入技术分析 → 术语污染 → 散文变"chart fact 换中文马甲"。
> 这是希腊化 render v1.0→v1.1 的教训（case-09 character 段 ~60% context 是术语）。

### 3a: 跨 topic 象核汇总
- 各 topic 象核的主锚/凶锚/用是否重叠或冲突？
- 同一因果組在不同 topic 的不同面向
- 理则三/四/五全盘触发汇总（哪些 topic 独立发现了同一条？）
- 同一飞化链在不同 topic 的不同角度
- `cross-topic-structural.md` 中结构模式在 findings 中的落实度

### 3b: 矛盾检查
- 不同 topic 对同一结构的断语方向是否矛盾
- Confidence 级别是否一致
- 星性取象一致性（不能 career 说"变动" spiritual 说"计划"）

### 3c: 全局叙事线索
- 跨 topic 底色/主线（如"迁移=压力汇聚点"）
- 大运/流年跨 topic 影响
- 开场/收尾全局叙事素材

**输出**: `cross-topic-notes.md`

---

## Stage 4: Render

**Skill**: `ziwei-render` (render mode)
**输入**: topic-findings/ + cross-topic-notes.md。`pre-validation-R1.md` 默认禁读；仅在 debug/training 复盘报告中可选读取 aggregate，不作 render 断语依据。
**输出**: reader/ + 可选 report.html

---

## 铁律

### 1. Skill Invoke 防绕行

每个 Stage 必须通过 Skill tool invoke 对应 skill。

| ❌ 禁止 | ✓ 正确 |
|---|---|
| subagent 直接读 skill 的 rules/ 子文件 | 主 agent invoke skill → 产出 → 分发 |
| 手动按 SKILL.md 逻辑跑而不 invoke | 每个 subagent 自己 invoke skill |
| 把 SKILL.md 内容粘贴给 subagent | skill 入口检查 + 版本控制完整走 |

**原因**: 直接读 rules/ = 绕过 SKILL.md 入口检查 + 版本控制。case-13 Stage 2/3A 已发生 deviation。

### 2. 流派防污染

理则/四化表/忌义分读/自化规则 = 100% 许铨仁。星性取象 = 当前纯许铨仁。紫云系列蒸馏尚未接入，接入后须标 [ziyun] 且冲突时许铨仁赢。

### 3. Source 禁引盘主本案例

盘主自己的案例分析（如 Z01）不能当 source。

### 4. Stage 2 禁读 subject-context

盲审纪律。subject-context.md 只在 Stage 4 render 可读。

### 5. 并行纪律

Stage 2 多 topic 可并行。每 topic 内部 2a→2b→2c 串联。每个并行 agent 自己 invoke skill。完成后 git status/diff 自验证。
