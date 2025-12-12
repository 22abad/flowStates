---
title: The Case of the Vanishing Diagrams: Debugging Mermaid & display:none
title_en: The Case of the Vanishing Diagrams: Debugging Mermaid & display:none
title_zh: 图表消失之谜：前端渲染机制与 Mermaid 的深度调试复盘
date: 2025-12-12
categories: DevLog
tags: Debugging, Mermaid, DOM, Frontend Engineering
summary_en: A deep dive into a tricky bug where Mermaid diagrams disappeared during language toggling. We explore why `display:none` breaks rendering, the concept of destructive DOM manipulation, and how to fix it using a "Time Travel" state restoration technique.
summary_zh: 深度复盘一个棘手的 Bug：Mermaid 图表在双语切换中离奇消失。我们探讨了为何 `display:none` 会破坏渲染、Mermaid 的破坏性 DOM 操作，以及如何通过“时光倒流”的状态还原技术完美解决它。
---

[EN]

# 🕵️ The Case of the Vanishing Diagrams
![The Ripple Effect|600](https://assets.flowstates.me/2025/20251212MermaidDebug.jpg)
While building **HDIPERS.LOG**, I encountered a classic yet frustrating frontend bug. The site supports bilingual switching (English/Chinese). Everything worked fine until I added **Mermaid.js** flowcharts.

### 🛑 The Symptom
1.  Load page (English): Diagrams look perfect.
2.  Click **"Switch to 中文"**: The Chinese text appears, but the **diagrams are gone** (or collapsed into tiny lines).
3.  Switch back to English: The original diagrams are **also broken**.
4.  Refresh page: Everything works again (until you switch).

### 🔍 The Root Cause Analysis

Debugging is not just about guessing; it's about understanding the **Browser Rendering Mechanism**.

#### 1. The `display: none` Trap
When we toggle languages, we use a CSS class that applies `display: none` to the inactive language.
*   **The Physics of DOM**: An element with `display: none` is removed from the **Render Tree**. It has a width and height of `0px`.
*   **The Crash**: When Mermaid tries to render the Chinese diagrams (which are initially hidden), it calculates the container size as 0x0. Consequently, it draws nothing.

#### 2. Destructive Rendering (The "One-Way Ticket")
Mermaid is not a "reactive" framework like React. It performs **destructive DOM manipulation**:
*   **Before Render**: `<div class="mermaid">graph TD; A-->B;</div>` (Source Code)
*   **After Render**: `<div class="mermaid"><svg>...</svg></div>` (SVG Image)

Once rendered, the **source code is lost**. When we switch languages back and forth, Mermaid sees an SVG (which it doesn't understand) instead of the original code, causing the render failure on the second pass.

### 🛠️ The Solution: "Time Travel" Restoration

To fix this, we need to manually manage the **Lifecycle** of the diagram. I implemented a **Backup & Restore** mechanism.

#### Step 1: Backup the "Source of Truth"
We hooked into the Markdown parser (`marked.js`) to save the original Mermaid code into a custom attribute `data-original-code` before it gets rendered.

```javascript
// marked.js renderer configuration
renderer.code = function(code, language) {
    if (language === 'mermaid') {
        // Safe encode the code and store it as a backup
        const safeCode = code.replace(/"/g, '&quot;');
        return `<div class="mermaid" data-original-code="${safeCode}">${code}</div>`;
    }
    return `<pre>...</pre>`;
};
```

#### Step 2: The Reset Function
We created a `rerenderMermaid` function that runs every time the language is toggled. It performs a "Time Travel" operation:

1.  **Wipe**: Finds all diagram containers.
2.  **Restore**: Replaces the broken SVG/HTML with the pristine `data-original-code`.
3.  **Reset**: Removes the `data-processed` tag so Mermaid treats them as new.
4.  **Render**: Calls `mermaid.run()` immediately after the element becomes visible.

```javascript
window.rerenderMermaid = async () => {
    document.querySelectorAll('.mermaid').forEach(node => {
        // Restore from backup
        const originalCode = node.getAttribute('data-original-code');
        if (originalCode) {
            node.removeAttribute('data-processed');
            node.innerHTML = originalCode;
        }
    });
    // Re-run rendering
    await mermaid.run({ querySelector: '.mermaid', suppressErrors: true });
};
```

---

### 🚀 Level Up: How to Master Debugging?

Solving this specific bug was satisfying, but the real value lies in the **Mental Model** improvements. If you want to solve problems efficiently, focus on these four pillars:

#### 1. Understand the Rendering Mechanism
Don't just use CSS; understand how the browser builds the page.
*   **DOM Tree vs. Render Tree**: `display: none` removes elements from the Render Tree. `visibility: hidden` keeps them but makes them invisible. This distinction is crucial for charting libraries.
*   **Reflow & Repaint**: Knowing when the browser calculates layout helps you understand why `setTimeout` is sometimes needed (to wait for the layout to stabilize).

#### 2. Lifecycle & State Management
Treat the DOM like a state machine.
*   **Init -> Update -> Destroy**: Mermaid doesn't handle "Update" automatically. We had to manually implement a "Destroy" (wipe SVG) and "Re-Init" (restore code) cycle.
*   **Source of Truth**: Never rely on the DOM as your data source if a library mutates it. Always keep a backup (like `data-original-code`).

#### 3. Master the DevTools
The Console is your best friend.
*   **Inspect Elements**: I saw the `div` was empty or had `height: 0`.
*   **Console Prototyping**: I ran `mermaid.init()` manually in the console to test if the library was loaded.
*   **Breakpoints**: Pause the code at the "Switch" button click to see exactly what order events fire in.

#### 4. Asynchronous Thinking (Event Loop)
JavaScript execution and UI Rendering are mutually exclusive.
*   When you remove `hidden` class, the element doesn't become visible *instantly*.
*   Using `setTimeout(() => ..., 0)` pushes the rendering logic to the next tick, giving the browser time to update the layout and calculate the correct width for the diagram.

> **Final Thought**: Debugging is not about fixing code; it's about correcting your mental model of how the system works.

[END]

[ZH]

# 🕵️ 图表消失之谜：前端渲染机制与 Mermaid 的深度调试复盘
![The Ripple Effect|600](https://assets.flowstates.me/2025/20251212MermaidDebug.jpg)
在构建 **HDIPERS.LOG** 的过程中，我遇到了一个经典但令人抓狂的前端 Bug。网站支持中英双语切换，一切看起来都很完美，直到我引入了 **Mermaid.js** 流程图。

### 🛑 故障现象
1.  加载英文版页面：图表显示完美。
2.  点击 **"Switch to 中文"**：中文文本出现了，但 **图表消失了**（或者缩成了一条线）。
3.  切回英文版：原本正常的英文图表也 **坏掉了**。
4.  刷新页面：一切恢复正常（直到你再次切换）。

### 🔍 根因分析 (Root Cause Analysis)

高效的 Debug 不靠猜，而是靠对 **浏览器渲染机制 (Browser Rendering Mechanism)** 的深刻理解。

#### 1. `display: none` 的陷阱
我们在切换语言时，使用了 CSS 类来隐藏非当前语言的内容（本质是 `display: none`）。
*   **DOM 物理学**: 当一个元素被设为 `display: none` 时，它会从 **渲染树 (Render Tree)** 中被移除。它的物理宽高变成了 `0px`。
*   **碰撞现场**: 当 Mermaid 尝试渲染中文图表时（初始状态为隐藏），它获取到的容器尺寸是 0x0。于是，它在一个不存在的画布上作画，结果自然是什么也没有。

#### 2. 破坏性渲染 (不可逆操作)
Mermaid 不像 React 那样是“响应式”的，它进行的是 **破坏性 DOM 操作 (Destructive DOM Manipulation)**：
*   **渲染前**: `<div class="mermaid">graph TD; A-->B;</div>` (源代码)
*   **渲染后**: `<div class="mermaid"><svg>...</svg></div>` (SVG 图片)

一旦渲染完成，**源代码就丢失了**。当我们来回切换语言时，Mermaid 面对的是一堆它看不懂的 SVG 标签，而不是最初的代码，这就导致了二次渲染失败。

### 🛠️ 解决方案：“时光倒流”大法

为了解决这个问题，我们需要手动管理图表的 **生命周期 (Lifecycle)**。我实现了一套 **“备份与还原”** 机制。

#### 第一步：备份“唯一真相” (Source of Truth)
我们 Hook 了 Markdown 解析器 (`marked.js`)，在图表被渲染前，偷偷将原始代码备份到一个自定义属性 `data-original-code` 中。

```javascript
// marked.js renderer 配置
renderer.code = function(code, language) {
    if (language === 'mermaid') {
        // 安全转义并备份源代码
        const safeCode = code.replace(/"/g, '&quot;');
        return `<div class="mermaid" data-original-code="${safeCode}">${code}</div>`;
    }
    return `<pre>...</pre>`;
};
```

#### 第二步：重置函数 (The Reset Function)
我们创建了一个 `rerenderMermaid` 函数，每次切换语言时都会触发。它执行了一次“时光倒流”：

1.  **清除**: 找到所有图表容器。
2.  **还原**: 用备份的 `data-original-code` 覆盖掉坏掉的 SVG/HTML。
3.  **重置**: 移除 `data-processed` 标记，假装这些图从来没被画过。
4.  **重绘**: 确保元素可见后，立即调用 `mermaid.run()`。

```javascript
window.rerenderMermaid = async () => {
    document.querySelectorAll('.mermaid').forEach(node => {
        // 从备份还原
        const originalCode = node.getAttribute('data-original-code');
        if (originalCode) {
            node.removeAttribute('data-processed');
            node.innerHTML = originalCode;
        }
    });
    // 强制重新渲染
    await mermaid.run({ querySelector: '.mermaid', suppressErrors: true });
};
```

---

### 🚀 升华：如何提升 Debug 能力？

解决这个 Bug 很有成就感，但更宝贵的是我们在过程中总结出的 **思维模型 (Mental Model)**。如果你想具备解决疑难杂症的能力，建议从这四个维度提升：

#### 1. 理解渲染机制 (Rendering Mechanism)
不要只会写 CSS，要理解浏览器是如何构建页面的。
*   **DOM Tree vs. Render Tree**: `display: none` 会把元素从渲染树移除，导致宽高为 0；而 `visibility: hidden` 只是看不见，位置还在。这对图表库至关重要。
*   **回流与重绘 (Reflow & Repaint)**: 理解浏览器何时计算布局，你就会明白为什么有时候需要 `setTimeout`（为了等待布局计算完成）。

#### 2. 生命周期与状态管理 (Lifecycle & State)
把 DOM 看作一个状态机。
*   **Init -> Update -> Destroy**: Mermaid 不会自动处理 "Update"。我们必须手动实现 "Destroy"（清除 SVG）和 "Re-Init"（还原代码）的闭环。
*   **单一数据源 (Source of Truth)**: 如果一个库会修改 DOM，永远不要信任 DOM 里的数据。永远保留一份备份（比如 `data-original-code`）。

#### 3. 驾驭开发者工具 (DevTools)
控制台是你最好的朋友。
*   **审查元素 (Inspect)**: 我通过它发现了 div 是空的，或者高度为 0。
*   **控制台原型验证**: 我在 Console 里手动运行 `mermaid.init()` 来测试库是否加载。
*   **断点调试**: 在“切换”按钮的点击事件里打断点，看看到底发生了什么。

#### 4. 异步思维与事件循环 (Event Loop)
牢记 JS 执行和 UI 渲染是互斥的。
*   当你移除 `hidden` 类时，元素并不会 *瞬间* 变得可见。
*   使用 `setTimeout(() => ..., 0)` 可以把渲染逻辑推到下一个“Tick”，给浏览器留出计算布局的时间，确保图表库能读到正确的宽度。

> **写在最后**: Debug 不仅仅是修代码，更是修正你对系统运作方式的认知偏差。

[END]