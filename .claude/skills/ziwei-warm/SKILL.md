---
name: ziwei-warm
description: 紫微斗数 session warm-up。一次性读完所有 deep card（主星+辅星+宫位+四化+飞星骨架+小星+render规则），后续 skill 不重复读。Use when 用户 says '读 deep card'、'warm up'、'加载紫微'、'紫微预热' 或开始任何紫微斗数分析/渲染 session。
---

# 紫微斗数 Session Warm-up

> v1.0 (2026-05-24) — 从 Z01 render session 实践提炼：不预读 deep card 的 render 产出是 cookbook 级
> 触发: `/ziwei-warm` 或 用户 说 "读 deep card / warm up / 加载紫微 / 紫微预热"
> 职责: 一次性加载全部参考知识到 context，后续 skill (feixing-core-v2 / render / lens / audit) 直接使用，不重复读

## 为什么需要 warm-up

1. **星性原书定性是 render 的骨架** — 没读主星 deep card，渲染出来的散文缺具体画面 (Z01 教训)
2. **四化本义+飞星骨架是 Stage 2 的地基** — 没读 sihua-benyi + feixing-framework，finding 会走 cookbook
3. **小星 deep card 提供场景合成素材** — 没读 wangtingzhi addon，小星只能贴标签不能画场景
4. **宫位 deep card 提供宫象合参依据** — 没读宫位 deep，只靠宫名查表
5. **一次读完 > 每个 skill 分别读** — 避免同一个 session 跑 3 个 skill 读 3 遍相同的卡

## 加载清单

### Layer 1: Codex 主星 deep (24 颗 — 14 主星 + 10 辅煞星)

