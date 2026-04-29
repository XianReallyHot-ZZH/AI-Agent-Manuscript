# chat

## 添加 子模块

git submodule add https://github.com/XianReallyHot-ZZH/AI-Agent-ZeroToOne vendors/AI-Agent-ZeroToOne
git submodule add https://github.com/XianReallyHot-ZZH/OpenClaw-ZeroToOne vendors/OpenClaw-ZeroToOne

## 重构

重构 slides.html，重构要求：
（1）内容编排上以一个项目一个项目的来，先介绍 AI-Agent-ZeroToOne，然后介绍 OpenClaw-ZeroToOne，现在的内容知识点过于融合，对初学者不够友好
    - AI-Agent-ZeroToOne：逐个讲解 session，包括知识点，最小心智模型，架构原理，核心代码
    - OpenClaw-ZeroToOne：先讲教学项目 light-claw-4j 逐个讲解 session，包括知识点，最小心智模型，架构原理，核心代码
    - OpenClaw-ZeroToOne：后讲企业级改造项目 enterprise-claw-4j，讲解项目结构，模块功能，整体架构
（2）slide 设计风格上还是以 claude design 设计理念为准，slide 的底色不要选黑色，选稍亮一点点的 claude 专属黄色。
（3）各篇章标题要吸引人
（4）篇章内 slides 顺序合理，后一个 slide 要像是从前一个长出来一样，逻辑路径自燃。
旧的 slides.html 就留在那里，重构新起一个 html 文件。
