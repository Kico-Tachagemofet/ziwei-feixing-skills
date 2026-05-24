---
name: ziwei-finding-audit
description: 紫微斗数飞星派 finding/composition 质量审计（检察官模式）。Stage 2 finding 产出后或 Stage 3 composition 产出后执行。A层程序合规 + B层内容质量，输出 PASS/FAIL 审计报告。审计只报告问题+标严重度，不解释为什么可以放过——放不放过是 用户 的决定。Use when 用户 says '审计 finding'、'audit composition'、'quality check'、'检查质量' 或 Stage 2/3 产出后。
---

# 紫微斗数 Finding/Composition 质量审计

> v2.1 (2026-05-24) — 扩展支持 Stage 3 composition audit (使用 stage3-composition.md A+B checklist) + 加 audit 闭环纪律段
> v2.0 (2026-05-23) — 双层审计: A层程序合规 + B层内容质量。检察官模式: 只报告问题，不辩护。
> v1.0 (2026-05-23) — 从 ziwei-feixing-core-v2 v3.3 核心纪律拆出
> Pipeline 位置:
> - **Stage 2 finding audit**: Stage 2c 之后、Stage 3 之前 (用本 SKILL 的 A1-A4 + B1-B6 checklist)
> - **Stage 3 composition audit**: Stage 3 之后、Stage 4 之前 (用 stage3-composition.md 的 A1-A4 + B1-B4 checklist)

## 审计定位: 检察官，不是辩护律师

⚠️ **审计的职责是找问题，不是解释为什么问题不严重。**

- 报告事实 + 标严重度 (BLOCKER / WARNING)
- **禁止**写"但这不影响判断"、"短卡已足够"、"不构成误判风险"、"在当前范围内成立"
- **禁止**替 finding 做辩护、找理由、重新解释规则边界
- 发现问题 → 写问题是什么 + 违反了哪条规则 + 建议修正动作。到此为止。
- 问题严不严重由 用户 判断，不由审计判断

## 职责

**做**: 逐条 finding 过 A+B 双层审计，输出 PASS/FAIL + 问题清单
**不做**: 不改写 finding / 不产新 finding / 不做飞化推导 / 不替 finding 辩护

## 输入

### Stage 2 finding audit 模式
| 文件 | 必需 |
|---|---|
| topic-findings/<slug>.md | 是 — 审计对象 |
| chart-stage1.md | 是 — 交叉验证 |
| sihua-transaction-map.md | 是 — 验证交易图引用 |
| innate-body.md | 是 — 验证先天体引用 |
| Topic Lens Packet | 是 — 验证 Taiji Lock / 覆盖度 |
| Topic Reference (如有) | 参照 `ziwei-topic-lens/references/` |
| Finding Schema | 参照 `ziwei-feixing-core-v2/rules/finding-schema.md` |

