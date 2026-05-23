---
name: ziwei-source-lookup
description: 紫微斗数 source-loader。v2.1 起短卡已开局全加载，本 skill 收窄为 deep 决策 + coverage audit + extractions fallback。Use when chart-facts.yaml ready and pipeline 进到 Stage 2b。不做断语，只提供证据。
---

# 紫微斗数 Source Loader Skill

> v2.1 (2026-05-21) — deep 决策 + coverage audit
> v2.0 按 chart_facts 映射表加载 source-cards 短卡
> v2.1 变更: 短卡已开局全加载(Tier 2)，本 skill 不再加载短卡，收窄为 deep 决策

## 职责

**v2.1 变更**: 所有 source-cards 短卡（非 deep/）已在开局全加载。本 skill 不再负责加载短卡。

**做**：
- 按 chart-facts + topic lens，列出哪些短卡的 needs_deep_when 被命中 → deep 追读清单
- Coverage Audit: chart_facts 有值但无对应短卡 → 标 gap
- Gap fallback: 搜索 `extractions/` 原始蒸馏
- Provenance 标注

**不做**：
- ~~加载短卡~~（已在开局全加载）
- 不做断语
- 不产 finding
- 不写 reader prose
- 不做飞化链计算

## 输入

- `chart-facts.yaml`（必需，from Stage 1.5）
- `topic_tag`（必需，from Stage 2a topic-lens）
- Topic Lens Packet（可选，兼容旧流程的 lookup queries）

## 加载逻辑

**确定性映射，不搜索。**

1. 读 `core-always/source-router.md` 映射表
2. ~~加载短卡~~ — 已在开局全加载，本步骤跳过
3. 按 topic_tag 查 `source-router.md` 映射表加载专题方法（不可直拼 `{topic_tag}.md`，因 career→career-wealth.md 等不对称）

### 查找路径

**短卡已在开局全加载（Tier 2）。** 本步骤只处理 deep 追读和 gap fallback。
source-cards 目录结构见 `core-always/source-router.md`。

### deep 追读条件

短卡 `needs_deep_when` 标记命中时，追读 `deep/*.full.md`:
- 短卡信息不足以支撑判断链
- 用户追问星性/原文/条件/例外
- 两个来源说法冲突需保留差异
- combo/star 会被用于 Primary finding、人物画像、主断语、吉凶修正、关系机制判断
- 需要判断主司优先级（官禄主、祸福主、财星、父母星、荫星等）时
- 需要排除某个候选义（如亏欠、桃花、无缘、灾厄）时

**v3.3 总原则**: 短卡只能 dispatch 和速查。Deep card 是分析的基础材料，不是可选的 confidence bumping。进入 Stage 2 时，太极点相关宫位的全套 deep 必须加载。

**硬规则 1 (combo deep)**: 太极点组合 + 对宫组合 + 飞四化落宫组合(如有主星) → 全部读 combo deep。未读则 Source Packet 标 `combo-deep-missing`。

**硬规则 2 (star deep)**: 太极点主星(2颗) + 飞四化化星(4颗，去重) → 全部读 star deep。未读标 `star-deep-missing`。

**硬规则 3 (flow deep)**: 太极点飞四化每个落宫+照宫 → 全部读 `flow/deep/into-{slug}.full.md`。"A飞X入B照C到底意味什么"由 flow deep 定义。未读标 `flow-deep-missing`。

**硬规则 4 (wtz addon)**: 太极点+对宫+落宫有小星(辅曜/煞曜/小星)时 → 加载对应 wtz 短卡+deep。小星信号是 gestalt 画面的必要组成部分。未读标 `wtz-missing`。

**任何标 missing 的 deep 项 → 对应飞化链不可作 Primary finding。Source Packet 必须列出全部 deep 加载状态清单。**

### Fallback

source-cards 无覆盖时，回退搜索 extractions/ 原始蒸馏:
```powershell
rg -n "<keyword>" "<YOUR_EXTRACTIONS_PATH>"
```

## 输出

`Source Packet` — 一个 markdown 文件，交给 ziwei-feixing-core-v2 使用。

### Source Packet Schema (v2)

