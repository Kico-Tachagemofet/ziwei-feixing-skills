---
name: ziwei-reader
description: 紫微斗数命盘结构化 + 审计 + subject-context 隔离。把原始命盘（文墨天机导出/截图/手动输入）转成 chart-stage1.md 结构化数据，做完整性审计（含 timing verdict），隔离盘主背景信息。Pipeline 第一步——跑完 reader 后进 Stage 1.5 全盘结构分析（ziwei-feixing-core-v2 structural mode）。Use when 用户 provides raw chart data and says '排盘'、'结构化'、'Stage 1'、'reader 跑一下'。
---

# 紫微斗数 Reader Skill

> v1.2 (2026-05-20) — 盘面结构化 + 审计 + subject-context 隔离
> 从 ziwei-feixing-core v1 Stage 1 拆出
> R1 验前事已移至 Stage 1.5 (ziwei-feixing-core-v2 structural mode)，见 pipeline-spec.md

## 职责

把原始命盘（文墨天机导出、截图、手动输入）转成结构化数据，审计完整性，隔离 subject 背景信息。

**做**：
- 盘面结构化（十二宫 + 生年四化 + 各宫飞四化 + 自化）
- 审计（十二宫完整性、宫干正确性、四化表一致性）
- Subject-context 隔离（盘主背景信息单独存放）

**不做**：
- 不做飞宫解读
- 不产 finding
- 不选太极点
- 不写 reader prose

## 输入

- 文墨天机 app 导出文本 / 排盘截图 / 用户手动输入的出生信息

## 输出

```
charts/<case>/chart-stage1.md         — 结构化盘面
charts/<case>/chart-stage1-audit.md   — 审计报告
charts/<case>/subject-context.md      — 盘主背景信息（Stage 2 禁读）
```

## 隔离纪律

**Stage 2 (ziwei-feixing-core-v2) 禁读 subject-context.md。**

Stage 2 只可读：
- chart-stage1.md
- chart-stage1-audit.md
- pre-validation-R1.md 的 aggregate hit rate（不读具体预判内容）
- Topic Lens Packet
- Source Packet

Subject-context.md 只在 render 阶段可读（用于校准 reader-facing 表述）。

## chart-stage1.md 模板

```markdown
# 紫微斗数 Stage 1 结构化盘面

> Subject: [case name]
> Source: [data source]
> Stage 1 date: [date]

## 基本信息

| 项目 | 值 |
|---|---|
| 性别 | |
| 钟表时间 | |
| 真太阳时 | |
| 农历时间 | |
| 节气四柱 | |
| 五行局数 | |
| 命主 | |
| 身主 | |
| 身宫位置 | |
| 来因宫 | |

## 十二宫总表

| 宫名 | 干支 | 主星(旺度) | 辅星(旺度) | 生年四化 | 自化 | 对宫 |
|---|---|---|---|---|---|---|
| 命宫 | | | | | | 迁移宫 |
| ... | | | | | | |

## 生年四化

年干: **[X]**

| 四化 | 化星 | 落宫 |
|---|---|---|
| 化禄 | | |
| 化权 | | |
| 化科 | | |
| 化忌 | | |

## 各宫宫干飞四化速查

| 宫名 | 宫干 | 化禄→(星@宫) | 化权→(星@宫) | 化科→(星@宫) | 化忌→(星@宫) |
|---|---|---|---|---|---|
| 命宫 | | | | | |
| ... | | | | | |

## 自化 / 视同自化

| 宫名 | 自化类型 | 方向 | 说明 |
|---|---|---|---|
| | | ↓离心 / ↑向心 | |

## Timing Metadata

| Field | Value |
|---|---|
| Age system | 虚岁 |
| Current nominal year | |
| Current virtual age | |
| 大运 direction | 顺行/逆行 |
| Current 大运 age range | |
| Current 大运命宫 | [对应本命哪个宫] |
| Target 大运 age range | [如有特定分析目标] |
| Target 大运命宫 | |

## 大运十二宫映射

| 大运宫职 | 对应本命宫 | 本命宫干 | 大运宫干 | 主星 | 生年四化 | 自化 |
|---|---|---|---|---|---|---|
| 大命 | | | | | | |
| 大兄 | | | | | | |
| 大夫 | | | | | | |
| 大子 | | | | | | |
| 大财 | | | | | | |
| 大疾 | | | | | | |
| 大迁 | | | | | | |
| 大友 | | | | | | |
| 大官 | | | | | | |
| 大田 | | | | | | |
| 大福 | | | | | | |
| 大父 | | | | | | |

## 流年宫位映射

For current year and optionally ±2 years:

| Year | 虚岁 | 流命对应本命宫 | 流迁对应本命宫 | 走到生年禄? | 走到生年权? | 走到生年科? | 走到生年忌? | 自化触发? |
|---|---:|---|---|---|---|---|---|---|

**Important**: 若源盘不含大运区间/方向，标 UNKNOWN 并阻断 timing pre-validation。不可自行推算补脑。
```

