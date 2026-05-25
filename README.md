# 紫微飞星 Skills for Claude Code

紫微斗数飞星派分析 pipeline，以 Claude Code skill 形式运行。

## 体系

**核心框架**: 许铨仁飞星派。四化表、理则、忌义分读、自化、应期等核心推导链走许铨仁体系。

**小星系统**: 王亭之《安星法及推断实例》。辅佐煞杂曜取象、场景合成、安星法校验走王亭之体系，作为飞星核心的 addon。

**其他借鉴**: 部分三合派小技法（宫位取象层面），但不进入飞化推导链。飞化链内部保持单一流派纯净度。

**核心认识论**: 象核 (structure kernel) 是思考单位，finding 是输出单位。先建交易图，再建象核，最后从象核产断语。不做 per-chain finding。

## Pipeline

```
Stage 1:    排盘结构化 ─── ziwei-reader
  ↓
Stage 1.5:  全盘结构分析 + chart-facts ─── ziwei-feixing-core-v2
  ↓
Step 0:     飞化交易象全列 (48 edges factual layer)
  ↓
Step 0.7:   先天体 (命迁线 deep + 生年四化资源盘点)
  ↓
Stage 2:    Per-topic Finding Production
  ├─ Topic Lens ─── ziwei-topic-lens
  ├─ Source Loading ─── ziwei-source-lookup
  └─ 象核 + Finding ─── ziwei-feixing-core-v2
  ↓
Audit A:    Stage 2 finding audit ─── ziwei-finding-audit (A1-A4 + B1-B6)
  ↓ (FAIL 必修, 闭环)
Stage 3:    Cross-topic Composition ─── ziwei-feixing-core-v2 (composition mode)
              产出 composition.md (8 段标准结构)
  ↓
Audit B:    Stage 3 composition audit ─── ziwei-finding-audit (A1-A4 + B1-B4)
  ↓ (FAIL 必修, 闭环)
Stage 4:    Render ─── ziwei-render (必读 composition.md 作骨架)
  ↓
Audit R:    Stage 4 render audit ─── ziwei-finding-audit (R1-R12 量化基准)
              (每个 reader 文件产出后跑)
```

**三层 audit 闭环**: Stage 2/3/4 每个 stage 产出后都必须跑对应 audit, FAIL 必修不允许 force pass。

## Skills 一览