路径: `source-cards\star\deep\`

**14 主星:**
1. ziwei.full.md — 紫微 (帝王/官禄主/解厄)
2. tianji.full.md — 天机 (兄弟主/智慧/驿马)
3. taiyang.full.md — 太阳 (官禄主/父夫男/光明)
4. wuqu.full.md — 武曲 (财帛主/将星/刚)
5. tiantong.full.md — 天同 (福德主/温和/福)
6. lianzhen.full.md — 廉贞 (官禄主/囚星/次桃花)
7. tianfu.full.md — 天府 (库星/财星/近化科)
8. taiyin.full.md — 太阴 (财田主/母妻女/快乐)
9. tanlang.full.md — 贪狼 (祸福主/桃花主)
10. jumen.full.md — 巨门 (是非/口舌/暗星)
11. tianxiang.full.md — 天相 (印星/衣食/制廉贞恶)
12. tianliang.full.md — 天梁 (荫星/父母星/中康)
13. qisha.full.md — 七杀 (将星/大杀神/血光)
14. pojun.full.md — 破军 (破耗/祸福/司夫子奴)

**10 辅煞星:**
15. zuofu-youbi.full.md — 左辅右弼 (成对助力/"再一次")
16. wenchang-wenqu.full.md — 文昌文曲 (文魁/文书/科名)
17. kuiyue.full.md — 天魁天钺 (阳贵阴贵)
18. huoling.full.md — 火星铃星 (近化权/急躁)
19. yangtuo.full.md — 擎羊陀罗 (刑伤/纠缠)
20. tianxing.full.md — 天刑 (规则/法/手术)
21. tianyao.full.md — 天姚 (风雅桃花)
22. tianma.full.md — 天马 (驿马/财马)
23. hongluan-tianxi.full.md — 红鸾天喜 (喜庆/姻缘形态)
24. lucun.full.md — 禄存 (财寿/解厄/主孤)

### Layer 2: Codex 宫位 deep (13 颗)

路径: `source-cards\palace\deep\`

1. 00-general.full.md — 宫位总论
2. ming.full.md — 命宫
3. xiongdi.full.md — 兄弟宫
4. fuqi.full.md — 夫妻宫
5. zinv.full.md — 子女宫
6. caibo.full.md — 财帛宫
7. jie.full.md — 疾厄宫
8. qianyi.full.md — 迁移宫
9. nupu.full.md — 交友宫(奴仆)
10. guanlu.full.md — 官禄宫
11. tianzhai.full.md — 田宅宫
12. fude.full.md — 福德宫
13. fumu.full.md — 父母宫

### Layer 3: Skill 内 deep card (飞星骨架+四化+语法)

路径: `.claude/skills/ziwei-feixing-core-v2/rules/`

1. deep/sihua-benyi.md — 四化本义底卡
2. deep/feixing-framework.md — 飞星派断盘骨架
3. core-always/sihua-minimal-semantics.md — 四化最小语义
4. core-always/feixing-minimal-grammar.md — 飞星最小判读语法
5. core-always/palace-minimal-map.md — 十二宫最小地图
6. session-warm/star-skeleton.md — 十四主星骨架
7. session-warm/palace-skeleton.md — 十二宫骨架

### Layer 4: 王亭之小星 addon deep (20 颗)

路径: `.claude/skills/ziwei-wangtingzhi-minor-stars-addon/source-cards/deep/`

**11 minor-star deep:**
1. minor-star/zuofu-youbi.full.md
2. minor-star/tiankui-tianyue.full.md
3. minor-star/wenchang-wenqu.full.md
4. minor-star/lucun-tianma.full.md
5. minor-star/qingyang-tuoluo.full.md
6. minor-star/huoxing-lingxing.full.md
7. minor-star/kongyao.full.md
8. minor-star/taohua-misc.full.md
9. minor-star/keming-stars.full.md
10. minor-star/tianxing.full.md
11. minor-star/guchen-guasu.full.md
12. minor-star/hongluan-tianxi.full.md

**7 minor-combo deep:**
13. minor-combo/baiguan-chaogong.full.md
14. minor-combo/single-star-warning.full.md
15. minor-combo/tianliang-yangxing.full.md
16. minor-combo/changqu-huaji.full.md
17. minor-combo/lucun-tianma.full.md
18. minor-combo/taohua-stack.full.md
19. minor-combo/jumen-speech-modifiers.full.md

**3 method deep:**
20. placement-methods.md
21. inference-principles.md
22. examples-index.md

### Layer 5: Render 规则 (2 颗)

路径: `.claude/skills/ziwei-render/rules/`

1. render-checklist.md — 内容层规则
2. anti-cliche-ziwei.md — 文体层规则

## 加载流程

⚠️ **并行读取**: 每个 Layer 内的文件互相独立，尽可能并行 Read。Layer 之间也可以并行。

### Step 1: 并行读 Layer 1 (24 主星+辅星 deep)
全部 24 个文件并行 Read。

### Step 2: 并行读 Layer 2 (13 宫位 deep)
全部 13 个文件并行 Read。

### Step 3: 并行读 Layer 3 (7 skill 内 deep)
全部 7 个文件并行 Read。

### Step 4: 并行读 Layer 4 (22 wangtingzhi deep)
全部 22 个文件并行 Read。

### Step 5: 并行读 Layer 5 (2 render 规则)
2 个文件并行 Read。

### Step 6: 报告加载结果

```
紫微 warm-up 完成:
- Layer 1: [N]/24 主星+辅星 deep ✓
- Layer 2: [N]/13 宫位 deep ✓
- Layer 3: [N]/7 飞星骨架+四化 ✓
- Layer 4: [N]/22 王亭之小星 deep ✓
- Layer 5: [N]/2 render 规则 ✓
总计: [N]/68 deep card 已加载
```

**如有文件读取失败，列出失败文件名，不 block 其他文件。**

## 加载后的 session 行为

1. **后续 skill invoke 不重复读这些文件** — feixing-core-v2 / render / lens / audit 可以直接引用 context 中已有的星性/宫位/四化知识
2. **只有 case-specific 的文件需要额外读** — chart-stage1.md / topic-findings / composition.md 等
3. **如果 session 太长 context 压缩了 deep card 内容** — 重跑 `/ziwei-warm` 补读

## 不加载的文件 (明确排除)

- **短卡 (session-warm/)** — 只是索引入口，deep card 已经包含完整内容
- **Codex rules deep** (flying-palace-rules.full.md / ti-yong.full.md / palace-star-symbol.full.md) — 规则层已在 skill rules/ 内覆盖
- **Codex timing deep** — 当前 timing skill deferred (2026-05-18)
- **Codex archived-methods deep** — 已整合进 topic-methods/
- **Codex reference deep** (calendar/chart-making/stems-branches) — 排盘参考，分析/渲染不需要
