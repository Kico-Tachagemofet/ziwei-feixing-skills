---
name: ziwei-render
description: 紫微斗数 Render Skill。把 composition.md (Stage 3 骨架) + topic-findings 渲染成中文散文 deliverable（纯翻译层，不做技术分析）。Pipeline 位置: Stage 4 (render)。必读 composition.md, 朋友代求测 (friend-proxy) 模式应用隐私规则。
---

# 紫微斗数 Render Skill

> v1.4 (2026-05-24) — 从 Z08-kico-mom 实践: composition 作骨架 + 高对比度结构化写法 + cross-reference 纪律 + subject-context 处理 + 隐私规则
> v1.3 (2026-05-23) — 星性原书定性做骨架 + 旺度不进正文判据 (Z04 翻车修复)
> v1.2 (2026-05-22) — 格式对齐 hellenistic-render-unguarded: 每 finding 独立小节 + inline 技术依据 + 不压缩
> v1.1 (2026-05-20) — 初版
> Pipeline 位置: Stage 4 (render)，见 pipeline-spec.md
>
> ⚠️ Cross-topic Composition (Stage 3) 在 ziwei-feixing-core-v2 做，本 skill 不重复做技术整合，但**必须读 composition.md 作骨架** (v1.4 新增)
> 原因: render context 里混入技术分析 → 术语污染 → 散文变"chart fact 换中文马甲" (hellenistic render v1.0→v1.1 教训)

## 职责

读 source-level findings → 写 reader-facing 中文散文。

**做**：
- 把每个 topic 的 findings 渲染成读者能直接看的中文散文
- 每句话不懂紫微也能说"准"或"不准"
- 飞化术语全部下沉到技术块（audit trail）
- 过 render-checklist + anti-cliche 双层质检

**不做**：
- 不产新 finding（那是 feixing-core-v2 的事）
- 不改动 finding 的断语方向（可以调措辞，不能改结论）
- 不读 subject-context.md（除非渲染阶段明确需要校准表述）

## 输入

| 文件 | 来源 | 必需 |
|---|---|---|
| `composition.md` | ziwei-feixing-core-v2 (Stage 3) | **是** (v1.4: render 骨架, 必读) |
| `topic-findings/<slug>.md` | ziwei-feixing-core-v2 | 是 |
| `topic-findings/<slug>-lens.md` | ziwei-topic-lens | 可选（看 boundaries） |
| `chart-stage1.md` | ziwei-reader | 可选（技术块引用） |
| `cross-topic-structural.md` | ziwei-feixing-core-v2 (Stage 1.5) | 可选（全盘结构参考） |
| `pre-validation-R1.md` | ziwei-feixing-core-v2 (Stage 1.5) | 可选（仅 debug/training 模式产出时可读，默认不存在） |
| `subject-context.md` | ziwei-reader | 渲染阶段可读（校准表述 + subject-context informed sections handling, 见 §subject-context handling） |

## 输出

```
reader/
  01-character.md      — 性格
  02-partnership.md     — 感情
  03-career.md          — 事业
  04-wealth.md          — 财运
  05-health.md          — 健康
  06-spiritual.md       — 精神
  ...
report.html            — 可选，合并所有 reader/ 文件
```

## 渲染流程

### Step 0: 读 composition.md 作骨架 (v1.4 新增)

**必做**。Stage 3 composition.md 是 render 的整体骨架。读法:

- **§1 全盘一句话** → 用作 reader 的总 hook (HTML title 段下的 condensed synthesis)
- **§2 paradox 主锚** → 哪些 finding 必须显式 cross-reference (出现在多 topic 里)
- **§4 关键人生主线** → 每 topic 的完整故事整合方向 (不是单 topic 复述, 是跨 topic 故事链)
- **§5 张力点** → reader 各 topic 之间张力的明示素材
- **§6 关键结构标签** → 每 topic bold hook 句的素材库
- **§7 整体生命形态 synthesis** → reader 整体画像 (e.g., "中转型枢纽命格")
- **§8 出口指向** → 给后续 Q&A 的指南 (本 skill 不输出 Q&A, 但保留 hand-off)

⚠️ **composition.md 缺失 → 停止 render, 提示先跑 Stage 3** (不要 fallback 自己整合, 那会破坏 Stage 3/4 分层)

### Step 1: 读 findings，提取直断候选

