---
name: ziwei-wangtingzhi-minor-stars-addon
description: 王亭之《安星法及推断实例》小星 addon sibling。Use with ziwei-feixing-core-v2 when 紫微斗数 analysis needs 辅曜、佐曜、煞曜、空劫、桃花、文曜、科名诸曜、流曜、安星法校验、成对/单星/杂曜组合 signals. Provides short cards and deep cards for minor-star signal extraction; does not replace the main Xu Quanren flying-star framework.
metadata:
  short-description: Wang Tingzhi minor-star addon
---

# 王亭之小星 Addon

本 skill 是 `ziwei-feixing-core-v2` 的 sibling addon，用王亭之《安星法及推断实例》整理小星、辅佐煞杂曜、流曜与安星法信号。

## 边界

- 主推导链仍由 `ziwei-feixing-core-v2` 负责：四化表、飞化理则、自化、忌义、应期主框架不在本 skill 改写。
- 本 skill 只提供 `[wtz]` 星曜信号层：小星分类、安星/流曜定位校验、组合质感、实例触发条件。
- 小星不能单独产出飞星 finding。必须先有飞化链、topic 宫位关系、三方四正会照、流限叠会或实例型组合命中。
- 若 `[wtz]` 与许铨仁核心飞星链冲突，核心飞星链优先；`[wtz]` 降级为辅助/背景/待核。

## 启动读取

每次触发本 addon，先读：

1. `rules/core-always/provenance-policy.md`
2. `rules/core-always/taxonomy.md`
3. `rules/core-always/composition-grammar.md`
4. `rules/core-always/minor-star-router.md`

然后按 router 读取 `source-cards/` 中的小卡。只有 `needs_deep_when` 命中时才读 deep。

## 使用位置

### Core Phase B 信号收集

当 core 进入“辅星信号（天马/桃花星/煞星？）”时，用本 addon 补齐：

- `minor_star_packet.group_signals`
- `minor_star_packet.pair_signals`
- `minor_star_packet.single_star_warnings`
- `minor_star_packet.flow_star_notes`
- `minor_star_packet.deep_cards_needed`

### Core Phase C 义的激活

只把本 addon 的结果作为质感证据：

- `supporting_signal`: 与飞化链方向一致，增强断语质感。
- `qualifier`: 修正吉凶、成败方式、人物/物象形相。
- `background`: 有星但没有推导关系，只作盘面背景。
- `excluded`: 与 topic/飞化链无关，不入 finding。

## 合参硬规则

小星不是独立标签库。任何“小星=某断语”的写法都必须先改写成：

```text
主星/组合的基础象 + 小星修饰 + 宫位/四化/流限承接 = 可用断语
```

例如不要写“红鸾=幽默有才”或“文曲=绕弯子”。应写成“巨门的口舌/表达象被红鸾或文曲修饰后，才可能出现幽默、有文采、文雅绕弯等表达质感”。脱离巨门、天机巨门、昌曲化忌等语境，小星只保留为候选信号。

## 输出格式

```yaml
minor_star_packet:
  source: wtz
  cards_loaded:
    short: []
    deep: []
  signals:
    primary: []
    supporting: []
    background: []
    excluded: []
  composition_checks:
    - "每个小星断语都绑定主星/组合/宫位/四化/流限"
    - "没有把小星单独当 cookbook 标签"
  boundaries:
    - "小星不单独产飞星 finding"
    - "静态星曜须有飞化链、宫位关系或流限叠会才可入断"
  provenance_tags:
    - "[wtz]"
```

## Deep Gate

读取 deep 的条件：

- 小星成为主证据或会改变吉凶判断。
- 出现成对/单星争议，例如辅弼、魁钺、昌曲只见一颗。
- 涉及流曜叠会、流昌流曲、流羊流陀、流年桃花等。
- 涉及安星法、起例、流曜定位校验。
- 需要引用王亭之实例模式，而不是只做分类标签。

## 来源

主要来源：王亭之《安星法及推断实例》。已提取为 `source-cards/` 下的结构化卡片，运行时不需要原书。
