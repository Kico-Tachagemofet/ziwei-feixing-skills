# 紫微飞星 Skills for Claude Code

紫微斗数飞星派分析 pipeline，以 Claude Code skill 形式运行。

## 体系

**核心框架**: 许铨仁飞星派。四化表、理则、忌义分读、自化、应期等核心推导链走许铨仁体系。

**小星系统**: 王亭之《安星法及推断实例》。辅佐煞杂曜取象、场景合成、安星法校验走王亭之体系，作为飞星核心的 addon。

**其他借鉴**: 部分三合派小技法（宫位取象层面），但不进入飞化推导链。飞化链内部保持单一流派纯净度。

**核心认识论**: 象核 (structure kernel) 是思考单位，finding 是输出单位。先建交易图，再建象核，最后从象核产断语。不做 per-chain finding。

## Pipeline

```
Stage 1:   排盘结构化 ─── ziwei-reader
  ↓
Stage 1.5: 全盘结构分析 + chart-facts ─── ziwei-feixing-core-v2
  ↓
Step 0:    飞化交易象全列 (48 edges factual layer)
  ↓
Step 0.7:  先天体 (命迁线 deep + 生年四化资源盘点)
  ↓
Stage 2:   Per-topic Finding Production
  ├─ Topic Lens ─── ziwei-topic-lens
  ├─ Source Loading ─── ziwei-source-lookup
  └─ 象核 + Finding ─── ziwei-feixing-core-v2
  ↓
Audit:     质量审计 ─── ziwei-finding-audit
  ↓
Stage 3:   Cross-topic Composite
  ↓
Stage 4:   Render ─── ziwei-render
```

## Skills 一览

| Skill | 职责 |
|---|---|
| **ziwei-feixing-core-v2** | 核心引擎。象核构建 + finding production + 先天体 + 交易图。前置缺失自动恢复。 |
| **ziwei-reader** | 盘面结构化。文墨天机/截图/手动输入 → chart-stage1.md + audit |
| **ziwei-topic-lens** | 太极点选择 + 宫对信号索引。含 character/sexuality/occult-spirituality/路人粉丝 等 topic reference |
| **ziwei-source-lookup** | Source-card deep 决策 + coverage audit |
| **ziwei-render** | 中文散文渲染（纯翻译层，不做技术分析） |
| **ziwei-finding-audit** | 质量审计（检察官模式）。A 层程序 + B 层内容，不做辩护 |
| **ziwei-wangtingzhi-minor-stars-addon** | 王亭之小星 addon。辅佐煞杂曜 + 场景合成 + 安星法 |

## 安装

1. 将 `.claude/skills/` 下的 7 个目录复制到你的 Claude Code 项目的 `.claude/skills/` 中
2. 将 `CLAUDE.md` 的内容追加到你项目的 `CLAUDE.md`
3. 配置 source-cards 路径（见下方）

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

王亭之 addon 的 source-cards 已包含在 repo 中，运行时不需要原书。

## 关键设计决策

### 象核 (Structure Kernel)

每个 topic 进入 Stage 2 后，先建象核五点:
1. **主锚** — 哪个宫/象设定基调
2. **凶锚** — 哪个相关宫客观凶（星+旺度+煞判，不是"有忌=凶"）
3. **禄随忌走** — 忌端+禄端+方向（因果组）
4. **用** — 实际产出（combo deep 必读）
5. **排除误读** — 盘面反驳什么常见误读

### 先天体 (Innate Body)

Step 0.7 产出 `innate-body.md`，是所有 topic 的共享输入:
- 命迁线 combo deep 必读
- 生年四化 4 落宫资源盘点（combo 短卡 + 宫象合参）
- 来因宫结构
- 结构速写 + open threads

### Phase C 词条交叉

三步不可跳:
1. **C.2a 词条全列** — 每颗星/象的关键词从 deep card 列出
2. **C.2b 语义交叉** — 共振（跨星重叠→放大）+ 分层（独有→层次）
3. **C.2c 画面合成** — 从共振产主干，从分层产层次

### 忌汇聚≠凶度

高频忌星（天同/太阴/文昌/文曲/天机/廉贞）坐某宫 → 多忌汇聚是四化表算术必然，不直接断凶。判断要看: 落宫旺度、忌主司、发射宫身份、同时有无禄权科照。

### 审计检察官模式

Finding audit 只报告问题+标严重度，不解释为什么可以放过。禁止写"但不影响判断"、"短卡已足够"等辩护。

## 已知问题

### LLM 偷懒不读 deep card

不管规则里怎么写"必读"、加粗、加警告符号，调了三天三夜，LLM 仍然会主动跳过 deep card 直接用短卡出断语（也有可能是 Anthropic 把算力偷去给 Mythos 了）。这个问题**非常致命**——deep card 是词条交叉 (Phase C) 的唯一原料来源，不读 deep = 断语全靠通论堆砌，直接影响准确度。

**目前比较有效的解决办法**: 跑完 finding 后调 `ziwei-finding-audit` 审计，audit 会明确标出哪些 deep 没读。把 audit 结果贴回去让它补做，补完之后产出质量会有明显提升。本质上是用审计做第二遍强制执行。

## 许可

本 skill 系统的规则、pipeline 设计和 source-cards 为原创整理作品。
许铨仁飞星派的理论基础来自许铨仁先生的著作。
王亭之小星 addon 基于王亭之《安星法及推断实例》。

## 鸣谢

- 许铨仁先生 — 飞星派体系
- 王亭之先生 — 小星取象