### Stage 3 composition audit 模式 (v2.1 新增)
| 文件 | 必需 |
|---|---|
| composition.md | 是 — 审计对象 |
| 所有 topic-findings/*.md | 是 — fidelity 验证 (composition 不引入 Stage 2 没说过的新断语) |
| Composition Schema | 参照 `ziwei-feixing-core-v2/rules/stage3-composition.md` (A1-A4 + B1-B4 checklist) |
| subject-context.md | 可选 — 验证 ✨ subject-context informed mark 完整性 |

---

## A 层: 程序合规 (BLOCKER — 不过则 finding 无效)

### A1. 架构门禁

- [ ] 象核存在且有完整 5 点 (主锚/凶锚/禄随忌走/用/排除误读)
- [ ] 交易图存在且被象核引用
- [ ] Finding 有象核归属 (C.0 填了)
- [ ] 非单链产出 (Chain Group ≥ 2 edge)
- [ ] Topic scope 单一 (未混合多个 scope)
- [ ] Lens Palace Index 存在且入口检查通过
- [ ] Per-star aggregation 在太极点下做了 (标发射宫角色+scope)，**且过程记录在文件中**

### A2. Gate 检查

- [ ] Gate 0 Topic 有效性 (不在禁入列表)
- [ ] Gate 1 Taiji Lock (Main Taiji == lens Main Taiji)
- [ ] Gate 2 Confidence vs Deep 一致性 (**BLOCKER**: high 必须有对应 deep ref 已读；未读 deep 的 finding 不可标 high)
- [ ] Gate 2 flow deep 已读 (太极点飞四化每个落宫+照宫)
- [ ] Gate 3 来因宫=年干 (未作后天交易 Primary)
- [ ] 凶锚宫禄权科已做四种判读之一

### A3. Deep + Source

- [ ] combo deep 已读 (太极+对宫+落宫组合)
- [ ] star deep 已读 (太极主星+化星) — **作为 Primary 证据的星必须有 deep**
- [ ] flow deep 已读 (飞四化落宫+照宫)
- [ ] wtz addon 已加载 (太极+对宫+落宫有小星时)
- [ ] 小星跨宫成对检查 (三台↔八座、龙池↔凤阁等须对宫/同宫才算成对；成对则读 wtz combo deep + 走 composition-grammar 场景合成)
- [ ] 小星系统列举 (每条 finding 的 Phase B 列出涉及宫位的关键小星 + wtz 信号标注)
- [ ] 每条 finding 有 source refs (RC-ID)
- [ ] 无 source 标了 gap

### A4. Phase C/D

- [ ] C.1 宫态吉凶从星+旺度+煞判 (不从"有忌=凶"判)
- [ ] C.2a 词条全列 (含 deep card 宫位范例)
- [ ] C.2b 语义交叉标了共振+分层
- [ ] C.2c 画面合成从交叉来 (不从 deep card 直接拍)
- [ ] C.2 没有跳步 (a→b→c 依次)
- [ ] 入/照/冲三层全记录
- [ ] 主司先于通论
- [ ] Phase D 过了 sihua-benyi.md §0 门禁
- [ ] Boundaries 列了
- [ ] Interpretive Kernel 三字段填了

---

## B 层: 内容质量 (BLOCKER — 质量不达标则 FAIL)

### B1. 覆盖度检查

根据 topic reference (如有) 检查 finding 覆盖是否完整:

- [ ] **character topic**: 是否覆盖固定 5 面 (印象/内在/脾气/先天方向/压力底色)？缺面 = BLOCKER
- [ ] **其他 topic**: 是否覆盖 reference 定义的必要面向？缺面 = BLOCKER
- [ ] 无 reference 的 topic: 覆盖度不检查，标 "no reference available"

### B2. 先天体引用检查

- [ ] innate-body.md 是否存在？不存在 = BLOCKER (应先跑 Step 0.7)
- [ ] 生年四化资源盘点是否在 finding/象核中被引用或分析？**完全不提生年四化 = BLOCKER**
- [ ] 命迁线画面是否从 innate-body 引用 (不重做)?

### B3. 忌汇聚结构必然性检查

- [ ] 如有忌汇聚 (≥3忌落同宫/冲同宫): 是否检查了该宫坐星是否为高频忌星？
- [ ] 高频忌星坐该宫但 finding 当特异凶兆处理 = BLOCKER
- [ ] 忌汇聚 finding 是否分析了发射宫身份 (谁给的忌=什么内容)？只数忌不看来源 = BLOCKER

### B4. Confidence vs Deep 一致性 (A2 的加强版)

- [ ] high confidence finding 是否全部有对应 deep card 已读？有任何一条 high + deep 未读 = BLOCKER
- [ ] finding 声称"短卡已足够" = 自动标 WARNING (审计不判断够不够，让 用户 看)

### B5. 四化取象宫象合参

- [ ] 每条涉及四化的断语是否做了宫象合参？(禄在疾厄=体质好，不是抽象"缘起")
- [ ] 通论义直接搬宫 = BLOCKER (例: "禄=缘起" 直接用在疾厄宫)
- [ ] 视同自化定义是否正确？(=不断缘起/新缘更替，不是"回收/自给自足")

### B6. 断语纪律

- [ ] 没有"管束" (忌入断困扰/是非/纠缠/执念)
- [ ] 忌义按条件分读 (人/事/物 × 入/冲)
- [ ] 星性先查组合 (先组合后单星)
- [ ] Finding 不做综合叙事 (一句中文直断)
- [ ] 宫态优先于飞化方向
- [ ] 来因宫飞化=生年四化标了体之体
- [ ] 人位宫三角度 (如太极=六亲宫): 关系面+利益面+画像面
- [ ] N象一星标了"结构常见"vs"特异"
- [ ] 不读 subject-context (盲审纪律)

---

## Verdict 判定

| 条件 | Verdict |
|---|---|
| A层或B层有任何 BLOCKER | **FAIL** |
| 只有 WARNING，无 BLOCKER | PASS_WITH_WARNINGS |
| 全部通过 | PASS |

⚠️ **PASS_WITH_WARNINGS 不等于"可以不管"。** 每条 WARNING 都需要 用户 确认是否接受。

---

## ⚠️ Audit 闭环纪律 (v2.1 新增, 从 Z08-kico-mom 实践)

**FAIL 必修, 不允许 force pass 进下一 stage**。

### 闭环流程

1. Audit 输出 FAIL + BLOCKER 清单
2. 主 agent 按清单逐条修 (读 deep / 加 §0 / 改框架 / 补 IK / 等)
3. 修完**再调 audit skill 重审**, 不依赖记忆/感觉
4. PASS 或 PASS_WITH_WARNINGS (用户 接受) 后再进下一 stage

### 反例 (禁止)

- ❌ Stage 2 audit FAIL → 觉得 BLOCKER "不重要" → 直接进 Stage 3
- ❌ Audit 输出"短卡已足够" / "在当前范围内成立" 等辩护性陈述 → 检察官原则禁止
- ❌ BLOCKER 跨 stage 累积 → 最终 render 出来才发现一堆系统性问题

### 正例 (Z08 实践)

- ✅ character audit FAIL (2 BLOCKER) → 修 §0×4 + into-ming flow deep → 再 audit → PASS
- ✅ partnership audit FAIL (3 BLOCKER) → 修 flow deep + Phase A + 凶锚段重判 → 再 audit → PASS
- ✅ composition audit PASS_WITH_WARNINGS (3 WARNING) → 修一致性 + subject-context mark + 措辞 → 0 WARNING

### 适用范围

不只 Stage 2 finding audit, **Stage 3 composition audit** + 任何后续 stage 的 audit 都遵循同纪律。
本 skill (ziwei-finding-audit) 可用于 Stage 3 composition audit, 用 stage3-composition.md 的 A+B checklist 代替本 SKILL 的 A+B。

---

## 常见失误参照

审计完 A+B 双层后, 对照 `common-mistakes.md` 扫一遍已知 pattern:
- M-A2-01~04: 取象逻辑错误(落宫/照宫双层、夫官互通、忌入六内主得、宫职/六内双层)
- M-A3-01~02: Deep/Source 遗漏(小星成对只标不读、star deep 替代未标注)
- M-A4-01~02: Phase C/D 流程遗漏(§0 缺失、入/照/冲不全)
- M-B1-01~03: 内容质量(confidence vs deep、忌汇聚结构必然、龙池凤阁误判)

命中已知 pattern → 直接标 WARNING/BLOCKER + 引用 M-ID。

---

## 输出格式

```markdown
# Finding 质量审计: [case] / [topic]

## 总览
| 层 | 类别 | 总数 | 通过 | BLOCKER | WARNING |
|---|---|---|---|---|---|
| A | A1 架构 | | | | |
| A | A2 Gate | | | | |
| A | A3 Deep+Source | | | | |
| A | A4 Phase C/D | | | | |
| B | B1 覆盖度 | | | | |
| B | B2 先天体 | | | | |
| B | B3 忌汇聚 | | | | |
| B | B4 Confidence一致 | | | | |
| B | B5 四化宫象合参 | | | | |
| B | B6 断语纪律 | | | | |

## Verdict: [PASS / PASS_WITH_WARNINGS / FAIL]

### Blockers
- [finding ID]: [检查项] — [事实描述] — [违反规则] — [建议修正]

### Warnings
- [finding ID]: [检查项] — [事实描述] — [建议修正]
```

**格式硬约束**:
- Blockers 和 Warnings 只写: 事实 + 违反规则 + 修正建议
- 不写: "但不影响..."、"在当前范围内..."、"已足够支撑..."
- 每条控制在 2-3 行，不写长段辩护

## First-invocation Flow

1. 接收 topic-findings/<slug>.md
2. 接收 chart-stage1 + transaction-map + innate-body + Lens Packet
3. 查 topic reference (如有) 确定覆盖度要求
4. **A 层**: 逐条 finding 过 A1-A4
5. **B 层**: 整体过 B1-B6
6. 汇总输出审计报告
7. 有 BLOCKER → FAIL; 只有 WARNING → PASS_WITH_WARNINGS; 全过 → PASS
