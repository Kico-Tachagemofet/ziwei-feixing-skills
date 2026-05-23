# Minor Star Router

## 输入字段

可接受字段名不强制固定，但语义须清楚：

- `minor_stars_by_palace`
- `auxiliary_stars`
- `flow_stars`
- `palace_stars`
- `topic_lens.target_palace`
- `topic_lens.involved_palaces`
- `topic_lens.opposite_palaces`
- `time_layers`

若只有原始盘面，先抽取涉及 topic 的宫位、三方四正、对宫与流限层，再路由。

## Always Load

- `source-cards/manifest.md`
- `source-cards/group/*.md` 中命中的类别卡
- `rules/core-always/composition-grammar.md`

## Star Dispatch

| 命中星曜 | 小卡 |
|---|---|
| 左辅 / 右弼 | `source-cards/minor-star/zuofu-youbi.md` |
| 天魁 / 天钺 | `source-cards/minor-star/tiankui-tianyue.md` |
| 文昌 / 文曲 | `source-cards/minor-star/wenchang-wenqu.md` |
| 禄存 / 天马 | `source-cards/minor-star/lucun-tianma.md` |
| 擎羊 / 陀罗 | `source-cards/minor-star/qingyang-tuoluo.md` |
| 火星 / 铃星 | `source-cards/minor-star/huoxing-lingxing.md` |
| 地空 / 地劫 / 天空 / 截空 / 旬空 | `source-cards/minor-star/kongyao.md` |
| 红鸾 / 天喜 | `source-cards/minor-star/hongluan-tianxi.md` |
| 咸池 / 大耗 / 天姚 / 沐浴 | `source-cards/minor-star/taohua-misc.md` |
| 三台 / 八座 / 龙池 / 凤阁 / 台辅 / 封诰 / 恩光 / 天贵 / 天官 / 天福 | `source-cards/minor-star/keming-stars.md` |
| 天刑 | `source-cards/minor-star/tianxing.md` |
| 孤辰 / 寡宿 | `source-cards/minor-star/guchen-guasu.md` |
| 华盖 | `source-cards/minor-star/huagai.md` |

## Combo Dispatch

| 触发 | 小卡 | deep gate |
|---|---|---|
| 多组辅佐科名成对拱照命/官/财/迁 | `source-cards/minor-combo/baiguan-chaogong.md` | 需判断格局成立时 |
| 左右、魁钺、昌曲、台座、龙凤等只见一颗 | `source-cards/minor-combo/single-star-warning.md` | 在父母/夫妻/福德/六亲宫时 |
| 天梁 + 擎羊/天刑，或煞刑会疾厄/迁移/父母 | `source-cards/minor-combo/tianliang-yangxing.md` | 疾厄、官非、手术题必须 deep |
| 文昌/文曲化忌，或流昌/流曲叠忌 | `source-cards/minor-combo/changqu-huaji.md` | 文书、财帛、官非、破财题 |
| 禄存 + 天马 | `source-cards/minor-combo/lucun-tianma.md` | 财帛/迁移/远方求财题 |
| 红鸾天喜 + 咸池/大耗/天姚，会夫妻/福德/命 | `source-cards/minor-combo/taohua-stack.md` | 婚恋/欲望/精神享受题 |
| 巨门与文曲/红鸾强关联 | `source-cards/minor-combo/jumen-speech-modifiers.md` | 口才、幽默、文雅、绕弯等表达质感 |

## Deep Rules

读 deep 的标准：

- 小星将成为 primary 或 supporting 证据。
- 需要判断“成对 vs 单星”。
- 需要安星法定位或流曜定位。
- 需要从实例模式判断触发条件。
- 小星信号会改变吉凶、强弱、人物或时间层。
- 小星取象来自“与某主星/组合放在一起”时。

## 输出

每张卡输出时都标：

- `role`: primary / supporting / background / excluded
- `path`: 小星如何连接 topic 或飞化链
- `base_combo`: 小星修饰的主星/组合
- `source`: `[wtz]`
- `boundary`: 不能由此推出什么