读 `topic-findings/<slug>.md`，对每条 finding 优先看：
- **Interpretive Kernel**（Main Scene / Secondary Scene / Do Not Render）— v3.1 新增的 render 接口
- Finding Statement（Phase D 的直断候选）
- Confidence 级别
- Boundaries

有 Interpretive Kernel 时，Main Scene 和 Secondary Scene 是已经翻译好的生活画面，直接用。
如 finding 含双象六组路径（C.2a），六组解义本身就是生活画面，Main Scene 会更具体。

### Step 1.5: 生活判断摘要（mandatory）

对每条 finding 提取**可验证生活判断点**。不是摘要句，是打勾/打叉清单：

```
F-PART-001:
  □ 感情里想太多、心思容易缠在关系上
  □ 精神层的纠缠比外在事件更明显
  □ 变化来了有突然性，之前催不动
  □ 事业/社会交际方向给感情带新缘
```

这个清单是渲染的**骨架**——散文里的每句实质内容都应该能追溯到某个判断点。写不出判断点的 finding = 太抽象，需要回 feixing-core 补具体。

### Step 2: 读规则

读两份规则：
1. `rules/render-checklist.md` — 内容层规则（写前检查）
2. `rules/anti-cliche-ziwei.md` — 文体层规则（写后检查）

### Step 3: 写散文

**格式对齐 hellenistic-render-unguarded。** 每条 finding 独立小节，不压缩合并。

每个 topic 一个 reader/ 文件。格式：

```markdown
# [Topic 中文名] — [case name]

> source: topic-findings/[slug].md ([N] findings)
> render discipline: render-checklist.md + anti-cliche-ziwei.md
> [date]

---

**[1-2句 bold hook——抓全 topic 核心，不用术语]**

---

### [小节标题——描述性人话，不是 finding 编号]

<!-- 生活判断: [可验证判断点清单，分号分隔] -->

[2-3段叙事散文]

每句话读者不懂紫微也能说"准"或"不准"。
confidence: medium 的 finding 用对冲语气。
source gap 的地方诚实保留模糊，不伪装确定。

技术依据: [inline 引用——飞化链/edge#/RC-ID/CG-ID/combo名/旺度/finding ID + confidence。不折叠不藏。]

---

### [下一个 finding 的小节标题]

<!-- 生活判断: ... -->

[2-3段]

技术依据: ...

---
```

#### 格式硬规则（v1.2 新增）

1. **每条 finding → 独立 `###` 小节**，标题用描述性人话（"给人的印象和本质"），不用 finding 编号（"F-CHAR-001"）
2. **每小节末尾 inline `技术依据:`**——飞化链/edge#/RC-ID/CG-ID/combo名/旺度/finding ID + confidence level。不用 `<details>` 折叠，不藏到底部
3. **每小节 `<!-- 生活判断: -->` HTML 注释**——可验证判断点清单，写在小节标题和正文之间
4. **渲染 = 翻译不压缩**——finding 里的每个具体画面/场景/信号/辅星/小星/碰撞/因果組都要进入 reader，不能为了"简洁"丢细节。**压缩 = 杀死辨识度**
5. **开头 bold 一句话 hook**——用 `**...**` 包裹，抓全 topic 核心
6. **不合并 findings**——5 个 finding 写 5 个小节，不压成 2-4 段。finding 之间可以有叙事衔接，但每个 finding 的信息量必须完整保留

#### 旧格式（已弃用）

~~2-4段压缩 + 底部 `<details>` 折叠技术依据~~ — Z01-astarion 首次渲染翻车：5 个 finding 压成 4 段，丢了辅星/小星/碰撞/因果組细节。用户 反馈"不够详细，还不如 finding 详细""没做引用"。重写对齐希腊化格式后 用户 确认"这个输出才是读了就知道在说的是 Astarion"。

#### 星性画面做骨架（v1.3 新增 — Z04-anonymous 翻车修复）

正文的实质内容以 star/combo 的**原书定性**为骨架，飞化链提供方向底色。

**骨架层（必须出现在正文）：**
- 命迁线 combo 画面（原书逐宫研究的白话版）
- 生年四化落宫的 combo 画面
- 太极点相关宫位的主星+辅星+小星**各自角色**（每颗小星一句具体角色，不只列星名）
- 飞化链用"送了什么/收了什么/回来什么"的**人话**解释

