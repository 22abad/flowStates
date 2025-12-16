---
title: A Tricky Bug: Mermaid Diagrams & display:none
title_en: A Tricky Bug: Mermaid Diagrams & display:none
title_zh: 一个棘手的 Bug：Mermaid 图表与 display:none
date: 2025-12-12
categories: 
  - "FrontEnd"
  - "DevLog"
tags: 
  - "FrontEnd"
  - "DevLog"
summary_en: Sharing a bug I encountered where Mermaid diagrams disappeared during language toggling. I learned about how `display:none` affects rendering and how to manage state when libraries modify the DOM.
summary_zh: 分享我在双语切换中遇到的 Mermaid 图表消失问题。这次经历让我加深了对 `display:none` 渲染机制的理解，以及当第三方库修改 DOM 时如何管理状态。
---

[EN]

# 🕵️ A Tricky Bug: Mermaid Diagrams & display:none

![The Ripple Effect|600](https://assets.flowstates.me/2025/20251212MermaidDebug.jpg)
While building **HDIPERS.LOG**, I ran into a bug that taught me a lot about how browsers render pages. The site supports bilingual switching (English/Chinese), which worked fine until I added **Mermaid.js** flowcharts.

### 🛑 The Issue

1.  Load page (English): Diagrams look fine.
2.  Click **"Switch to 中文"**: The Chinese text appears, but the **diagrams are gone**.
3.  Switch back to English: The original diagrams are **also broken**.
4.  Refresh page: Everything works again (until I switch).

### 🔍 Understanding the Cause

After some digging, I realized this was related to how the browser handles hidden elements.

#### 1. The `display: none` Issue

When toggling languages, I use a CSS class that applies `display: none` to the inactive language.

- **What happens**: An element with `display: none` is removed from the layout flow. It effectively has a size of `0px`.
- **The Result**: When Mermaid tries to render the Chinese diagrams (which are initially hidden), it sees a container with 0 width and height, so it draws nothing.

#### 2. Destructive Rendering

Mermaid works by replacing the text description of a graph with an SVG image.

- **Before Render**: `<div class="mermaid">graph TD; A-->B;</div>` (Source Code)
- **After Render**: `<div class="mermaid"><svg>...</svg></div>` (SVG Image)

Once rendered, the **source code is replaced**. When I switched languages back and forth, Mermaid was trying to re-render an SVG instead of the original code, which caused it to fail.

### 🛠️ How I Fixed It

To solve this, I needed a way to "reset" the diagrams each time the language changed.

#### Step 1: Backup the Original Code

I modified the Markdown parser (`marked.js`) to save the original Mermaid code into a custom attribute `data-original-code` before it gets rendered. This acts as a backup.

```javascript
// marked.js renderer configuration
renderer.code = function (code, language) {
  if (language === "mermaid") {
    // Safe encode the code and store it as a backup
    const safeCode = code.replace(/"/g, "&quot;");
    return `<div class="mermaid" data-original-code="${safeCode}">${code}</div>`;
  }
  return `<pre>...</pre>`;
};
```

#### Step 2: The Reset Function

I wrote a function `rerenderMermaid` that runs every time the language is toggled. It basically restores the original code from the backup and asks Mermaid to render it again.

```javascript
window.rerenderMermaid = async () => {
  document.querySelectorAll(".mermaid").forEach((node) => {
    // Restore from backup
    const originalCode = node.getAttribute("data-original-code");
    if (originalCode) {
      node.removeAttribute("data-processed");
      node.innerHTML = originalCode;
    }
  });
  // Re-run rendering
  await mermaid.run({ querySelector: ".mermaid", suppressErrors: true });
};
```

---

### 🚀 What I Learned

Fixing this bug was a great learning experience. It reminded me of a few key concepts:

#### 1. Browser Rendering

It's important to remember that `display: none` removes elements from the render tree, meaning they have no dimensions. This can confuse libraries that need to calculate size, like charting tools.

#### 2. State Management

Since Mermaid modifies the DOM directly, I couldn't rely on the DOM to hold the "source of truth." Keeping a backup of the original code (`data-original-code`) was essential to be able to re-render it later.

#### 3. Asynchronous Timing

Sometimes, elements aren't visible immediately after removing a class. Using `setTimeout` can help ensure the browser has finished updating the layout before we try to draw the diagrams.

> **Takeaway**: Debugging is often less about fixing the code and more about understanding how the underlying system works.

[END]

[ZH]

# 🕵️ 一个棘手的 Bug：Mermaid 图表与 display:none

![The Ripple Effect|600](https://assets.flowstates.me/2025/20251212MermaidDebug.jpg)
在构建 **HDIPERS.LOG** 的过程中，我遇到了一个 Bug，它让我对浏览器的渲染机制有了更深的理解。网站支持中英双语切换，一切看起来都很完美，直到我引入了 **Mermaid.js** 流程图。

### 🛑 问题现象

1.  加载英文版页面：图表显示正常。
2.  点击 **"Switch to 中文"**：中文文本出现了，但 **图表消失了**。
3.  切回英文版：原本正常的英文图表也 **坏掉了**。
4.  刷新页面：一切恢复正常（直到再次切换）。

### 🔍 原因分析

经过一番排查，我发现这与浏览器处理隐藏元素的方式有关。

#### 1. `display: none` 的问题

我们在切换语言时，使用了 CSS 类来隐藏非当前语言的内容（本质是 `display: none`）。

- **发生了什么**: 当一个元素被设为 `display: none` 时，它就不占据布局空间了。它的宽高变成了 `0px`。
- **结果**: 当 Mermaid 尝试渲染中文图表时（初始状态为隐藏），它获取到的容器尺寸是 0，所以它什么也画不出来。

#### 2. 破坏性渲染

Mermaid 的工作原理是将图表的文本描述替换为 SVG 图片。

- **渲染前**: `<div class="mermaid">graph TD; A-->B;</div>` (源代码)
- **渲染后**: `<div class="mermaid"><svg>...</svg></div>` (SVG 图片)

一旦渲染完成，**源代码就被替换了**。当我们来回切换语言时，Mermaid 面对的是 SVG 标签而不是最初的代码，这就导致了二次渲染失败。

### 🛠️ 我是如何解决的

为了解决这个问题，我需要在每次切换语言时“重置”图表。

#### 第一步：备份原始代码

我修改了 Markdown 解析器 (`marked.js`)，在图表被渲染前，将原始代码备份到一个自定义属性 `data-original-code` 中。

```javascript
// marked.js renderer 配置
renderer.code = function (code, language) {
  if (language === "mermaid") {
    // 安全转义并备份源代码
    const safeCode = code.replace(/"/g, "&quot;");
    return `<div class="mermaid" data-original-code="${safeCode}">${code}</div>`;
  }
  return `<pre>...</pre>`;
};
```

#### 第二步：重置函数

我写了一个 `rerenderMermaid` 函数，每次切换语言时都会触发。它的作用是从备份中还原原始代码，并让 Mermaid 重新渲染。

```javascript
window.rerenderMermaid = async () => {
  document.querySelectorAll(".mermaid").forEach((node) => {
    // 从备份还原
    const originalCode = node.getAttribute("data-original-code");
    if (originalCode) {
      node.removeAttribute("data-processed");
      node.innerHTML = originalCode;
    }
  });
  // 重新渲染
  await mermaid.run({ querySelector: ".mermaid", suppressErrors: true });
};
```

---

### 🚀 我的收获

解决这个 Bug 是一个很好的学习过程。它让我重温了几个关键概念：

#### 1. 浏览器渲染

记住 `display: none` 会把元素从渲染树移除是很重要的，这意味着它们没有尺寸。这可能会让像图表库这样需要计算尺寸的工具感到困惑。

#### 2. 状态管理

由于 Mermaid 直接修改 DOM，我不能依赖 DOM 来保存“唯一真相”。保留原始代码的备份 (`data-original-code`) 对于稍后的重新渲染至关重要。

#### 3. 异步时机

有时，移除类后元素并不会立即变得可见。使用 `setTimeout` 可以帮助确保在尝试绘制图表之前，浏览器已经完成了布局更新。

> **心得**: Debug 往往不仅仅是修代码，更是理解底层系统是如何工作的。

[END]
