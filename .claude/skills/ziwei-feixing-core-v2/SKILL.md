---
name: ziwei-feixing-core-v2
description: 紫微斗数飞星派核心 skill (纯许铨仁体系)。主模式=Stage 2 象核驱动 finding production；前置缺失时自动补跑 Step 0 + 调用 topic-lens/source-lookup。输入 chart-stage1 即可启动，缺失的中间产物会按顺序恢复。质量审计走 ziwei-finding-audit。
---

# 紫微斗数飞星派 Core v3.4

> v3.4 (2026-05-23) — 架构拆分: SKILL.md 收窄为 Stage 2 only; 核心纪律+质量检查移至 ziwei-finding-audit; Finding Schema 外置 rules/finding-schema.md
> v3.3 (2026-05-23) — 入/照/冲三层 + 双象气数组路由 + N象一星校准 + deep必读 + 四化回查

## 核心认识论

```
紫微适合: 全盘交易图 → 结构核/局 → topic 映射 → 断语

Finding 不能当思考单位，只能当输出单位。
真正的思考单位 = 结构核/象核/局。
```

**推导顺序 (不可跳步)**:
1. 全盘交易图 (Step 0: 48 edges factual layer)
2. 象核構建 (structure kernel: 主锚/凶锚/禄随忌走/用/排除误读)
3. Topic 映射 (象核 → topic question)
4. 断语输出 (finding = 象核的某个面向 + topic context)

## 防污染声明

本 skill 是纯许铨仁飞星派。四化表、理则、忌义分读、自化、应期全在这一个体系内。不能在飞星推导链里混入其他流派规则。

小星 addon (`ziwei-wangtingzhi-minor-stars-addon`) 遵循其 provenance-policy：`[wtz]` 信号只做 supporting/qualifier/background，不替换核心飞星链。

## 加载架构

| 层 | 何时 | 内容 |
|---|---|---|
| **Tier 1 core-always** | 每次启动 | `rules/core-always/` 全部（见下表） |
| Tier 1.5 session-warm | 进入断盘 | 十干四化表 + 宫骨架 + 星骨架 + 太极点表 + 生年×自化矩阵 |
| **Tier 2 短卡** | **开局全加载** | `source-cards/_bundles/tier2-short-cards.md` (88张合并) |
| Tier 3 topic-methods | topic_tag 命中 | `rules/topic-methods/` 专题操作流程 |
| **Tier 4 deep** | **Stage 2 进入即加载** | 太极点相关宫位 combo/star/flow deep = 必读项 |
| WTZ addon | Stage 2 有小星时 | 太极+对宫+落宫小星→wtz 短+deep |

