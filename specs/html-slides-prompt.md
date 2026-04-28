# Slide Generation Prompt

## Task

基于两个 AI Agent 教学项目（AI-Agent-ZeroToOne、OpenClaw-ZeroToOne），生成一份用于团队技术分享的 reveal.js HTML 演示文稿。

## 项目源码路径

生成 slides 时需要追溯以下源码目录，提取代码片段、架构描述和文档内容：

```
# 项目 A：AI-Agent-ZeroToOne（从零到一写一个 Agent）
vendors/AI-Agent-ZeroToOne/

# 项目 B：OpenClaw-ZeroToOne（从零到一实现 AI Agent Gateway）
vendors/OpenClaw-ZeroToOne/
```

优先追溯以下材料，避免只凭概要生成：

- `vendors/AI-Agent-ZeroToOne/README.md`
- `vendors/AI-Agent-ZeroToOne/mini-agent-4j/docs/cn/`
- `vendors/AI-Agent-ZeroToOne/mini-agent-4j/src/main/java/com/example/agent/sessions/`
- `vendors/OpenClaw-ZeroToOne/README.md`
- `vendors/OpenClaw-ZeroToOne/claw-4j/light-claw-4j/docs/`
- `vendors/OpenClaw-ZeroToOne/claw-4j/light-claw-4j/src/main/java/com/claw0/sessions/`
- `vendors/OpenClaw-ZeroToOne/claw-4j/enterprise-claw-4j/docs/`
- `vendors/OpenClaw-ZeroToOne/claw-4j/enterprise-claw-4j/src/main/java/com/openclaw/enterprise/`

## 内容真实性与源码引用

- 生成前必须先阅读源码和项目文档，不要只根据本 prompt 的概要推断。
- 所有 Java 代码片段必须来自真实源码，禁止虚构类名、方法名、字段名、配置项或调用链。
- 每个代码页必须在页脚或代码标题中标注来源：`相对路径:类/方法`；如果能定位行号，也标注行号。
- 代码片段允许删节，但必须用 `// ...` 或 `/* ... */` 明确标出省略位置，不能改变关键语义。
- 架构图必须能追溯到源码模块、文档或 README 中描述的真实结构；推断内容需要在 speaker notes 中说明“这是基于源码结构的归纳”。
- 对比两个项目时，必须说明“相同问题、不同取舍”：教学单文件实现、Gateway 多平台抽象、生产级 Spring Boot 组织方式分别解决什么问题。

## 受众

程序员团队，部分人熟悉 Agent 概念，部分人不熟悉。需要在通俗易懂和硬核深度之间平衡。

## 时长

约 1 小时。核心主讲控制在 40-46 张 slides，另提供 6-10 张 appendix 作为可跳过的深入页。Q&A 至少预留 5-8 分钟。

## 风格要求

- **标题吸引眼球**，可以用标题党风格（例如「5 行代码写一个 AI Agent」「你的 Agent 为什么总是失忆？」）
- **内容通俗易懂**：每个概念先用大白话解释，再上代码/架构
- **硬核深度**：必须包含真实代码片段（Java）、架构图（Mermaid）、设计模式分析
- **理论与实践结合**：每个核心概念都要有对应的代码实现展示
- **中文内容**，代码注释可中英混合

## 两个项目概要

### 项目 A：AI-Agent-ZeroToOne（从零到一写一个 Agent）

- **核心理念**："The Model IS the Agent"——模型本身就是 Agent，代码只是运行环境（Harness）
- **三个推论**：Agent = Loop + Tools；Intelligence is in the model；Context is the agent's memory
- **学习路径**：4 个阶段，19 个 Session
  - Phase 1（s01-s02）：基础循环与工具分发——5 行核心代码
  - Phase 2（s03-s06）：规划与知识——TodoWrite、Subagent 上下文隔离、Skill 两层注入、三级压缩
  - Phase 3（s07-s14）：持久化——权限系统、Hook 系统、跨会话记忆、系统提示词组装、错误恢复、任务 DAG、后台虚拟线程、Cron 调度
  - Phase 4（s15-s19）：多 Agent 协作——JSONL 邮箱、握手协议、自治 Agent、Worktree 隔离、MCP 插件
