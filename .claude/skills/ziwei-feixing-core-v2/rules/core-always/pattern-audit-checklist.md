# Pattern Audit Checklist

> Tier 1 core-always。Stage 2b.5 每次必跑。
> 输入: chart_facts (from Stage 1.5 / 2b) + topic lens packet 的 topic_tag / query flags（P14 需要）
> 输出: triggered_patterns: []
> 逻辑: 对每条 check，命中则输出 pattern ID + 加载对应卡。未命中跳过。

## 检查表

| ID | Pattern | 检查条件 | 命中后加载 |
|---|---------|---------|----------|
| P01 | 退马忌 | flying_transforms 中: 忌入隔一宫 AND 该宫有 self_transform.忌 | `pattern-rules/tuimaji.md` |
| P02 | 三象媒介 | 任一宫命中 ≥3 个四化象（生年+飞宫+自化组合计） | `pattern-rules/sanxiang-meijie.md` |
| P03 | 万象归一元 | 大命/流命所在宫 == 某生年四化所在宫 | `pattern-rules/wanxiang-guiyiyuan.md` |
| P04 | 四凤三旗 | 有自化 AND 同宫无同类生年（纯自化，无生年锚） | `pattern-rules/sifeng-sanqi.md` |
| P05 | 无生年有自化 | 某宫 self_transforms ≠ [] AND native_transforms == [] | `pattern-rules/wu-shengnian-you-zihua.md` |
| P06 | 单象+单自化 | 某宫恰好 1 生年 + 1 自化 | `pattern-rules/danxiang-danzihua.md` |
| P07 | 单象+双自化 | 某宫 1 生年 + ≥2 象飞出/自化 | `pattern-rules/danxiang-shuangzihua.md` |
| P08 | 双象+双自化 | 某宫 ≥2 生年/飞宫象 + ≥2 自化 | `pattern-rules/shuangxiang-shuangzihua.md` |
| P09 | 生年+自化忌 | 某宫有生年禄/权/科 AND 同时有自化忌 | `pattern-rules/shengnian-zihuaji.md` |
| P10 | 出忌/刺出忌 | 飞忌入B AND B对宫有生年忌或自化忌 | `pattern-rules/chuji.md` |
| P11 | 自化带出震荡 | 某宫有自化X AND 同宫有同类生年X（如自化禄+生年禄） | `pattern-rules/zihua-daichu.md` |
| P12 | 洛书宫职重叠 | 大限命宫 与 本命宫 形成洛书位（戴九履一/二四肩/左三右七/六八足） | `pattern-rules/luoshu-chongdie.md` |
| P13 | 交易象碰撞 | 飞宫四化落处已有生年四化（伏象碰撞） | `pattern-rules/jiaoyi-pengzhuang.md` |
| P14 | 自化应期 | 有自化 AND topic 涉及"何时/几岁/应期/时间" | `pattern-rules/zihua-yingqi.md` |

## 输出格式

```yaml
triggered_patterns:
  - id: P05
    pattern: 无生年有自化
    palace: 夫妻宫
    load: pattern-rules/wu-shengnian-you-zihua.md
  - id: P09
    pattern: 生年+自化忌
    palace: 官禄宫
    load: pattern-rules/shengnian-zihuaji.md
```

## 注意

- P04 和 P05 有重叠：P04 聚焦"无同类生年"（可能有其他类型生年），P05 聚焦"完全无生年"。两者可同时 hit。
- P14 触发面较宽。应期基础判读（自化宫+同类生年宫交会）已在 `self-transform-minimal.md` 中，P14 卡只补充进阶应期技法。
- 检查基于 chart_facts 结构化数据 + topic lens 的 query flags，不重新读盘面。chart_facts 不完整则对应 check 跳过；P14 需 topic lens 提供 query flags。