## chart-stage1-audit.md 检查清单

- [ ] 十二宫是否全部填入（12行完整）
- [ ] 宫干是否全部填入（天干10配12宫，必有重复天干但干支不重复）
- [ ] 生年四化是否与年干查表结果一致
- [ ] 各宫飞四化的化星是否与该宫宫干查表结果一致
- [ ] 各宫飞四化的落宫是否 = 该化星在盘中的实际位置
- [ ] 自化标注是否与命盘一致
- [ ] 身宫位置是否标注
- [ ] 来因宫是否标注
- [ ] 有无 UNKNOWN 项（列出）
- [ ] 特别记录（供 Stage 2 参考的结构特征，如互飞忌、双自化等）
- [ ] 结构总评: PASS / FAIL

### Timing Audit

- [ ] 虚岁 computed correctly
- [ ] 大运 age ranges copied from source chart or computed with documented rule
- [ ] Current 大运命宫 identified
- [ ] 大运十二宫 sequence follows 命兄夫子财疾迁奴官田福父
- [ ] 大运宫职 correctly mapped back to 本命宫
- [ ] 流命 / 流迁 for target year mapped
- [ ] 生年四化 trigger flags correct
- [ ] UNKNOWN timing fields listed
- [ ] Timing verdict: PASS / PARTIAL / FAIL

Timing verdict gate:

| Verdict | 条件 | Pre-validation 允许范围 |
|---|---|---|
| PASS | 大运+流年映射完整且审计通过 | 生年 + 大运 + 流年/自化 claims |
| PARTIAL | 大运可用但流年映射不完整 | 生年 + 大运 claims only，no 流年 |
| FAIL | 大运数据缺失或审计不通过 | natal-only claims，timing locked |

## subject-context.md 模板

```markdown
# Subject Context: [case name]

> Stage 2 禁读此文件。仅 render 阶段可用。

## Subject Profile
- 性别:
- 年龄/时代:
- 已知背景: [简要，不含 chart 数据]

## Known Life Events
- [如有]

## Subject Type
- 真人 / 虚构角色 / hybrid / 灵体

## Notes
- [任何影响 render 表述的特殊信息]
```

## 填表操作规则

1. 十二宫必须全部填完，不可省略空宫（空宫填"无主星"）
2. 宫干从命盘直接读取，不自己推算
3. 生年四化查十天干四化表，用出生年天干
4. 各宫飞四化查十天干四化表，用该宫宫干
5. 飞四化落宫 = 查该化星在命盘中的实际位置（星辰导气）
6. 自化用命盘标注（↓ = 离心自化，↑ = 向心自化）
7. 旺度照搬命盘标注，不自创
8. UNKNOWN 的地方标 UNKNOWN，不补脑

## First-invocation Flow

1. 检查用户是否提供了命盘数据
   - 有 → Step 2
   - 无 → 提示提供
2. 结构化 → chart-stage1.md
3. 审计 → chart-stage1-audit.md
4. 提取 subject 背景 → subject-context.md
5. 报告 audit 结果
6. **停止。** 输出以下提示后不再执行任何后续操作:

> Stage 1 完成。下一步请输入:
> - `跑 Stage 1.5` 或 `全盘分析` → 调用 ziwei-feixing-core-v2 进入 Stage 1.5 全盘结构分析
> - `跑全流程` → 从 Stage 1.5 开始自动走完

## 出口门禁

⚠️ **本 skill 的职责到 Stage 1 结束。以下行为全部禁止:**

- ❌ 不读 pipeline-spec.md 继续执行 Stage 1.5
- ❌ 不自行开始全盘结构分析 / chart-facts 萃取
- ❌ 不读 source-cards 或 core-always 规则文件
- ❌ 不飞四化、不做宫象星合参、不产 finding

Stage 1.5 及后续阶段必须通过 **Skill tool invoke `ziwei-feixing-core-v2`** 进入，不可在 reader 内直接执行。读了 pipeline-spec 也不能跑——pipeline-spec 是路线图不是执行授权。