- **实现**：mini-agent-4j，Java 21，20 个独立可运行文件（s01-s19 + SFull），每个 Session 都是完整的单文件程序
- **关键设计模式**：Tool Dispatch Map、Self-Tracking with Nag、Process = Context Isolation、Two-Layer Knowledge Injection、Tiered Compression、JSONL Mailbox、Idle Poll + Auto-Claim、Control/Execution Plane Split

### 项目 B：OpenClaw-ZeroToOne（从零到一实现 AI Agent Gateway）

- **核心理念**：渐进式构建一个完整的 AI Agent Gateway，从 CLI 到多平台到生产部署
- **学习路径**：10 个 Session（S01-S10）
  - S01：Agent Loop——while True + stop_reason 分发
  - S02：Tool Use——JSON Schema 定义 + Handler Map 分发，4 个内置工具
  - S03：Sessions & Context Guard——JSONL 会话持久化，三级上下文溢出恢复
  - S04：Channels——多平台抽象（CLI / Telegram / 飞书），统一 InboundMessage
  - S05：Gateway & Routing——五级绑定表路由，WebSocket + JSON-RPC 2.0
  - S06：Intelligence——八层系统提示词组装，混合记忆检索（TF-IDF + 哈希向量 + MMR）
  - S07：Heartbeat & Cron——主动式 Agent 行为，心跳检查 + 定时任务
  - S08：Delivery——WAL 磁盘队列，原子写入保证消息不丢
  - S09：Resilience——三层重试洋葱（认证轮换 → 上下文恢复 → 工具重试）
  - S10：Concurrency——命名车道队列，虚拟线程，生成跟踪
- **两个实现**：
  - light-claw-4j：轻量教学版，每课一文件，纯 Java 21 无框架
  - enterprise-claw-4j：生产级 Spring Boot 3.5，87+ 类，Docker/K8s 部署
- **关键设计模式**：双表设计（Schema + Handler）、JSONL Append-Only、WAL 写入、三层重试洋葱、五级路由、混合检索、车道队列

## Slide 结构规划

核心主线建议 44-45 张。每个部分至少包含 1 张“概念白话页”、1 张“真实代码页”或“真实架构页”、1 张“设计取舍页”。复杂实现放 appendix，主线只讲最能支撑观点的切面。

### 第一部分：开场与破冰（5 张）

1. 封面：标题党主标题 + 技术副标题
2. 你每天用的 Claude Code，核心其实是一个循环
3. 什么是 AI Agent？大白话：模型在环境里循环观察、决定、行动
4. 两个项目全景图：Agent 构建教学 vs Agent Gateway 构建教学
5. 今天的路线图：Loop → Memory → Reliability → Multi-Agent → Production

### 第二部分：核心原理——Agent 的本质（8 张）

6. "The Model IS the Agent"：为什么智能主要在模型里？
7. Agent = Loop + Tools：核心循环伪代码
8. 真实代码：AI-Agent S01 Agent Loop 的最小闭环
9. stop_reason 分发：模型如何告诉 Harness 下一步做什么
10. Tool Dispatch 模式：Schema 表 + Handler 表
11. 真实代码：S02 工具定义和 handler 分发
12. 两个项目的 S01/S02 对比：同一个 Loop，不同工程边界
13. 小结：你写的不是智能本身，而是智能可行动的运行环境

### 第三部分：从 Loop 到可用智能体（8 张）

