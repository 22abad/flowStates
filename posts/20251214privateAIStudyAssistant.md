---
title: "My Journey: Building a Private AI Study Assistant for My University Courses"
date: 2025-12-14
tags:
  - AI
  - RAG
  - Ollama
  - VS Code
  - AnythingLLM
---

# My Journey: Building a Private AI Study Assistant for My University Courses

![Private AI Study Assistant|600](https://assets.flowstates.me/2025/20251214privateAIStudyAssistant.jpg)

<!-- Image Prompt: A cinematic, high-tech illustration of a disheveled computer science student in a dimly lit university computer lab late at night. The student is intensely focused on a laptop screen displaying complex code and AI model visualizations. In the background, server racks with glowing blue and green LEDs hum with activity, symbolizing a private local AI server. The atmosphere is a mix of exhaustion and determination, with a cyberpunk aesthetic. Holographic projections of neural networks and exam notes float subtly around the workspace. 8k resolution, detailed, digital art style. -->

It all started with a simple question about some strange backup files, but it quickly spiraled into a deep dive into the world of local AI, knowledge bases, and developer workflows. This is the story of how I went from being confused by my school's database to building a powerful, private AI assistant that runs entirely on my own machine, helping me prepare for my exams.

### Chapter 1: The Initial Spark - A Need for Smarter Study Tools

Like many students, I was faced with preparing for my university exams. The primary resource was our school's PostgreSQL database, accessible through a clunky `phpPgAdmin` interface. My initial thought was simple: could I export the database structure and have an AI like Gemini generate practice questions for me? This simple idea was the starting point of a much bigger project.

### Chapter 2: Going Local - The Ollama & VS Code Dream

As a developer with a GitHub Student Pack, I wanted more control and privacy. The idea of a cloud-based solution felt limiting. This led me to **Ollama**, a fantastic tool for running large language models (LLMs) like Llama 3 and Qwen locally.

The plan was to create a knowledge base (KB) directly inside my favorite editor, **VS Code**. The key piece of the puzzle was the **Continue** plugin, an open-source AI code agent that promised to connect my editor to my local Ollama instance.

My initial setup involved two models:

- **Chat Model (`qwen2.5-coder:7b`)**: The "brain" that would answer my questions.
- **Embedding Model (`nomic-embed-text`)**: The "eyes" that would read my documents and turn them into a format the AI could search.

### Chapter 3: The Configuration Nightmare - A Series of Unfortunate Events

This is where the real challenge began. What seemed like a straightforward setup turned into a multi-step troubleshooting saga:

1. **Deprecated Indexing**: The first hurdle was a warning in Continue: "Indexing has been deprecated." I learned the workflow had changed from a slow, full-project index to a more efficient, on-demand context using `@` commands like `@file`.
2. **Configuration Hell**: The plugin refused to load my local models. I battled with `config.yaml` vs. `config.json` file formats, invisible syntax errors, and the frustrating realization that the plugin wasn't reading my configuration at all.
3. **The "Reload Window" Trick**: After trying everything, the classic developer solution of "turning it off and on again" (Reload Window in VS Code) and completely resetting the plugin's configuration folder finally worked. My local models appeared!

### Chapter 4: The Uncooperative AI - When the Engine Refuses to Start

Success was short-lived. I fed my course documents to the AI, but it stubbornly refused to answer based on them. It kept giving generic, unhelpful replies, claiming it couldn't access my files, even though the RAG (Retrieval-Augmented Generation) system was clearly finding the right documents (as shown by the "citations").

The problem wasn't the setup; it was the model itself. Qwen-Coder, being highly specialized for writing code, was too cautious and lacked the robust instruction-following ability needed for a RAG task. Its built-in safety guardrails prevented it from answering questions it deemed related to "specific exam content."

### Chapter 5: The Breakthrough - The Right Tool for the Job

The solution was simple but profound: **change the brain**. I switched from the specialized `qwen2.5-coder` to the general-purpose, powerful **`llama3.1`**.

The difference was night and day.

I asked it to summarize a course PDF, and it responded perfectly: "According to your request, I have examined the document 2023-CS210-Autumn.pdf. The document is a final exam paper for the computer science course CS210..."

It worked. The entire local RAG pipeline was finally functional.

### Chapter 6: Leveling Up - A Persistent, Always-On Knowledge Base with AnythingLLM

While the VS Code setup was powerful for coding, I wanted a more robust, "always-on" knowledge base that could automatically monitor my files, just like dedicated apps like Cherry. This led me to **AnythingLLM**.

I set up a full-stack private RAG system:

- **LLM**: Ollama (`llama3.1`)
- **Embedding**: AnythingLLM's built-in embedder
- **Vector Database**: LanceDB

The system came to life, my laptop's fans spinning up for the first time as it processed my PDFs and PPTs. This was the ultimate private study assistant.

### Conclusion: From Confusion to Control

This journey taught me invaluable lessons. The world of local AI is incredibly powerful but requires perseverance. The key takeaways were:

- **Choose the Right Model**: The LLM's capabilities (e.g., coding vs. instruction-following) are as important as the RAG setup itself.
- **Embrace the Workflow**: Integrating AI directly into the editor (Continue in VS Code) is a game-changer for developer productivity, eliminating context switching.
- **Privacy is Power**: Running a powerful RAG system entirely on a personal machine is not just possible; it's empowering.

What started as a simple need to study for an exam became a deep exploration of building a personalized AI. Now, I have a study buddy that's private, powerful, and perfectly tailored to my needs.

---

# 我的探索之旅：为我的大学课程构建一个私有 AI 学习助手

![Private AI Study Assistant|600](https://assets.flowstates.me/2025/20251214privateAIStudyAssistant.jpg)

这一切都始于一个关于奇怪备份文件的简单问题，但很快就演变成了一场深入探索本地 AI、知识库和开发者工作流的旅程。这是一个关于我如何从对我学校的数据库感到困惑，到最终构建一个完全运行在自己机器上的、强大的私有 AI 助手来帮助我备考的故事。

### 第一章：最初的火花 —— 对更智能学习工具的需求

和许多学生一样，我面临着准备大学考试的挑战。主要资源是我们学校的 PostgreSQL 数据库，通过一个笨拙的 phpPgAdmin 界面访问。我最初的想法很简单：我能否导出数据库结构，然后让像 Gemini 这样的 AI 为我生成练习题？这个简单的想法，成了一个宏大得多的项目的起点。

### 第二章：走向本地 —— Ollama 与 VS Code 之梦

作为一名拥有 GitHub 学生包的开发者，我渴望更多的控制权和隐私。基于云的解决方案让我觉得受限。这引导我走向了 **Ollama**，一个可以在本地运行像 Llama 3 和 Qwen 这样的大语言模型（LLM）的神奇工具。

计划是直接在我最喜欢的编辑器 **VS Code** 内部创建一个知识库（KB）。解开谜题的关键是 **Continue** 插件，这是一个开源的 AI 代码代理，它承诺能将我的编辑器连接到我的本地 Ollama 实例。

我的初始设置包含两个模型：

- **聊天模型（`qwen2.5-coder:7b`）**：负责回答我问题的“大脑”。
- **嵌入模型（`nomic-embed-text`）**：负责读取我的文档，并将其转换为 AI 可以搜索的格式的“眼睛”。

### 第三章：配置噩梦 —— 一系列不幸的事件

真正的挑战从这里开始。看似简单的设置变成了一场多步骤的排错传奇：

1. **被废弃的索引功能**：第一个障碍是 Continue 中的一个警告：“索引功能已被废弃”。我了解到其工作流已经从缓慢的全项目索引，转变为使用 `@` 命令（如 `@file`）的、更高效的按需上下文模式。
2. **配置地狱**：插件拒绝加载我的本地模型。我在 `config.yaml` 和 `config.json` 文件格式之间挣扎，与看不见的语法错误作斗争，并沮丧地发现插件根本没有读取我的配置。
3. **“重载窗口”大法**：在尝试了所有方法后，经典的开发者解决方案“关掉再打开”（在 VS Code 中是 Reload Window）以及彻底重置插件的配置文件夹，终于奏效了。我的本地模型出现了！

### 第四章：不合作的 AI —— 当引擎拒绝启动

成功是短暂的。我把课程文档喂给 AI，但它固执地拒绝基于这些文档回答。它不停地给出通用、无用的回复，声称无法访问我的文件，尽管 RAG（检索增强生成）系统明明已经找到了正确的文档（“引用”部分显示了这一点）。

问题不在于设置，而在于模型本身。Qwen-Coder 是一个为编写代码而高度特化的模型，它过于谨慎，缺乏 RAG 任务所需的强大的指令跟随能力。其内置的安全护栏阻止了它回答任何它认为与“具体考试内容”相关的问题。

### 第五章：突破 —— 为正确的工作选择正确的工具

解决方案简单而深刻：**更换大脑**。我从特化的 `qwen2.5-coder` 切换到了通用的、强大的 **`llama3.1`**。

效果天差地别。

我让它总结一份课程 PDF，它完美地回应了：“根据你的请求，我检查了 2023-CS210-Autumn.pdf 这份文档。文档是一份计算机科学课程 CS210 的期末考试试卷……”

它成功了。整个本地 RAG 流程终于完全打通。

### 第六章：再度升级 —— 使用 AnythingLLM 打造持久化、全天候的知识库

虽然 VS Code 的集成对编码很强大，但我想要一个更健壮的、“全天候在线”的知识库，能像 Cherry 这样的专用应用一样自动监控我的文件。这让我找到了 **AnythingLLM**。

我搭建了一套全栈私有 RAG 系统：

- **大语言模型（LLM）**：Ollama（`llama3.1`）
- **嵌入（Embedding）**：AnythingLLM 的内置嵌入器
- **向量数据库（Vector Database）**：LanceDB

当我的笔记本电脑风扇第一次呼啸着转起来，处理我的 PDF 和 PPT 文件时，这个系统真正地活了过来。这，就是终极的私人学习助手。

### 结论：从困惑到掌控

这段旅程教会了我宝贵的经验。本地 AI 的世界无比强大，但也需要坚持不懈。关键的收获是：

- **选择正确的模型**：LLM 自身的能力（例如，编码 vs. 指令跟随）与 RAG 的设置本身同等重要。
- **拥抱工作流**：将 AI 直接集成到编辑器中（VS Code 中的 Continue）是开发者生产力的颠覆性提升，它消除了上下文切换的干扰。
- **隐私就是力量**：在个人电脑上完整运行一个强大的 RAG 系统不仅是可能的，更是赋予自己能力的体现。

最初只是一个简单的备考需求，最终演变成了一场对构建个性化 AI 的深度探索。现在，我拥有了一个私密、强大且完美满足我需求的学习伙伴。