Source-cards 路径: `E:\My Grimoire\Codex\紫微斗数\source-cards\`

### Tier 1 core-always 必读文件

| 文件 | 用途 |
|---|---|
| `feixing-minimal-grammar.md` | 判读语法 + 双象六组 + 入/照/冲 + 时序 |
| `sihua-minimal-semantics.md` | §0 门禁 + 四化本义 + 两组对待 |
| `ji-interpretation-guide.md` | 化忌分读速查 — 六内六外/入冲/主司/层级 |
| `self-transform-minimal.md` | 自化三类型 + 交易规则 |
| `shengnian-zihua-matrix.md` | 生年×自化 16 组矩阵 + 四种变量 |
| `palace-minimal-map.md` | 十二宫分类 |
| `source-router.md` | chart-facts → source-cards 映射 + combo dispatch |
| `pattern-audit-checklist.md` | 14 条 pattern 检查 |

## Pipeline 位置

本 skill = Stage 2c (象核構建 + Finding Production)。
上游: reader → Stage 1.5 → Step 0 → topic-lens → source-lookup → **本 skill**
下游: **ziwei-finding-audit** (Stage 2 质量审计) → Stage 3 composition (本 skill composition mode, 见 `rules/stage3-composition.md`) → ziwei-finding-audit (Stage 3 audit) → Stage 4 ziwei-render (必读 composition.md 作骨架)

## 职责

**做**: 先建象核再从象核产 finding。每条 finding 追溯到象核，带 chart row + source refs + confidence + boundaries。
**不做**: 不从单链产 finding / 不写 reader prose / 不做综合叙事 / 不读 subject-context / 不做质量审计(走 audit skill)

## 输入

| 文件 | 来源 | 必需 |
|---|---|---|
| chart-stage1.md | ziwei-reader | 是 |
| chart-facts.yaml | Stage 1.5 | 是 |
| cross-topic-structural.md | Stage 1.5 | 是 |
| sihua-transaction-map.md | Step 0 | 是 |
| **innate-body.md** | **Step 0.7** | **是** |
| Topic Lens Packet | ziwei-topic-lens | 是 |
| Source Packet | ziwei-source-lookup | 是 |
| subject-context.md | — | **禁读** |

### 前置缺失恢复 (不可跳过)

进入 Stage 2 前检查上述必需输入。缺失时**按顺序补跑**，不可跳过或绕过:

| 缺失 | 恢复动作 |
|---|---|
| chart-stage1.md | 停止。提示用户先调 ziwei-reader 起盘 |
| chart-facts.yaml / cross-topic-structural.md | **自行执行 Stage 1.5**: 读 chart-stage1，做全盘宫象星合参+chart-facts 萃取+cross-topic 结构分析。**不可绕过直接用 chart-stage1 代替。** |
| sihua-transaction-map.md | **自行执行 Step 0**: 读 chart-stage1 + chart-facts + `rules/step0-schema.md`，产 `sihua-transaction-map.md` |
| innate-body.md | **自行执行 Step 0.7**: 读 chart-stage1 + transaction-map + `rules/step07-innate-body.md`，产 `innate-body.md`。**命迁 combo deep 必读。** |
| Topic Lens Packet | **调用 ziwei-topic-lens**: 用 Skill tool invoke，传入 topic + chart-stage1 + transaction-map + chart-facts |
| Source Packet | **调用 ziwei-source-lookup**: 用 Skill tool invoke，传入 chart-facts + topic_tag |

恢复顺序固定: Stage 1.5 → Step 0 → **Step 0.7** → topic-lens → source-lookup → Stage 2c。不可乱序。

⚠️ Step 0 和 Step 0.7 由本 skill 自行执行。topic-lens 和 source-lookup 必须通过 Skill tool invoke 调用对应 skill，不可在本 skill 内直接读它们的 rules/ 子文件代替。

### Timing-aware Behavior

| Timing audit | 允许 |
|---|---|
| PASS | 结构 finding + 大运/流年 timing claims |
| PARTIAL | 结构 finding + broad 大运 background only |
| FAIL / 无 R1 | 结构 finding only，标 `uncalibrated` |

## Finding Schema

模板见 `rules/finding-schema.md`。Step 0 交易图模板见 `rules/step0-schema.md`。

## 生产硬约束 (做 finding 时必须遵守，不只是事后审计)

1. **象核先于 finding** — 没有象核不产 finding。象核=思考单位，finding=输出单位。
2. **交易图先于象核** — 没有 sihua-transaction-map.md 不建象核。
3. **Per-star aggregation 在太极点下做** — 飞宫四化 scope 随发射宫×太极点关系变。不能全局层聚合后搬进象核。
4. **凶锚禄权科不直读正面** — 先走代价/逃生口/转化/补偿四种判读。
5. **一条 finding 不从单链产** — 必须从象核某点产出，象核的点必涉多链。
6. **宫态优先于飞化方向** — 吉凶从星+旺度+煞判，飞化方向是叠加不是来源。
7. **词条交叉不可跳步** — C.2a→C.2b→C.2c 依次。读了 deep card 直接拍结论=跳步。
8. **太极点选择不可自行代劳** — 太极点由 `ziwei-topic-lens` 产出。Core 缺 Lens Packet 时必须 invoke lens skill，不可自行选太极后继续（Gate 1 硬门禁）。

---

## Stage 2: 象核驱动 Finding Production

### 入口门禁

#### Gate 0: Topic 有效性

禁止进入: "生年四化全局/资源大局/全盘大局/性格总论/命主概览" 及 ≥3 宫 Primary 的 topic。

#### Gate 0.5: Topic Scope 单一性

一个 topic 一个 scope。父母关系/起源结构/权威互动/婚姻关系/恋爱模式/财运/事业方向 各自独立。混合 → 回退 topic-lens 拆分。

#### Gate 1: Taiji Lock (硬门禁 — 无 Lens Packet 不进 Stage 2)

⚠️ **Core 禁止自行选择太极点。** 太极点选择是 `ziwei-topic-lens` 的专属职责。

- Lens Packet **不存在** → **STOP**。必须先 Skill tool invoke `ziwei-topic-lens`，拿到 Packet 后才能继续。不可"先自行判断太极点，等下再补 lens"。
- Lens Packet **存在** → Main Taiji 必须等于 lens Main Taiji。次太极最高 Secondary，全局最高 Background。Core 不可覆盖或修改 lens 给出的太极点。

#### Gate 2: Confidence Tier

| Tier | 条件 |
|---|---|
| high | deep card ref + chart 双重支持 |
| medium | 短卡 ref + chart，或 deep 但 chart 部分满足 |
| low | source 模糊或通论级 |
| insufficient-source | 无 RC ref 或 deep gate 未通过 → 不产 finding |

#### Gate 3: 来因宫=年干门禁

来因宫飞四化 = 体之体，不作后天交易 Primary。来因宫本身的星/象/态 = 结构事实，不受此限。

---

### Step 0.5: 引用 Lens Palace Index (Lens Packet 强制校验)

⚠️ Lens Packet 必须有宫对索引。缺 Lens Packet 或 Packet 无宫对索引 → **STOP，Skill tool invoke `ziwei-topic-lens` 补跑**。Core 不自行补做信号收集、不自行选太极点、不自行列宫对。

### Step 1: 象核構建

> 必须在任何 finding 之前完成。没有象核，不产 finding。

#### 象核五点

```markdown
## 象核: [topic]
### 1. 主锚 — 太极宫飞化 ↔ scope-关键宫的 Layer 1 吉象 (定吉凶有无的基调)
### 2. 凶锚 — 太极宫飞化 ↔ scope-关键宫的 Layer 1 凶象 (scope-dependent outcome, 不是 static 标签)
### 3. 禄随忌走 — 忌端+禄端+方向
### 4. 用 — 实际产出/行动力(用宫必读 combo deep)
### 5. 排除误读 — 盘面反驳什么常见误读
```

#### 象核構建流程

**1a** 从交易图提取 topic 相关 edges
**1b** 太极点下 per-star aggregation (标发射宫角色+scope)
  - N象一星: 结构常见≠特异。天梁不化忌/紫微不化忌才特异。看碰撞内容不看数量。
**1c** 聚合 → 构建五点

#### 凶锚识别（应用 feixing-minimal-grammar §宫位吉凶判读优先级）

⚠️ "凶锚" 是 **scope-dependent Layer 1 outcome** 标签，不是 static 属性。识别步骤:

1. 列 topic scope 的关键宫（命 + scope-specific 人位/事位，见 feixing-minimal-grammar 表）
2. 查太极宫飞化 ↔ 这些关键宫的交互
3. 凶象（忌冲命/忌冲人位/忌入命/出忌结构）= 凶锚
4. 同一条飞化在不同 scope 角色可能不同，必须 scope-aware

⚠️ **旧规则消解**: "凶锚宫飞出的禄权科先判代价/逃生口/转化/补偿" 已 DEPRECATED 2026-05-24。原因：飞化对命有禄科照入的宫从一开始就不是凶宫，不需要 ad-hoc 框架解释吉象。

### Step 2: 象核 → Finding 映射

一象核 → 1-3 条 finding。每条必须指向象核某点。

---

### Phase A-D 流程

**Phase A: 结构读取**

a. chart-stage1 对应行
b. Source Packet snippets
c. 飞化链群(理则一二)
d. 双象检查(先天共存→矩阵; 外来碰→双象六组)
d2. **先天气数组路由**: topic 有专题方法时，以太极立太极定气数组(权忌/禄忌/科忌组) → 路由到 topic-method 专属方法
d3. **入/照/冲强制检查**: 禄权科入A=照A对宫。忌入A=冲A对宫。三层全记录。禄科照命不可漏。
e. 理则三/四/五 + 理则六(小归大)
f. 自化 + 四种变量

**Phase B: Lens Index 引用/核验**

引用 Lens Packet 信号。缺 → stop rerun lens。不在 core 内补做信号收集。

**Phase B.5: Deep Read Gate**

⚠️ **四化交易 deep 必读**: 太极点飞四化每个落宫+照宫 → 读 `flow/deep/into-{slug}.full.md`。未读 = 不可 Primary。
- b5-FLOW: 忌落宫 / 禄落宫 / 照宫 / 命飞入太极 / 碰撞落宫
- combo/star deep: 太极+对宫+落宫组合+主星 = 必读
- wtz addon: 太极+对宫+落宫小星 = 必读。检查太极点轴+关键轴的跨宫成对小星（三台↔八座、龙池↔凤阁等），命中则走 composition-grammar 场景合成。
- 生年+自化同宫 → 矩阵(不走双象六组)

**Phase C: 象核驱动解读**

```
C.0: 象核归属
C.1: 宫态吉凶 (星+旺度+煞判，不从"有忌=凶"判)
C.2: 词条交叉 (三步不可跳):
  C.2a 词条全列 (含 deep card 宫位范例)
  C.2b 语义交叉 (共振+分层)
  C.2c 画面合成 (人位宫须三角度: 关系面+利益面+画像面)