14. 为什么 5 行 Loop 很快会失控：遗忘、跑偏、无计划、上下文爆炸
15. 规划能力：TodoWrite + Self-Tracking with Nag
16. 上下文隔离：Subagent 为什么等价于“开一个干净脑袋”
17. 两层知识注入：Skill metadata 先注入，body 按需加载
18. 三级压缩：micro → auto → manual 的触发链路
19. OpenClaw 对应：JSONL Session + Context Guard
20. 多平台接入：Channel 抽象如何隐藏 CLI / Telegram / 飞书差异
21. 设计取舍：知识注入、系统提示词组装、上下文恢复解决的是同一类问题

### 第四部分：生产级 Agent（9 张）

22. 生产级 Agent 的底线：可控、可恢复、可投递、可观测
23. 权限系统：deny / mode / allow / ask 管线
24. Hook 系统：PreToolUse / PostToolUse 生命周期
25. 跨会话记忆：持久化 + DreamConsolidator
26. 系统提示词组装：AI-Agent 6 层 vs OpenClaw 8 层
27. OpenClaw 八层提示词架构图：Identity → Soul → Tools → Skills → Memory → Bootstrap → Runtime → Channel
28. 混合记忆检索：TF-IDF + 哈希向量 + MMR 重排序
29. 三层重试洋葱：认证轮换 → 上下文恢复 → 工具重试
30. 消息投递安全：WAL 写入与 tmp → fsync → atomic rename

### 第五部分：多 Agent 协作（7 张）

31. 从单兵到团队：多 Agent 不是“多开几个模型”这么简单
32. JSONL 邮箱模型：Agent 间通信的最小可靠协议
33. 握手协议：shutdown / plan approval FSM
34. 自治 Agent：idle → poll → claim → work 生命周期
35. Worktree 隔离：Git worktree 作为目录级沙箱
36. MCP 插件：JSON-RPC 2.0 over stdio 的外部工具集成
37. OpenClaw Gateway：五级路由 + WebSocket + 多 Agent 管理

### 第六部分：教学版 vs 生产版（4 张）

38. light-claw-4j vs enterprise-claw-4j：同一概念的两种工程形态
39. 教学版：每课一文件、零框架、低认知负担
40. 生产版：Spring Boot 3.5、87 个 Java 主类、Docker/K8s 部署边界
41. 从教学到生产：哪些设计决策必须改变，哪些核心模式保持不变？

### 第七部分：总结与展望（4 张）

42. 全景回顾：29 个 Session 的知识图谱
43. 核心设计模式 Top 12：从 Tool Dispatch 到 Control/Execution Plane Split
44. 两个项目的互补关系：Agent Runtime 与 Agent Gateway 的分工
45. Q&A + 资源链接

### Appendix（6-10 张，可跳过）

- A1. AI-Agent S01/S02 完整代码路径与运行方式
- A2. OpenClaw light S01-S10 源码地图
- A3. enterprise-claw-4j 包结构与核心类关系
- A4. Context Guard 与压缩策略代码细节
- A5. Delivery WAL 失败场景推演
- A6. PromptAssembler 8 层实现代码细节
- A7. CommandQueue / LaneQueue 并发细节
- A8. MCP stdio JSON-RPC 消息示例

Appendix slides 使用 reveal.js 的 `data-visibility="uncounted"` 或统一 appendix 页码样式，不计入主讲进度。

## 技术要求

### reveal.js 配置

- 使用 reveal.js 4.x CDN，加载 `reveal.css`，但不要加载任何内置 theme CSS。
- 完全自定义 CSS，视觉方向参考 Anthropic / Claude 的温暖、克制、技术感风格。
- 代码高亮使用 highlight.js 或 reveal.js highlight plugin，必须支持 Java、JSON、Bash。
- 支持演讲者备注：使用 `<aside class="notes">...</aside>`，并启用 `RevealNotes`。
- 支持概览模式：`overview: true`，按 ESC 进入。
- Mermaid 不假设 reveal.js 内置插件。通过 Mermaid CDN 直接加载 `mermaid.min.js`，在 `Reveal.initialize()` 后手动初始化并渲染 `.mermaid` 节点。
- Appendix slides 使用 `data-visibility="uncounted"` 或独立 appendix 页码样式，避免干扰主讲页码。
- 预览方式写入 HTML 顶部注释或演讲者备注：建议通过本地 HTTP server 打开，例如在项目根目录运行 `python -m http.server 8000` 后访问 `http://localhost:8000/slides.html`；不要假设 `file://` 下 speaker notes 和 CDN 资源都稳定。

