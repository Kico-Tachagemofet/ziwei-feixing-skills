# Stage 3 Composition Schema

> v1.0 (2026-05-24) — 从 Z08-kico-mom 实践提炼
> Pipeline 位置: Stage 2 finding production → **Stage 3 composition** → Stage 4 render
> 输出: `composition.md`

## 用途

Stage 2 所有 topic findings + audit 完成后, Stage 4 render 之前, **必产**一个 cross-topic composition file. 用途:

1. 整合多 topic 出统一命主画面 (单 topic 看不出的 cross-topic 真发现)
2. 给 Stage 4 render 提供骨架 (避免 7 个 topic 文件孤立)
3. 给后续 Q&A 模式提供命主全盘定位参考

## 8 段标准结构 (硬性)

### §1. 全盘一句话
- 1-3 句 condensed synthesis
- 抓全盘最特殊结构 + 命格定调 + 主线轮廓
- 不出现术语, 不出现宫位代号
- 例: "命主一生因由通过事业 + 婚姻共同体现, 但事业是终点, 婚姻是结构桥梁; 内空外满; 早婚不顺但离婚后命主重心更纯粹回归事业, 走六阳贵格而非富格"

### §2. paradox 主锚 / 全盘最特殊结构
- 列出"全盘只此一处"的结构 (如来因宫=年干 + 三重重叠, 三重 0 互飞, 体之体冲命, 等)
- 这是 cross-topic 真发现, 单 topic 看不出
- 必含: 结构含义 + 对命主整体定调

### §3. 性格底色 (N 层合参)
- 整合 character topic 多 finding 出"人格 N 层"画面
- 通常: 命迁线 / 内心 / 脾气 / 行为模式 = 4 层
- 每层 1-2 句

### §4. 关键人生主线 (按 topic 整理)
- 每 topic 一条"主线", 不只是 finding 复述, 是**整合 cross-topic 信号的完整故事**
- 例: 婚姻线 = 早婚 → 配偶画像 → 互动模式 → 离婚 → 后果 (而不是只列 finding statements)
- Subject-context informed sections **必须 mark `✨ subject-context informed`**
- 与其他 topic 联动的部分必须 cross-reference

### §5. 张力点
- 至少 3 个 cross-topic 真张力 (单 topic 看不出)
- 形式: "X vs Y → 张力解决方式: Z"
- 例: 内空 vs 外满 / 稳重 vs 变动 / 贵格 vs 虚花 / 多承压 vs 多辉映

### §6. 关键结构标签 (供 render 引用)
- 8-12 个标签 (按 topic 维度)
- 每个标签 = 1-2 句话的 reader-facing label
- 是 Stage 4 render 写 bold hook 的素材库

### §7. 整体生命形态 synthesis
- 1-2 句话给命主下一个"画像 label" (e.g., "中转型枢纽命格")
- label 后跟 1-2 段 deep summary
- 整体画像必须 trace 回 Stage 2 证据

### §8. 出口指向
- 给 Stage 4 render 的 hand-off 说明 (composition 是骨架, findings 是细节)
- 给 Q&A 模式的指南 (追问回到对应 Stage 2 finding)
- 给未来扩展的指南 (新 topic 应回到本 composition 检查呼应)

## 硬约束 (audit checklist)

### A 层 — composition 结构
- [A1] **覆盖度**: 所有 Stage 2 topic 都被显式整合, 无 topic 失踪
- [A2] **Fidelity**: 不引入 Stage 2 没说过的新断语. Synthesis label OK (是综合不是新断), 但具体判断必须 trace
- [A3] **Cross-topic 整合质量**: 张力点是真张力不是硬凑; 标签能 trace 到 Stage 2 证据
- [A4] **核心结构提取**: paradox 主锚 + 张力 + synthesis label 三者齐

### B 层 — composition 内容
- [B1] **Subject-context 标记**: subject-context informed sections 必须显式 `✨ mark` (Stage 4 render 时会 strip)
- [B2] **应期/事件性边界**: 应期审慎 (用"候选"/"结构必然"非"事件必然"); 不预测未来
- [B3] **断语纪律**: 无"管束"/"刑克"; 不超出 Stage 2 范围的新断; "婚姻是早段经历"等暗示性措辞 → 改为"早段已经历"等描述性
- [B4] **Render-ready**: §8 出口段明确 hand-off

## Audit 流程

Composition 写完后:
1. 调 ziwei-finding-audit skill (或人工对照本 checklist) 跑 A+B 双层
2. FAIL/WARNING 必修, **不允许 force pass 进 Stage 4**
3. 修完再 audit, 直到 PASS

## 例外情况

- **简单盘** (topic 数 < 4): §3 性格层数可缩为 2 层, §4 主线条数随 topic 数
- **无明显 paradox 主锚**: §2 改为"主要结构特征" (列前 3 个最 special 的结构)
- **subject-context 缺失**: §4 主线只写盘面盲推结构, 不假装有 subject 验证

## 产出标准

文件名: `composition.md` (放在 case 目录)
长度: 通常 10-15 KB (≈ 中等 finding 文件大小)
段落: 8 个 `##` 段 (按硬性结构)

## Stage 4 render 接口

Stage 4 render skill 读取 composition.md 作为骨架:
- 各 topic reader 文件用 §6 标签作 bold hook 素材
- §4 主线 cross-topic 整合用于 reader cross-reference
- §7 synthesis label 作为 reader 整体画像

详见 ziwei-render skill `Step 0: 读 composition.md`。
