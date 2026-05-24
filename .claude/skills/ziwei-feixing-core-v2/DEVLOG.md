# 紫微飞星 Core 开发日志

> 记录 issue → investigation → solution → outcome
> 回滚参考: `.claude/backup/` 存有关键版本快照

---

## 2026-05-23

### ISSUE-001: v3.4 瘦身后 Z03 产出质量断崖式下跌

**触发**: SKILL.md 从 v3.3 (1168行) 瘦身到 v3.4 (203行)，Z03-kico new session character finding 出现多个严重问题

**症状**:
- 生年四化完全未分析（来因宫=年干"不当后天交易"被误读为"不提生年四化"）
- Deep card 未读但标 high confidence
- Character 只有 3 条 finding（缺福德/疾厄覆盖）
- "视同自化禄入疾厄=缘起"——四化取象直接搬通论不过宫位
- 4忌冲命被当凶兆（实际是天同太阴文昌坐迁移的四化表算术必然）
- Audit skill 给了 PASS_WITH_WARNINGS（辩护律师模式）

**根因分析**:
1. v3.4 砍掉了"为什么"层，只留"做什么" → model 跳步
2. 规则引用表删了 → 新窗口不知道读哪些 core-always 文件
3. 加载架构删了 → 不知道 deep 什么时候必读
4. 核心纪律移到 audit-only → 生产时无 guardrail
5. Step 0 schema 悬空 → pipeline-spec 引用 SKILL.md 但内容已删
6. topic-lens 没有 character reference → scope 随机
7. 缺忌汇聚规则 → 高频忌星汇聚被当特异信号
8. 缺先天体步骤 → 命迁线+生年四化分析无保底机制

**Solution 清单**:

| # | 修复 | 文件 | 状态 |
|---|---|---|---|
| S1 | SKILL.md 补回加载架构+Tier 1 文件表 | SKILL.md | ✅ |
| S2 | SKILL.md 补回生产硬约束 7 条 | SKILL.md | ✅ |
| S3 | Step 0 schema 外置 | rules/step0-schema.md (NEW) | ✅ |
| S4 | pipeline-spec 引用修正 | pipeline-spec.md | ✅ |
| S5 | Phase C 教学范例(脱敏 few-shot) | SKILL.md | ✅ |
| S6 | 四化断语前强制回查 | SKILL.md (v3.4 已有) | ✅ |
| S7 | ziwei-reader 出口门禁 | ziwei-reader/SKILL.md | ✅ |
| S8 | 前置缺失恢复逻辑 | SKILL.md | ✅ |
| S9 | 忌汇聚≠凶度规则 | feixing-minimal-grammar.md | ✅ |
| S10 | 先天体 Step 0.7 spec | rules/step07-innate-body.md (NEW) | ✅ |
| S11 | Character topic reference | topic-lens/references/character.md (NEW) | ✅ |
| S12 | Pipeline 总览更新 (加 Step 0.7) | pipeline-spec.md | ✅ |
| S13 | innate-body.md 加入输入表+恢复表 | SKILL.md | ✅ |
| S14 | Audit skill v2 (检察官模式) | ziwei-finding-audit/SKILL.md | ✅ |

**Outcome**: 待 Z03 重跑验证

**回滚点**: `.claude/backup/SKILL-v3.3-full-1168lines.md`

---

### ISSUE-002: 四化取象不过宫位（禄在疾厄=缘起）

**触发**: Z03 health topic finding 中 "视同自化禄入疾厄=缘起"

**症状**: 通论义"禄=缘起"直接搬到疾厄宫，没有宫象合参（应为"禄在疾厄=脾气好/体壮"）

**根因**: flow cards (flow/into-*.md) 可能太薄，缺逐宫四化具体取象速查；SKILL.md 没有强制回查机制

**Solution**: SKILL.md v3.4 新增"四化断语前强制回查"section（9条checklist，含宫象合参示例）。audit v2 新增 B5 检查项。

**Outcome**: 规则层已补，待验证执行效果。flow cards 厚度待后续评估。

---

### ISSUE-003: 忌汇聚被当凶兆

**触发**: Z03 character F-CHAR-002 "4忌冲命=中凶"

**症状**: 天同太阴文昌坐迁移 → 庚/乙/辛三干忌自然飞此 → 4忌汇聚是四化表算术必然。model 只数忌不看来源、不查结构必然性。

**根因**: feixing-minimal-grammar 缺忌汇聚规则（与 N象一星 平行的问题）

**Solution**: feixing-minimal-grammar.md 新增"忌汇聚≠凶度"section: 6颗高频忌星表 + 4点判断法。audit v2 新增 B3 检查。

**Outcome**: ✅ 规则已加

---

### ISSUE-004: Audit 辩护律师模式

**触发**: Z03 character audit 给 PASS_WITH_WARNINGS，但 finding 质量明显不达标

**症状**: audit 为每条 warning 写一段辩护词（"短卡已足够"、"不在此规则覆盖范围"、"不构成误判风险"）。只检查程序不检查内容。

**根因**: v1 audit 设计为单层程序检查，没有内容质量层；语气设定缺失。

**Solution**: audit v2 双层架构 (A程序+B内容) + 检察官语气约束（禁止自我辩护）。新增 B1覆盖度/B2先天体/B3忌汇聚/B4 confidence一致/B5 四化宫象合参 五个内容检查。

**Outcome**: v2 回测 Z03 → 预计 4 BLOCKER → FAIL

---

### ISSUE-005: 先天体缺失导致每个 topic 重复分析命迁但都做不到位

**触发**: Z03 character 缺生年四化、缺命迁 deep，但每个 topic 其实都需要命迁交互

**症状**: 命迁线+生年四化不是 topic，是所有 topic 的先天底色。但没有专门步骤保底 → 各 topic 各做各的 → 都做不到位

**Solution**: Step 0.7 先天体 (innate-body.md) — 命迁deep必读+生年四化4落宫+来因宫+结构速写+open threads。所有 topic 引用此文件不重做。character reference 固定5面(印象/内在/脾气/先天方向/压力)。

**Outcome**: 架构已建，待验证

---

### ISSUE-006: Z03 new session topic-lens 无 character reference

**触发**: Z03 invoke topic-lens for character，没找到 reference → "building from scratch"

**症状**: character 的 scope 无定义 → model 自己决定覆盖面 → 只做了 3 条

**Solution**: `ziwei-topic-lens/references/character.md` 新建，固定5面+必引用innate-body+banned misreads

**Outcome**: ✅ reference 已建

---

## 版本快照索引

| 版本 | 日期 | 备份位置 | 说明 |
|---|---|---|---|
| v3.3 full | 2026-05-23 | `.claude/backup/SKILL-v3.3-full-1168lines.md` | 瘦身前完整版 |
| v3.4 patched | 2026-05-23 | 当前 SKILL.md (~250行) | 瘦身+4洞修补+few-shot+先天体 |

---

## 待验证 (下次 session)

- [ ] Z03 重跑 character: 先天体→character 全流程，验证 5 面覆盖+生年四化+deep 读取
- [ ] Audit v2 回测: 对旧 Z03 findings 跑 v2 audit，确认 FAIL 判定
- [ ] 新窗口 zero-shot 测试: 无任何 context 的 session 能否正确走 Step 0 → 0.7 → Stage 2
- [ ] flow cards 厚度评估: 逐宫四化取象是否需要扩充 flow/deep/