### 视觉设计（Anthropic / Claude-inspired 风格）

#### 设计哲学

参考 Anthropic / Claude 给人的视觉印象，但不要声称使用官方品牌规范或官方 palette。核心理念是：

- **Warm & Human**：温暖、有人味、平易近人——用温暖色调打破「AI = 冰冷科技」的刻板印象
- **Technical Craft**：精致的技术感——排版考究、间距精确、代码优雅
- **Functional First**：功能优先，每个视觉元素都有信息传达目的，拒绝装饰性噪音

#### 色彩体系（Claude-inspired Palette）

**基础色：**

| 用途 | 色值 | 说明 |
|------|------|------|
| 主背景（深色模式） | `#141413` | 几乎纯黑但带暖调，非冷灰 |
| 次背景 | `#1a1a18` | 卡片、代码块背景 |
| 主文字 | `#faf9f5` | 暖白，非纯白 |
| 次文字 | `#b0aea5` | 中灰，用于说明性文字 |
| 淡文字/分割线 | `#e8e6dc` | 浅暖灰，用于分隔线、边框 |

**强调色（Smart Accent Cycling）：**

| 色值 | 用途 |
|------|------|
| `#d97757`（Anthropic Orange） | 主强调色——Claude 的标志性橙色，用于关键数字、核心标题、CTA |
| `#6a9bcc`（Claude Blue） | 次强调色——用于第二层级信息、链接、辅助高亮 |
| `#788c5d`（Claude Green） | 三级强调色——用于成功状态、代码行号、注释高亮 |

**强调色轮换规则：**
- 每个章节（Part）自动轮换主强调色（Part 1 橙、Part 2 蓝、Part 3 绿、Part 4 橙...）
- 同一页只使用一个语义主强调色；中性色、代码语法高亮色、状态色除外
- 代码高亮的关键行用当前章节的强调色

#### 排版体系

**字体栈（Font Stack）：**

```css
/* 标题：几何无衬线，中文回退到系统黑体 */
--font-heading: 'Poppins', 'PingFang SC', 'Microsoft YaHei', 'Noto Sans SC', sans-serif;

/* 正文：英文可带一点人文感，中文优先保证投影可读性 */
--font-body: 'Lora', 'PingFang SC', 'Microsoft YaHei', 'Noto Sans SC', Georgia, serif;

/* 代码：等宽 */
--font-code: 'JetBrains Mono', 'Fira Code', 'Menlo', monospace;
```

- 通过 Google Fonts 加载 Poppins、Lora、JetBrains Mono；中文使用系统字体回退。
- 如果网络字体加载失败，页面仍必须可读，不得出现布局错乱。
- 标题可以大胆，正文必须优先保证投影场景下的可读性。

**排版层级：**

| 层级 | 字号 | 字重 | 颜色 |
|------|------|------|------|
| 封面标题 | 3.2em | 700 (Poppins Bold) | `#d97757`（橙色） |
| 章节标题 | 2.0em | 600 (Poppins SemiBold) | 当章强调色 |
| 页面标题 | 1.6em | 600 | `#faf9f5` |
| 正文 | 0.9em | 400 (Lora Regular) | `#faf9f5` |
| 说明文字 | 0.7em | 400 | `#b0aea5` |
| 代码 | 0.75em | 400 (JetBrains Mono) | `#faf9f5` |
| 代码注释 | 0.75em | 400 | `#788c5d`（绿色） |

**间距系统：**
- 页面内边距：60px（四边）
- 标题与正文间距：30px
- 正文段落间距：20px
- 代码块内边距：24px，圆角 8px
- 代码块上方间距：24px

