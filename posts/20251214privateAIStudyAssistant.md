---
title: "Constructing a Local RAG System for Academic Review"
title_zh: "构建本地 RAG 系统辅助学术复习"
date: 2025-12-14
author: "Dong Li"
categories:
  - "Engineering"
  - "AI Implementation"
  - "Study Tools"
tags:
  - "RAG"
  - "Ollama"
  - "Local LLM"
  - "System Design"
summary_en: "A technical log of setting up a local RAG (Retrieval-Augmented Generation) system using Ollama, VS Code, and AnythingLLM to assist with university exam preparation. Discussing the trade-offs between privacy, model capability, and system complexity."
summary_zh: "一篇关于使用 Ollama、VS Code 和 AnythingLLM 搭建本地 RAG（检索增强生成）系统的技术日志，旨在辅助大学期末复习。探讨了隐私、模型能力和系统复杂性之间的权衡。"
---

[EN]

# Constructing a Local RAG System for Academic Review

![Private AI Study Assistant|600](https://assets.flowstates.me/2025/20251214privateAIStudyAssistant.jpg)

> **Objective:** To establish a secure, local AI environment for reviewing course materials, leveraging RAG technology to interact with private documents.

### 1. The Motivation

Facing the pressure of university exams, I sought a more efficient method for review. The primary resource was a PostgreSQL database, accessible only through a legacy `phpPgAdmin` interface.

My initial hypothesis was straightforward: **Could I export the database structure and utilize an AI to generate targeted practice questions?** This question led to the implementation of a local RAG system.

### 2. Choosing Local Tools: Ollama & VS Code

I prefer to keep my data local for privacy reasons, rather than relying on cloud-based solutions.

This led me to discover **Ollama**, a tool that makes running Large Language Models (LLMs) locally easy. My goal was to build a private Knowledge Base inside VS Code.

The key to unlocking this puzzle was **Continue**, an open-source AI code agent plugin. It promised to seamlessly connect my editor to my local Ollama instance, enabling "coding as conversation."

My initial tech stack was:

- **Inference Brain (Chat Model)**: `qwen2.5-coder:7b`, responsible for understanding and answering my questions.
- **Data Eyes (Embedding Model)**: `nomic-embed-text`, responsible for transforming my documents into vector data the AI could understand.

### 3. Configuration Challenges

Setting up the environment required some troubleshooting:

1. **Deprecated "Full Indexing"**: The first warning Continue threw at me was "Indexing has been deprecated." I was forced to update my mental model: the workflow had evolved from a slow, clunky full-project index to a more flexible, efficient **On-Demand Context** mode, using commands like `@file` to feed data precisely.
2. **The Maze of Config Files**: The plugin stubbornly refused to load my local models. I fell into a chaotic war between `config.yaml` and `config.json` formats, fighting invisible syntax errors. Most frustratingly, the plugin seemed to be completely ignoring my configuration files.
3. **Restarting VS Code**: After trying various debugging methods, simply reloading the VS Code window and resetting the configuration folder solved the issue.

### 4. Model Selection Issues

The joy of success was short-lived. When I eagerly fed my course documents to the AI, it stubbornly refused to answer based on them. It acted like a customer service bot speaking in platitudes, constantly repeating generic "cannot access files" responses, even though the RAG (Retrieval-Augmented Generation) system logs and "citations" clearly showed it had accurately found the relevant documents.

The problem wasn't the RAG pipeline; it was the model itself. **Qwen-Coder** is a model highly specialized for code generation. Its safety guardrails were too conservative, and it lacked the **strong Instruction Following capability** essential for RAG tasks. It incorrectly flagged my exam review requests as sensitive content and refused to answer.

### 5. Switching to Llama 3.1

The solution, in hindsight, was simple yet profound: **If the brain doesn't fit, swap it.**

I decisively abandoned the specialized `qwen2.5-coder` and switched to the more general-purpose, ecosystem-mature **`llama3.1`**.

The difference was significant.

When I asked it to summarize a course PDF again, it responded perfectly: "According to your request, I have examined the document 2023-CS210-Autumn.pdf. The document is a final exam paper for the computer science course CS210..."

This confirmed that the local RAG setup was working correctly.

### 6. Setting up AnythingLLM

While the VS Code + Continue combination excelled in coding scenarios, I craved a more robust, persistent, **"always-on" knowledge base** that could automatically monitor file changes, just like Cherry.

This led me to **AnythingLLM**. I built a complete full-stack private RAG system based on it:

- **LLM Inference Core**: Ollama (`llama3.1`)
- **Vector Engine (Embedding)**: AnythingLLM's built-in efficient embedder
- **Vector Database**: LanceDB (High-performance local vector storage)

The system successfully batch processed my PDF and PPT files, creating a comprehensive local knowledge base for my studies.

### Conclusion

This journey didn't just help me prepare for exams; it taught me a valuable lesson about AI. The world of local AI has infinite potential, but it requires the spirit of exploration and tinkering.

My core methodology summary:

- **Model Capability Determines the Ceiling**: The foundational capabilities of the LLM (e.g., instruction following vs. code generation) are just as important as the RAG architecture itself. Choose the right model, and half the work is done.
- **Workflow**: Integrating AI capabilities directly into the editor (like Continue in VS Code) improves productivity by reducing context switching.
- **Privacy**: Running a RAG system locally ensures that personal data and knowledge remain private.

I spent two days building this tool instead of studying, but it was a productive diversion. Now I have a useful assistant to help me with my exam preparation.

[END]

[ZH]

# 构建本地 RAG 系统辅助学术复习

![Private AI Study Assistant|600](https://assets.flowstates.me/2025/20251214privateAIStudyAssistant.jpg)

> **目标：** 建立一个安全、本地化的 AI 环境，利用 RAG 技术与私有文档交互，辅助复习课程资料。

### 1. 动机：寻找更高效的复习工具

面对大学考试的压力，我寻求一种更高效的复习方法。主要资源是一个只能通过过时的 phpPgAdmin 界面访问的 PostgreSQL 数据库。

我最初的假设很简单：**我能否导出数据库结构，并利用 AI 生成针对性的练习题？** 这个问题促使我实施了一个本地 RAG 系统。

### 2. 选择本地方案：Ollama 与 VS Code

出于对隐私的考虑，我不希望将数据上传到云端。

这让我发现了 **Ollama**，它可以轻松在本地运行大语言模型。我的目标是在 VS Code 内部搭建一个私有知识库。

解决方案的核心是 **Continue**，一个开源的 AI 代码代理插件。它能将编辑器连接到本地的 Ollama 实例。

我的初始技术栈如下：

- **推理模型（Chat Model）**: `qwen2.5-coder:7b`，负责理解和回答问题。
- **嵌入模型（Embedding Model）**: `nomic-embed-text`，负责将文档转化为向量数据。

### 3. 配置过程中的挑战

配置过程中遇到了一些技术障碍：

1.  **索引机制变更**：Continue 提示“索引功能已被废弃”。工作流已从全项目索引进化为更灵活的**按需上下文（On-Demand Context）**模式，例如使用 `@file` 命令精准投喂数据。
2.  **配置文件问题**：插件最初无法加载本地模型。经过排查，发现是 `config.yaml` 与 `config.json` 格式混淆以及语法错误导致的。
3.  **解决方案**：重启 VS Code 窗口并重置配置文件夹最终解决了问题。

### 4. 模型选择与调试

初步配置完成后，遇到了一些问题。当我们将课程文档提供给 AI 时，它拒绝基于这些材料进行回答，尽管 RAG 系统显示已检索到相关文档。

问题的症结在于模型本身。**Qwen-Coder** 是一个为代码生成特化的模型，其安全策略较为保守，且在 RAG 任务所需的**指令遵循（Instruction Following）能力**上有所欠缺。

### 5. 更换模型

解决方案是更换更适合的模型。

我切换到了通用能力更强、生态更成熟的 **`llama3.1`**。

效果有了明显改善。当我再次要求它总结一份课程 PDF 时，它能够准确地根据文档内容进行回应。这证明系统已经可以正常工作。

### 6. 使用 AnythingLLM 搭建持久化知识库

虽然 VS Code + Continue 在编码场景下表现良好，但我需要一个更健壮、能自动监控文件变化的知识库系统。

因此我引入了 **AnythingLLM**，搭建了一套完整的全栈私有 RAG 系统：

- **LLM 推理核心**: Ollama (`llama3.1`)
- **向量化引擎（Embedding）**: AnythingLLM 内置的高效嵌入器
- **向量数据库（Vector Database）**: LanceDB（高性能的本地向量存储）

系统成功批量处理了数百份 PDF 和 PPT 文件，构建了本地知识库。

### 总结

这段经历不仅解决了备考需求，也加深了我对 AI 系统架构的理解。

核心经验总结：

- **模型选择至关重要**：LLM 的基础能力（如指令遵循 vs. 代码生成）直接影响 RAG 的效果。
- **工作流集成**：将 AI 能力集成到编辑器（如 VS Code 中的 Continue）可以减少上下文切换，提高效率。
- **数据隐私**：在个人设备上运行 RAG 系统可以确保数据安全。

最初关于数据库的一个简单疑问，最终演变成了一次构建个性化 AI 系统的实践。现在，我拥有了一个安全、高效的复习助手。

[END]