**方向层（正文可以出现但术语下沉）：**
- 因果组的结构用"A方向给B方向带来困扰，同时给C方向带来缘起"
- 碰撞用"碰上了先天的XX=先有XX后有XX"

**禁止层：**
- **旺度不进正文**。许铨仁飞星派不主看旺度，有争议。正文不以旺度作判据（"旺地抗忌""庙地正面""陷=力量弱"全禁）。技术依据行可作事实记录。

**参考级别：** Z01-astarion reader/01-character.md — 5 finding 5 小节，每小节 4-6 段，star/combo 原书定性做骨架，每颗小星有具体角色，飞化链用"给出什么/回来什么"人话。

**写每小节前**过 render-checklist。**写每小节后**过 anti-cliche。

### Step 4: Cross-topic 收尾 (v1.4 强化)

所有 topic 写完后：
1. 检查跨 topic 是否有重复段落（同一结构在不同 topic 重复渲染）
2. 检查理则三/五等跨 topic 发现是否有统一的收尾段落
3. **必须有 cross-reference, 不允许 7 个 topic 文件孤立** (v1.4):
   - 每 topic 与其他 topic 联动的部分必须显式引用 (如 "这一点在'婚姻与配偶'那一节已详细说"; "这一条与'事业方向'的 Z 信号呼应")
   - composition.md §4 主线/§5 张力的 cross-topic 整合点, 在对应 topic 必须出现至少 1 个 cross-ref
   - 整体感来自 cross-ref 网络, 不是孤立 topic 累加
4. 生成 report.html 合并（用 `.tech` class 渲染引用行，深色主题）

### Step 5: Subject-context handling (v1.4 新增)

Subject-context.md 在 render 阶段可读, 但有严格用法:

**Stage 2 finding 整合的 subject-context informed sections**:
- composition.md / topic findings 里如有 `✨ subject-context informed` mark 的段落 → 已经在 Stage 2/3 完成了"subject context 反查盘面 + N 线共振"整合
- Render 这些段落时 **保留共振结构** (X 线汇聚清单/RC 对照表) 但 **strip 来源标注**
- 不写"用户 提供的 subject-context...", 不写"经命主验证...", 改为客观描述 (e.g., "命主已知人生事件 + 盘面盲推结构 100% 命中验证" 或直接陈述)

**Stage 4 render 时新读到的 subject-context**:
- 仅用于校准措辞精度 (e.g., 调整指代/性别/年龄表述)
- 不能引入 Stage 2/3 没说过的新断语
- 不能 contaminate finding 结论方向

### Step 6: Privacy 规则 (v1.4 新增, 引用 memory)

适用所有 friend-proxy 模式 (代为求测他人的盘):

**3 条隐私规则** (详见 memory `ziwei_render_html_privacy_rules.md`):

1. **不提 subject-context 信息提供者** — Reader 看到的应是结构性论断, 不是元方法论说明
2. **命名/标题用中性** — HTML title 用 "[盘主性别+生年] 紫微斗数命盘解读" 之类, 不出现求测人名字
3. **去掉署名** — 不写 "by CC / 紫微飞星派 / skill 名称" 等 attribution

**Why**: Render 给盘主看, 不给求测代理人看; 提及来源/署名会让盘主感觉"被三方讨论"。

**适用判断**: subject-context.md 中 "Subject Type" 或 "盘主关系" 显示 "他人代求" / "朋友/家人代查" → 触发隐私规则

## 核心原则

### 1. 每句话都是生活判断

> "感情里先天带一种放不下的纠缠——心思容易缠在伴侣/关系上面，越想越深。"

这句话盘主能回答"准"或"不准"。

> "天机化忌落夫妻宫，形成缘灭收束在思虑变动层面。"

这句话盘主不知道怎么回答。**禁止出现在正文。**

### 2. Chart-tech 分层

技术引用放在每小节末尾 `技术依据:` 行（inline，不折叠）。**正文零飞化术语，但星性描写必须保留（见下）。技术依据行可以用术语。**

**正文**允许出现（已进入日常语言）：
- 命宫、夫妻宫、财帛宫等宫名（但不说"丙辰"干支）
- 大运、流年（但不说"虚岁29"具体数字，说"26到35岁这十年"）

