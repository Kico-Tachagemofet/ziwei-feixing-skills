<!-- generated-by: build_source_cards_v2.py -->
# Source Router

## topic tag 到 topic method

| topic_tag | 加载 |
| --- | --- |
| marriage / peach-blossom | `rules/topic-methods/marriage.md` |
| career / wealth / industry | `rules/topic-methods/career-wealth.md` |
| health / illness / lifespan | `rules/topic-methods/health-longevity.md` |
| traffic / accident | `rules/topic-methods/traffic-accident.md` |
| property / real-estate | `rules/topic-methods/property.md` |

## chart facts + topic lens 到 source card

以下字段来自 **topic lens packet**（Stage 2a 产出，topic-dependent）：
- `main_taiji`、`involved_palaces`、`target_palace`、`opposite_palaces` -> `source-cards/palace/*.md`
- `main_stars`、`transform_stars` -> `source-cards/star/*.md`

以下字段来自 **chart_facts.yaml**（Stage 1.5 产出，topic-independent）：
- 同宫双星 -> `source-cards/combo/*.md`（见下表）
- 四化类型 -> `source-cards/sihua/*.md`
- 飞化落宫 -> `source-cards/flow/*.md`
- 应期问题、大限、流年流月 -> `source-cards/timing/*.md`
- 排盘、历法、干支基础 -> `source-cards/reference/*.md`

默认读取短卡。只有证据不足、用户追问原文、出现高风险主题或短卡提示 `needs_deep_when` 时，才读取 deep/full。

## combo dispatch 映射

loader 检测 main_stars 中的同宫/对宫星对，按下表加载对应 combo 卡：

| 同宫星对 | combo 卡 |
|---|---|
| 紫微+贪狼 / 紫微独坐对宫贪狼 | `combo/ziwei-tanlang.md` |
| 紫微+破军 | `combo/ziwei-pojun.md` |
| 紫微+天相 | `combo/ziwei-tianxiang.md` |
| 紫微+天府 | `combo/ziwei-tianfu.md` |
| 紫微+七杀 | `combo/ziwei-qisha.md` |
| 天机+巨门 / 天机独坐对宫巨门 | `combo/tianji-jumen.md` |
| 天机+天梁 | `combo/tianji-tianliang.md` |
| 天机+太阴 | `combo/tianji-taiyin.md` |
| 太阳+天梁 | `combo/taiyang-tianliang.md` |
| 太阳+太阴 | `combo/taiyang-taiyin.md` |
| 太阳+巨门 | `combo/taiyang-jumen.md` |
| 武曲+天府 / 武曲+七杀 | `combo/wuqu-tianfu-qisha.md` |
| 武曲+贪狼 | `combo/wuqu-tanlang.md` |
| 武曲+天相 / 武曲+破军 | `combo/wuqu-tianxiang-pojun.md` |
| 天同+太阴 | `combo/tiantong-taiyin.md` |
| 天同+巨门 | `combo/tiantong-jumen.md` |
| 天同+天梁 | `combo/tiantong-tianliang.md` |
| 廉贞+天相 | `combo/lianzhen-tianxiang.md` |
| 廉贞+七杀 | `combo/lianzhen-qisha.md` |
| 廉贞+天府 | `combo/lianzhen-tianfu.md` |
| 廉贞+贪狼 | `combo/lianzhen-tanlang.md` |


