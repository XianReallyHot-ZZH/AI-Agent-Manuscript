# Slide Generation Prompt

## Task

基于两个 AI Agent 教学项目（AI-Agent-ZeroToOne、OpenClaw-ZeroToOne），生成一份用于团队技术分享的 reveal.js HTML 演示文稿。

## 受众

程序员团队，部分人熟悉 Agent 概念，部分人不熟悉。需要在通俗易懂和硬核深度之间平衡。

## 时长

约 1 小时（约 50-60 张 slides，含代码页和架构图页）。

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

### 第一部分：开场与破冰（~5 张）

1. 封面：标题党主标题 + 副标题
2. 你每天用的 Claude Code，核心只有 5 行代码
3. 什么是 AI Agent？（大白话解释：while 循环 + 工具调用）
4. 两个项目全景图（AI-Agent-ZeroToOne vs OpenClaw-ZeroToOne 的定位差异）
5. 今天的路线图（agenda）

### 第二部分：核心原理——Agent 的本质（~10 张）

6. "The Model IS the Agent"——为什么说模型就是 Agent？
7. Agent = Loop + Tools（核心循环伪代码）
8. 实际代码：S01 Agent Loop 的 Java 实现（5 行核心 + 完整上下文）
9. Tool Dispatch 模式：双表设计（Schema 表 + Handler 表）
10. 实际代码：S02 的工具定义和分发
11. 对比两个项目的 S01/S02 实现差异
12. 从 5 行代码到完整 Agent——全景演进路线图
13. 设计哲学：Harness Engineering——你写的不是智能，是智能的容器
14. Context is Memory——为什么上下文管理是 Agent 的核心挑战
15. 互动：你能想到哪些 Agent 失败的场景？（引出后续 Session 要解决的问题）

### 第三部分：从 Loop 到智能体（~10 张）

16. Phase 2 开场：Agent 需要什么才能变得「靠谱」？
17. 规划能力：TodoWrite + Nag 机制（代码展示）
18. 上下文隔离：Subagent 为什么重要？进程隔离 = 上下文隔离
19. 两层知识注入：Skill Loading 的 metadata + body 模式
20. 三级压缩：micro → auto → manual（附流程图）
21. OpenClaw 对应：Session 持久化 + Context Guard
22. 实际代码：三级上下文溢出恢复策略
23. 多平台接入：Channel 抽象（CLI / Telegram / 飞书）
24. 设计模式对比：两个项目如何解决同一个问题（知识注入 vs 系统提示词组装）
25. 小结：从 Loop 到智能体的关键升级

### 第四部分：生产级 Agent（~12 张）

26. Phase 3 开场：让 Agent 能「活下去」
27. 权限系统：deny/mode/allow/ask 管线
28. Hook 系统：PreToolUse / PostToolUse 生命周期
29. 跨会话记忆：持久化 + DreamConsolidator
30. 系统提示词组装：6 层 vs 8 层（两个项目对比）
31. OpenClaw：八层提示词架构图（Mermaid）
32. OpenClaw：混合记忆检索（TF-IDF + 哈希向量 + MMR 重排序）
33. 错误恢复：三种策略 + 指数退避
34. OpenClaw：三层重试洋葱（架构图）
35. 消息投递安全：WAL 写入（tmp → fsync → atomic rename）
36. 任务系统：文件持久化 DAG + 后台虚拟线程 + Cron 调度
37. 小结：生产级 Agent = 可靠 + 可恢复 + 可观测

### 第五部分：多 Agent 协作（~8 张）

38. Phase 4 开场：从单兵到团队
39. JSONL 邮箱模型：Agent 间通信（代码展示）
40. 握手协议：shutdown / plan approval FSM
41. 自治 Agent：idle → poll → claim → work 生命周期
42. Worktree 隔离：Git worktree 实现目录级沙箱
43. MCP 插件：JSON-RPC 2.0 外部工具集成
44. OpenClaw：Gateway 五级路由 + WebSocket 多 Agent 管理
45. 命名车道队列：main / cron / heartbeat 的并发控制
46. 小结：多 Agent 架构的关键设计决策

### 第六部分：教学版 vs 生产版（~5 张）

47. light-claw-4j vs enterprise-claw-4j 架构对比
48. 教学版：每课一文件，零框架，纯 Java 21
49. 生产版：Spring Boot 3.5，87+ 类，Docker/K8s
50. 从教学到生产：哪些设计决策改变了？为什么？
51. 同一个概念在两个版本中的代码对比（如 Agent Loop）

### 第七部分：总结与展望（~5 张）

52. 全景回顾：29 个 Session 的知识图谱
53. 核心设计模式 Top 12 一览
54. 两个项目的互补关系：Agent 构建 vs Agent Gateway 构建
55. 企业级 Java + AI 架构蓝图概览
56. Q&A + 资源链接

## 技术要求

### reveal.js 配置

- 使用 reveal.js 4.x CDN
- 主题：使用 `dracula` 或 `night` 深色主题（适合代码展示）
- 代码高亮：highlight.js，Java 语法高亮
- 支持演讲者备注（speaker notes）
- 支持概览模式（Overview Mode）
- 字体：代码用等宽字体，中文用系统字体

### 视觉设计

- 深色背景配亮色代码（护眼，适合长时间技术分享）
- 关键数字和关键词用亮色高亮
- 架构图用 Mermaid 渲染（reveal.js 的 mermaid 插件或静态 SVG）
- 代码块使用等宽字体，控制每页不超过 15 行代码
- 适当使用 fragment 动画逐步展示复杂内容
- 每个部分用不同颜色标识，方便听众感知进度

### 交互

- 左右键翻页
- 按 ESC 进入概览模式
- 按 S 打开演讲者备注

## 输出

生成一个完整的单文件 HTML（内联所有 CSS/JS，通过 CDN 加载 reveal.js 和 highlight.js），文件名为 `slides.html`，放在项目根目录下。