**正文**允许出现（星性描写层 — v1.3 新增）：
- 星名 + 原书白话定性（"破军在福德=此生难得清闲""廉贞天府=老谋深算、府城深"）
- 小星名 + 具体角色（"天月是病星""天哭=身体不好时情绪化"）
- 组合通性人话版（"动中求财积蓄成富"）
- 飞化链人话版（"送去了困扰""回来的是纠缠"）

**正文**禁止出现的术语：
- 化禄/化权/化科/化忌
- 飞宫、理则、四化
- 生年象、自化、视同自化
- RC-ID、Phase A/B/C/D
- 任何干支代号（丙、戊、辛等）
- **旺度判断**（"旺地抗忌""庙地正面发挥""陷=力量弱"等）— 许铨仁飞星派不主看旺度，有争议，不作正文判据

**技术依据行**允许出现：飞化链完整写法（命(壬)忌武曲→交友）、edge#、RC-ID、CG-ID、combo名、旺度标注、finding ID + confidence。这一行是给同行看的 audit trail。

### 3. 来源宫上下文不丢

Finding 说"感情的忌入田宅"，渲染时不能变成"家宅有压力"——必须保持"因为感情的纠葛，内心/家里才有那种不安宁"。来源宫=内容色彩，丢了就变成 cookbook。

### 4. Confidence 转化为语气

| confidence | 散文语气 |
|---|---|
| high | 直陈："你天生有……"、"这十年……" |
| medium | 对冲："倾向于……"、"很可能……"、"如果……的话" |
| low | 诚实保留："这一点不太确定，可能……" |
| gap | 不写进正文，或明确说"这方面信息不够，不下判断" |

### 5. R1 校准（仅限 R1 存在时）

R1 (pre-validation-R1.md) 在 v3.0+ 默认不产出（debug/training only）。如果存在，校准结果（哪些准、哪些不准）影响措辞。如果不存在，跳过此步。

### 6. 不改 finding 方向

渲染可以调措辞、调语气、调详略，但**不能改变 finding 的断语方向**。如果 finding 说"无红灯"，渲染不能暗示"其实有隐患"。如果 finding 说"有凶底色但有缓冲"，渲染不能只写"有凶底色"省掉缓冲。

## 质量检查

每段写完后双层检查：

### 内容层（render-checklist）
- [ ] 每句话是否是生活判断（能回答准/不准）
- [ ] 来源宫上下文是否保留
- [ ] confidence 是否正确转化为语气
- [ ] R1 校准是否体现（仅 R1 存在时）
- [ ] 有无术语外露
- [ ] 有无改变 finding 方向
- [ ] source gap 处是否诚实
- [ ] **#11 高对比度结构是否用了 N 线共振/RC 对照表/cross-ref** (v1.4 新增)
- [ ] **subject-context informed sections 的来源标注是否已 strip** (v1.4 新增)
- [ ] **隐私规则: friend-proxy 时命名/署名/来源是否中性化** (v1.4 新增)
- [ ] **cross-reference 网络: topic 之间是否有显式引用** (v1.4 新增)

### 文体层（anti-cliche）
- [ ] 有无紫微 cookbook 套话
- [ ] 有无抽象属性作主语
- [ ] 有无"管束"等机制层词汇
- [ ] 有无模糊泛化（"需要注意"、"有一定的"）
- [ ] 有无短句碎片化
- [ ] 有无"不是A而是B"过度使用

## First-invocation Flow (v1.4)

1. **Step 0 必先**: 确认 `composition.md` 存在 (Stage 3 由 ziwei-feixing-core-v2 产出, 不在本 skill)。**缺失 → 停止 render, 提示先跑 Stage 3**, 不要 fallback 自己整合 (会破坏 Stage 3/4 分层)
2. 读 composition.md (§1-§8) 作骨架 — 见 Step 0
3. 确认 topic-findings/ 下有哪些 topic
4. 按 topic 顺序逐个渲染: Step 1 → 1.5 → 2 → 3 (引用 composition.md 各段)
5. Step 4 cross-topic 收尾 (cross-reference 网络 + 去重 + 统一收束)
6. Step 5 subject-context handling (Stage 2/3 已 mark 的段保留共振结构 strip 来源)
7. Step 6 privacy 规则 (friend-proxy 模式触发 → 中性命名 + 去署名 + 不提来源)
8. 报告: N 个 topic 渲染完成, reader/ 文件列表 + report.html