#### 视觉元素规范

**代码块：**
- 背景：`#1a1a18`（比主背景稍亮）
- 边框：左侧 3px 实线，颜色为当前章节强调色
- 圆角：8px
- 每页代码不超过 15 行
- 关键行用强调色背景高亮（`rgba(accent, 0.15)`）
- 行号用绿色 `#788c5d` 显示

**架构图（Mermaid）：**
- 使用 Mermaid 深色主题
- 节点填充：`#1a1a18`
- 节点边框：当前章节强调色
- 连线：`#b0aea5`
- 文字：`#faf9f5`
- 关键节点用强调色填充

**表格：**
- 表头背景：`#1a1a18`，文字为强调色
- 偶数行背景：`rgba(250, 249, 245, 0.03)`
- 边框：`#e8e6dc`（低透明度）

**进度指示器：**
- 底部进度条使用当前章节强调色
- 右下角显示「Part X / 7」和页码

**章节分隔页：**
- 全屏大标题，字号 3em
- 背景使用主背景色 + 当前章节强调色的低透明度色块或边框，不使用渐变背景
- 底部有一条细线 + 章节编号

**Fragment 动画：**
- 使用 reveal.js 的 fade-in 效果
- 复杂内容逐步展示（如架构图先显示核心模块，再显示周边模块）
- 代码块的关键行用 fragment 高亮

#### 反模式（避免）

- 不要使用花哨渐变背景；章节页也不要用大面积渐变
- 不要使用超过 3 种强调色在同一页
- 不要使用投影/发光效果（flat design）
- 不要使用 emoji 或装饰性图标
- 不要使用默认的 reveal.js 主题样式
- 不要让代码块撑满整个页面（留白很重要）

### 内容密度与可读性

- 每张 slide 只讲一个主观点；标题必须能独立表达观点，而不是泛泛的章节名。
- 每页正文最多 4 个 bullet；每个 bullet 尽量不超过 18 个中文字符。
- 每页代码不超过 15 行；复杂代码拆成“概念页 + 关键片段页 + speaker notes 解释”。
- 架构图节点控制在 6-10 个；超过 10 个节点时拆成多页或用 fragment 分步展示。
- 每个核心概念按“白话解释 → 为什么需要 → 源码实现 → 设计取舍”展开。
- 互动页最多 2 张，不要打断主线节奏。

### 交互

- 左右键翻页
- 按 ESC 进入概览模式
- 按 S 打开演讲者备注

## 输出

生成一个完整的单文件 HTML，文件名为 `slides.html`，放在项目根目录下。

- CSS 和自定义 JS 内联在 HTML 中。
- reveal.js、highlight.js、Mermaid、Google Fonts 通过 CDN 加载。
- 不要生成额外资源文件。
- 不要出现 TODO、placeholder、lorem ipsum 或未替换的示例路径。

## 验收标准

- `slides.html` 通过本地 HTTP server 打开后，正文、代码高亮、Mermaid 图、speaker notes、概览模式都可用。
- Markdown/HTML 结构完整，没有未闭合代码块、未闭合标签或无效 Mermaid 语法。
- 所有代码页都标注真实源码来源；随机抽查任意 5 个代码片段都能在 vendor 源码中找到对应实现。
- 1366x768 和 1920x1080 视口下不出现横向滚动，主要内容不被页脚、进度条或 speaker notes 标记遮挡。
- 主讲页控制在 40-46 张；Appendix 不计入主讲页码。
- 至少包含 6 张 Mermaid 架构/流程图：Agent Loop、Tool Dispatch、Context Guard、Prompt Assembly、Retry Onion、Multi-Agent/Gateway Routing。
- 至少包含 10 张真实 Java 代码页，覆盖两个项目，且不要集中在同一个 session。
- 每个 Part 末尾有一句“本节 takeaway”，帮助听众建立知识框架。