```markdown
# Source Packet: <case> / <topic>

## Loading Inputs
- topic_tag: [marriage/career/health/traffic/property/general]
- chart_facts_fields_used: [列出哪些 chart_facts 字段触发了加载]

## Loaded Cards

### Palace Cards
- `palace/fuqi.md` — 夫妻宫速查
- `palace/ming.md` — 命宫速查

### Star Cards
- `star/ziwei.md` — 紫微星速查

### Combo Cards
- `combo/ziwei-tanlang.md` — 紫微贪狼组合（桃花）

### Sihua Cards
- `sihua/ji.md` — 化忌

### Flow Cards
- `flow/into-ming.md` — 四化入命宫

### Deep Cards (追读)
- `star/deep/ziwei.full.md` — [追读原因: needs_deep_when 命中]
- `combo/deep/ziwei-tanlang.full.md` — [追读原因: Primary/人物画像使用组合]

## Deep Priority Audit
| Chain / candidate finding | combo/star used as | required deep | loaded? | verdict |
|---|---|---|---|---|
| [F-xxx 或链名] | Primary / 人物画像 / 主断语 / 吉凶修正 | combo deep + star deep | yes/no | ok / insufficient-source |

## Topic Method
- `topic-methods/marriage.md` — 婚姻专题操作流程

## Key Source Excerpts

[按类别列出从短卡/deep 中提取的关键 snippet，带来源标注]

### 宫义
> [来源: palace/fuqi.md] ...

### 星性
> [来源: star/ziwei.md] ...

### 组合
> [来源: combo/ziwei-tanlang.md] ...

## Gaps
- [chart_facts 中有值但无对应卡的字段]
- [追读 deep 后仍未覆盖的内容]

## Coverage Audit
| 类别 | 应加载 | 已加载 | 缺失 |
|---|---|---|---|
| palace | [N] | [N] | [0 或列出] |
| star | [N] | [N] | |
| combo | [N] | [N] | |
| sihua | [N] | [N] | |
| flow | [N] | [N] | |
```

## Timing Source Packet Queries

When topic lens or debug grounding check requires timing data, use these query categories:

### timing-layering
- `生年四化` as 体（先天定数）
- `大运` / `大限` as 十年场域
- `流年` as yearly trigger
- `自化` / `自化应期` as change/time marker

### major-period
- `大运命宫走生年四化` / `大命走到生年`
- `大运宫飞忌冲本命命宫` / `大运冲命`
- `大运同名宫忌冲本命同名宫` / `理则四` / `同类用冲体`
- `气由大向小走` / `本命飞宫看大运`

### annual
- `流命` / `流迁` + `走生年禄权科忌`
- `流命飞宫先归大运再归本命`
- `流年有自化` = year has change
- `流年` only triggers; does not decide existence

### self-transform-timing
- `自化宫` as timing point
- `同类生年四化宫` as timing point
- `自化代表变化与时间`
- `入 vs 出` / `万象归一元`

### Key Source Files for Timing

```
b-series/b1-three-transformation-types.md  (RC-b1-01~05)
b-series/b7-flying-palace-liqi.md          (RC-b7-06~09, 大运操作速查)
xu-book3/ch1-sihua-xiangxue.md             (RC-xu3-ch1-11~19)
xu-book3/ch2-wealth-career.md              (RC-xu3-ch2-11, ch2-18)
xu-book3/ch5-ten-essentials-marriage.md    (RC-xu3-ch5-02~04)
xu-book3/ch6-property-cases.md             (RC-xu3-ch6-07~11)
```
- ...
```

## Edge Cases 必须能回答

以下问题在 source packet 中必须有明确回答（有 source 支持则引用 RC-ID，无 source 则标 gap）：

1. **人位忌入人位到底断什么** — 机制是"管束"（A干涉B），断语是困扰/是非/纠缠/执念。不能把"管束"作为断语输出。
2. **理则二冲 C 能不能独立断** — 不能。只有理则三/五的冲命才独立成断。冲 C 是气的结构记录。
3. **禄忌例外条件到底是什么** — 飞宫忌碰**生年禄**，或**生年禄同宫自化忌** → 虽起伏但禄在。需确认"飞禄碰自化忌"是否也算例外。
4. **双星组合 vs 单星** — 必须先查组合（武杀、廉贪、机巨、同梁等），再看单星补充。
5. **太阴在命、太阳对宫** — 是否应看日月对拱组合，还是只看太阴单星。
6. **Primary/人物画像是否已 deep** — 如果 combo/star 要支撑主断语，source packet 必须列出 combo deep + star deep 的加载状态。

## 核心纪律

1. **不编造 source** — 找不到就标 gap，不能用"一般认为"替代
2. **RC-ID 必须引用** — 每条 snippet 标明出处
3. **guardrail 必须带上** — RC card 的 guardrail 不能省略
4. **不做推断** — source packet 只提供证据，推断是 core-v2 的事
5. **固定加载，不搜索** — 短卡/deep/topic-method 全部按 source-router 映射表确定性加载。只有标 gap 后才 fallback 搜 extractions/
6. **流派隔离** — 分析框架（理则/四化表/飞化操作/应期）纯许铨仁体系。星性取象当前纯许铨仁，紫云系列蒸馏尚未接入。顾绍贤/qintian/nanpai 只做参考标注，不进推导链
7. **禁引盘主本案例** — 盘主自己的案例分析（如 Z01）不能当 source，否则=抄答案不 test skill
8. **短卡不支撑人物画像** — 人物画像、关系机制、吉凶主断语必须来自 deep 或明确标 insufficient-source
