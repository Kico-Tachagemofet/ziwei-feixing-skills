# Ziwei Pre-validation R1

> 验前事方法论：用 5 条高信号 claim 校准盘面精度和 timing 层可用性

## Purpose

用 5 条高信号 claims 校准：
- 盘面数据正确性
- Timing 层正确性
- 下游 Stage 2 的可信度天花板
- Timing 技术是否 unlock

## Claim Mix

### Default: 5 claims (Timing PASS)

| # | 类型 | 说明 |
|---|---|---|
| 1 | 生年四化 static | 纯先天定数，不涉及 timing |
| 2 | 生年四化 + 来因宫/命身 | 来因宫定象 + 身宫定位 |
| 3 | Current 大运 | 当前十年的大运场域特征 |
| 4 | Topic-relevant 大运 | 与分析主题相关的大运飞化 |
| 5 | 流年/自化 timing | 近期/当年的具体触发 |

### Timing PARTIAL: 5 claims

| # | 类型 |
|---|---|
| 1-3 | 生年四化 / static claims |
| 4-5 | 大运 claims |
| — | no yearly claim |

### Timing FAIL: 5 claims

| # | 类型 |
|---|---|
| 1-5 | 全部 natal/static claims |
| — | timing locked，明确标注 |

## Claim Requirements

每条 claim 必须包含：

```markdown
### R1-<NN>

Claim: [一句人话断言，不堆术语]
Expected answer type: 准 / 不准 / 部分准 / 不知道

Basis:
- Chart rows: [引用 chart-stage1 具体行]
- Timing rows: [引用 timing metadata 具体行，如有]
- Source refs: [RC-ID]

Why high-signal:
- [为什么这条 claim 的辨别力高]
- [低模糊度、不容易被"什么人都适用"覆盖]

What this validates:
- [验证盘面数据 / timing 映射 / 特定 topic lens]
```

## Claim 生成原则

### 生年四化 claims

生年四化是先天定数，claim 方向：
- 禄落某宫 → 该宫人/事有缘、顺利、资源增多
- 忌落某宫 → 该宫人/事有困扰/亏欠/纠缠
- 权科互因 → 能力+贵人在哪个方向
- 来因宫定象 → 生命气化凝聚处的特征

### 大运 claims

大运 = 十年场域，claim 方向 [RC-xu3-ch1-15, RC-xu3-ch6-07]:
- 大命走到生年四化宫 → 该十年进入该先天现象的应用期
- 大运宫飞忌冲本命命宫 → 该十年亮红灯 [RC-b7-06]
- 大运同名宫忌冲本命同名宫 → 该领域十年凶 [RC-b7-07, 理则四]
- 大运飞宫先看本命，再看流年 [RC-xu3-ch2-11]

### 流年/自化 claims

流年 = yearly trigger，自化 = 变动标记 [RC-xu3-ch1-11]:
- 流命走到生年禄/权/科/忌 → 该年触发对应先天象 [RC-xu3-ch6-11]
- 有自化的宫位 = timing point [RC-xu3-ch1-13]
- 同类生年四化宫也是 timing point
- 自化代表变化与时间，是断应期的核心

## Hit Rate and Unlocks

### 计分

| 回答 | 得分 |
|---|---|
| 准 | 1 |
| 部分准 | 0.5 |
| 不准 | 0 |
| 不知道 | excluded from denominator，but note uncertainty |

### Unlock 规则

| Hit rate | Downstream status |
|---|---|
| >= 4/5 or >= 80% | Full Stage 2 + timing allowed |
| 3/5 to <4/5 | Stage 2 allowed; timing Moderate confidence only |
| 2/5 to <3/5 | Stage 2 structural only; timing locked except broad 大运 background |
| <2/5 | Stop; recommend re-audit chart / timing mapping |

### 输出分离

- `pre-validation-R1.md` — claims + chart-side basis + aggregate hit rate + unlock result
- `subject-context.md` — per-claim user responses + biographical explanations

Stage 2 blind-audit 只可读 aggregate hit rate 和 unlock result，不可读 per-claim biographical explanations。

## Do Not

- 不用 subject-context 已知传记来生成 claims
- 不问引导性问题
- 不把"不准"重新解释成"准"
- 不用低置信度的 cookbook 星性特征作 R1 claim
- 不在 R1 包含敏感 death/longevity/medical claims（除非明确要求且有 source 支持）

## Source Anchors

| Topic | RC-ID | Source |
|---|---|---|
| 三种四化框架 | RC-b1-01~05 | b-series/b1-three-transformation-types.md |
| 大运冲命 | RC-b7-06 | b-series/b7-flying-palace-liqi.md |
| 大运同类冲 | RC-b7-07 | b-series/b7-flying-palace-liqi.md |
| 多线索锁定 | RC-b7-09 | b-series/b7-flying-palace-liqi.md |
| 自化=应期核心 | RC-xu3-ch1-11 | xu-book3/ch1-sihua-xiangxue.md |
| 自化应期取法 | RC-xu3-ch1-13 | xu-book3/ch1-sihua-xiangxue.md |
| 大运=断命断运枢纽 | RC-xu3-ch1-15 | xu-book3/ch1-sihua-xiangxue.md |
| 批流年两条主线 | RC-xu3-ch5-02 | xu-book3/ch5-ten-essentials-marriage.md |
| 财官后天看大命 | RC-xu3-ch6-07 | xu-book3/ch6-property-cases.md |
| 流命走生年 | RC-xu3-ch6-11 | xu-book3/ch6-property-cases.md |
| 本命飞宫看大运 | RC-xu3-ch2-11 | xu-book3/ch2-wealth-career.md |