| Skill | 职责 |
|---|---|
| **ziwei-warm** ⚡ | Session warm-up。一次性读 68 个 deep card 到 context，后续 skill 不重复读。**详见 [⚠️ 模型要求](#-ziwei-warm-模型要求)** |
| **ziwei-feixing-core-v2** | 核心引擎。象核构建 + finding production + 先天体 + 交易图 + Stage 3 composition。前置缺失自动恢复。 |
| **ziwei-reader** | 盘面结构化。文墨天机/截图/手动输入 → chart-stage1.md + audit |
| **ziwei-topic-lens** | 太极点选择 + 宫对信号索引。含 character/sexuality/occult-spirituality/路人粉丝 等 topic reference |
| **ziwei-source-lookup** | Source-card deep 决策 + coverage audit |
| **ziwei-render** | 中文散文渲染（纯翻译层，不做技术分析）。v1.5+ 强制详细度基准 + Step 7 自查 |
| **ziwei-finding-audit** | 质量审计（检察官模式）。**三层 A+B+R**：Stage 2 finding / Stage 3 composition / Stage 4 render，不做辩护 |
| **ziwei-wangtingzhi-minor-stars-addon** | 王亭之小星 addon。辅佐煞杂曜 + 场景合成 + 安星法 |

## 安装

1. 将 `.claude/skills/` 下的 8 个目录复制到你的 Claude Code 项目的 `.claude/skills/` 中
2. 将 `CLAUDE.md` 的内容追加到你项目的 `CLAUDE.md`
3. 配置 source-cards 路径（见下方）

## ⚠️ ziwei-warm 模型要求

`ziwei-warm` 一次性加载 **68 个 deep card** 到 session context（包括 24 主星+辅星、13 宫位、22 王亭之小星、7 飞星骨架、2 render 规则）。

**实测能 handle 的模型**:
- ✅ **1m token 模型 (Opus 4.6 等)** — 实测可以提升表现，star/combo 原书定性进入 context 后 render 不再是 cookbook 级
- ❌ **200k 模型** — 大概率完全 handle 不了（68 个 deep card 全部加载会爆 context 或严重压缩，反而劣化）
- ❓ **其他模型 (Sonnet/Haiku 等)** — 未实测，自行评估

**注意**: 即使在 1m 模型上，目前很多 pipeline 表现仍然受限于模型的注意力分配和算力调度，而非规则本身。`ziwei-warm` 是缓解措施之一（解决"LLM 偷懒不读 deep card"问题），但不是万能。

## Source Cards (已包含)

本 repo 包含 **source-cards/**（许铨仁飞星体系的结构化笔记），分为短卡 (88 张 bundle) 和深卡 (deep)，共 190 个文件。

```
source-cards/
├── palace/     (12 宫 + general) + deep/
├── star/       (14 主星 + 辅星) + deep/
├── combo/      (21 张双星组合) + deep/
├── sihua/      (9 张四化系统) + deep/
├── flow/       (12 落宫) + deep/
├── rules/      (理则/体用/宫星象) + deep/
├── timing/     (大运/流年/应期) + deep/
├── reference/  (历法/排盘/干支)
└── _bundles/tier2-short-cards.md  (88 张短卡合并)
```

如果你的项目目录结构与本 repo 不同，需在以下文件中调整 `source-cards/` 路径:
- `ziwei-feixing-core-v2/SKILL.md`
- `ziwei-source-lookup/SKILL.md`
- `ziwei-warm/SKILL.md` (warm-up 也要读这些路径)

王亭之 addon 的 source-cards 已包含在 repo 中，运行时不需要原书。

## 关键设计决策

### 象核 (Structure Kernel)

每个 topic 进入 Stage 2 后，先建象核五点:
1. **主锚** — 哪个宫/象设定基调
2. **Layer 1 outcome (前身: 凶锚)** — 见下方 "Layer 1/2/3 优先级"。某宫是否"凶"是 **scope-dependent Layer 1 outcome**，不是静态属性
3. **禄随忌走** — 忌端+禄端+方向（因果组）
4. **用** — 实际产出（combo deep 必读）
5. **排除误读** — 盘面反驳什么常见误读

### Layer 1/2/3 宫位吉凶判读优先级 (新方法论)

**后天交易为第一要素，先天象不独立判吉凶。**

| Layer | 内容 | 优先级 |
|---|---|---|
| **Layer 1** | 后天交易（太极宫↔scope-相关关键宫的飞化交互） | **第一要素，定吉凶有无** |
| **Layer 2** | 先天结构（生年忌/自化/双象气数组等） | 按 Layer 1 outcome 调整读法 |
| **Layer 3** | 应期（流年/大运激活时机） | Layer 1+2 定基调后看 timing |

**取舍规则**:
- Layer 1 明显吉 → 先天凶象降级为形式/隐忧，不反驳后天的吉
- Layer 1 明显凶 → 先天吉象可作缓冲，但不抵消凶
- Layer 1 无明显象 → Layer 2 独立成断
- 命↔太极零互飞 → 该宫对命主间接影响，结论降级

**"凶锚/吉锚" 是 scope-dependent Layer 1 outcome 标签，不是 static 属性**。同一条飞化在不同 topic 角色不同（如夫忌冲子女在 partnership=配偶子女缘薄，在 property=background）。

**旧框架已消解**: "凶锚宫飞出的禄权科先判代价/逃生口/转化/补偿" 在新规则下消解——飞化对命有禄科照入的宫从一开始就不是凶宫。

### 先天体 (Innate Body)

Step 0.7 产出 `innate-body.md`，是所有 topic 的共享输入:
- 命迁线 combo deep 必读
- 生年四化 4 落宫资源盘点（combo 短卡 + 宫象合参）
- 来因宫结构
- 结构速写 + open threads

### Stage 3 Composition (8 段标准结构)

Stage 2 所有 topic findings 完成后，**必产** `composition.md` 给 Stage 4 render 作骨架。8 段:

1. **§1 全盘一句话** — condensed synthesis，不术语
2. **§2 paradox 主锚** — cross-topic 真发现（来因宫重叠/0 互飞/体之体冲命）
3. **§3 性格底色** (N 层合参)
4. **§4 关键人生主线** — 每 topic 一条完整故事链
5. **§5 张力点** — ≥3 个 cross-topic 真张力
6. **§6 关键结构标签** — 8-12 个 reader-facing labels
7. **§7 整体生命形态 synthesis** — 画像 label + deep summary
8. **§8 出口指向** — 给 render / Q&A / 未来扩展 的 hand-off

详见 `ziwei-feixing-core-v2/rules/stage3-composition.md`。

### Render v1.5 详细度基准

Render 严禁压缩 finding 信息。**每条 finding 必须独立小节**，每小节 4-6 段。Reader 文件大小:
- 短 topic (≤3 finding): ≥ 6 KB / 每小节 ≥ 3 段
- 复杂 topic (≥4 finding): ≥ 10 KB / 每小节 ≥ 4 段

**Step 7 强制自查**: 写完每个 topic 必跑量化基准 + 10 项自查清单，不达标立即重写。详见 `ziwei-render/SKILL.md` Step 7。

### Phase C 词条交叉

三步不可跳:
1. **C.2a 词条全列** — 每颗星/象的关键词从 deep card 列出
2. **C.2b 语义交叉** — 共振（跨星重叠→放大）+ 分层（独有→层次）
3. **C.2c 画面合成** — 从共振产主干，从分层产层次

### 忌汇聚≠凶度

高频忌星（天同/太阴/文昌/文曲/天机/廉贞）坐某宫 → 多忌汇聚是四化表算术必然，不直接断凶。判断要看: 落宫旺度、忌主司、发射宫身份、同时有无禄权科照。

### 审计检察官模式 + 三层闭环

Finding/composition/render audit 只报告问题+标严重度，不解释为什么可以放过。禁止写"但不影响判断"、"短卡已足够"等辩护。

**三层闭环纪律**: FAIL 必修, 不允许 force pass 进下一 stage。
- Stage 2 finding FAIL → 修 → 再 audit → PASS 再下 stage
- Stage 3 composition FAIL → 修 → 再 audit → PASS 再 render
- Stage 4 render FAIL → 修该 topic → 再 audit → PASS 再发布

## 已知问题

### LLM 偷懒不读 deep card

不管规则里怎么写"必读"、加粗、加警告符号，调了三天三夜，LLM 仍然会主动跳过 deep card 直接用短卡出断语（也有可能是 Anthropic 把算力偷去给 Mythos 了）。这个问题**非常致命**——deep card 是词条交叉 (Phase C) 的唯一原料来源，不读 deep = 断语全靠通论堆砌，直接影响准确度。

**当前缓解方案**:

1. **`/ziwei-warm` skill (v1.0 新增)**: Session 开始就 invoke 一次, 一次性把 68 个 deep card 全读进 context, 后续 skill 直接引用。**仅在 1m token 模型 (Opus 4.6) 上实测有效**, 200k 模型会爆 context。详见 [⚠️ ziwei-warm 模型要求](#-ziwei-warm-模型要求) 段。

2. **三层 audit 闭环**: 跑完 finding/composition/render 后调 `ziwei-finding-audit`，audit 会明确标出哪些 deep 没读、哪些 confidence 标 high 但 deep 未读等。把 audit 结果贴回去让它补做，补完之后产出质量会有明显提升。本质上是用审计做第二遍强制执行。

3. **Render Step 7 量化基准**: render 输出强制自查（小节数 = finding 数 / 段数 ≥4 / 文件 ≥6 KB / 高对比度信号必用 #11 结构化展示），catch 压缩翻车（Z01 案例: 6 finding 压成 1 节 2.7 KB, 应为 15+ KB）。

### 模型注意力/算力限制 (硬约束)

即使有以上缓解方案，目前 pipeline 表现仍受限于模型本身的注意力分配和算力调度。这是当前 LLM 的硬约束，无法通过 prompt 规则完全绕过。建议:

- **首选 1m token 模型 (Opus 4.6+)**: warm-up 全加载 + 复杂跨 stage 推理
- **200k 模型**: 跳过 warm-up，每个 skill 单独按需读 deep card
- **小模型 (Haiku 等)**: 不推荐跑完整 pipeline，可用于 Stage 1 排盘结构化等简单任务

## 许可

本 skill 系统的规则、pipeline 设计和 source-cards 为原创整理作品。
许铨仁飞星派的理论基础来自许铨仁先生的著作。
王亭之小星 addon 基于王亭之《安星法及推断实例》。

## 鸣谢

- 许铨仁先生 — 飞星派体系
- 王亭之先生 — 小星取象