```

**Phase D: 本义校验 + 断语**

过 sihua-benyi.md §0 门禁 → finding statement (一句人话) → boundaries

---

### Phase C 教学范例 (脱敏)

> 某盘夫妻宫空宫(双驿马位)，有视同自化忌+左辅+云消雾散组合命中+双向科。以下展示 C.2 三步如何操作:

**C.2a 词条全列:**

```
左辅: 主司=助星 / 化气=善 / 属性域=灵巧,随和,再一次 / 宫位范例(夫妻)=先天缺憾,配偶有过去
视同自化忌: 本义=涌动力+收束 / 特性=一波未平一波又起,新缘更替
驿马位(双): 变迁,奔波,不固定
科(双向): 本义=延续,旧缘,风度 / 命→太极+太极→命=互送延续
云消雾散(天同+文昌+忌): 分离信号(六亲宫=强, 此处六事宫=参考级)
```

**C.2b 语义交叉:**

```
共振: 变动×4 — 驿马(变迁)+视同自化忌(涌动)+左辅(再一次)+云消雾散(分离)
      延续×2 — 双向科(延续)+左辅(再一次=接续)
分层: 变动(关系结构) vs 延续(命主态度) vs 缺憾(左辅独有)
```

**C.2c 画面合成:**

主干(共振): 关系核心=**变动**——不断有新变化涌起, 经历断续, 形态不固定。
层次(分层): 命主态度=**愿意延续**——双向科=彼此念旧, 左辅=接受缺憾重新开始。
合成: **动荡中的坚持——关系形态一直在变, 命主念旧的心不变。**

> ⚠️ 注意: C.2a 列了 5 个信号源的完整词条; C.2b 找到 4 重共振+2 重延续+1 独有; C.2c 从共振产主干、从分层产层次。**跳过 C.2a/C.2b 直接写 C.2c = 跳步，禁止。**

---

### 四化断语前强制回查 (飞星派最核心门禁)

⚠️ **四化是飞星派最核心的操作系统。任何涉及四化的断语之前必须回查——越熟越要查，越觉得"我知道"越要对照原文。**

- [ ] **禄=缘起/聚缘** — 不是现成财富。自化禄=新缘涌起。宫象合参: 禄在疾厄=脾气好/体壮，不是抽象"缘起"
- [ ] **权=缘变/天意/无奈** — 不是权力(power)。自化权=天意突变有应期
- [ ] **科=缘续/延续/旧缘** — 不只贵人。自化科=平顺延续
- [ ] **忌=缘终/收束/归藏** — 不只是坏。忌入六内可主得。自化忌=变动收束
- [ ] **入/照/冲三层** — 禄权科照对宫(正面)，忌冲对宫(负面)。三层全记录
- [ ] **视同自化=宫干化星在对宫 → 不断缘起/新缘随时间更替** — 不是"回收/自给自足"
- [ ] **生年+自化同宫 → 矩阵不走双象六组**
- [ ] **先天气数组 → topic-method 路由**
- [ ] **四化取象必须宫象合参** — 通论义过宫位才成画面，不可直接搬

---

### First-invocation Flow

```
前置: Gate 0/0.5/1/3 检查 + chart/map/lens/source 存在确认

Step 0.5: 引用 Lens Palace Index
Step 1: 象核構建 (1a edges → 1b per-star → 1c 五点)
Step 2: 象核 → Finding 映射 (1-3 条)
Step 3: Per-finding Phase A → B → B.5 → C → D + 四化强制回查
Step 4: 输出 topic-findings/<slug>.md → 交 ziwei-finding-audit 审计
```
