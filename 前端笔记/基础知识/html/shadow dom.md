
Shadow DOM 是 **Web Components** 核心规范之一。它允许开发者将一个隐藏的、独立的 DOM 树附加到常规的 HTML 元素上，从而实现真正的组件级**封装**。

---

## 1. 核心优势：封装性

在传统的网页开发中，CSS 和 JS 全局作用域往往会导致样式冲突。Shadow DOM 解决了以下痛点：

- **样式隔离（Scoped CSS）**：在 Shadow DOM 内部定义的样式不会影响外部，外部的样式也不会污染内部。
    
- **DOM 隔离**：外部的 `document.querySelector` 无法直接选到 Shadow DOM 内部的节点，保证了组件内部结构的安全性。
    

---

## 2. 关键术语

- **Shadow Host（宿主）**：Shadow DOM 附加到的常规 DOM 节点。
    
- **Shadow Tree（影子树）**：Shadow DOM 内部的 DOM 树。
    
- **Shadow Boundary（边界）**：影子树与常规 DOM 的分界线。
    
- **Shadow Root（根）**：影子树的根节点。
    

---

## 3. 基本用法

通过 JavaScript 的 `attachShadow()` 方法即可创建一个影子根。

JavaScript

```
// 1. 获取或创建一个宿主元素
const host = document.querySelector('#my-element');

// 2. 附加 Shadow Root
// mode: 'open' 表示可以通过 host.shadowRoot 访问内部
const shadow = host.attachShadow({ mode: 'open' });

// 3. 编写内部结构与样式
shadow.innerHTML = `
  <style>
    p { color: #2ecc71; font-family: sans-serif; }
  </style>
  <p>你好，这是在 Shadow DOM 内部的文字！</p>
`;
```

---

## 4. Slot（插槽）机制

Shadow DOM 通过 `<slot>` 标签实现内容的分发。它允许你在保持组件结构不变的情况下，从外部注入自定义内容：

HTML

```
<template id="my-card">
  <div class="card">
    <h2><slot name="title">默认标题</slot></h2>
    <div class="content">
      <slot></slot> </div>
  </div>
</template>
```

---

## 5. 常见模式对比

|**模式**|**说明**|
|---|---|
|**Open (开放)**|最常用。外部可以通过 `element.shadowRoot` 访问并操作影子树。|
|**Closed (封闭)**|外部无法通过 JS 访问内部，通常用于浏览器原生控件（如 `<video>`）。|


创建一个 Shadow DOM 的过程非常直接。在现代浏览器中，主要通过 JavaScript 的 `attachShadow()` 方法来实现。

以下是实现 Shadow DOM 的标准步骤和代码示例：

---

### 1. 基础实现步骤

要创建一个 Shadow DOM，你通常需要执行以下三个动作：

1. **选取宿主 (Host)**：选择一个普通的 HTML 元素作为容器。
    
2. **附加根节点 (Root)**：调用 `attachShadow` 创建影子根。
    
3. **注入内容**：向影子根中添加 HTML 或 CSS。
    

#### 代码示例：

JavaScript

```
// 1. 获取宿主元素
const hostElement = document.querySelector('#my-container');

// 2. 附加 Shadow Root
// mode: 'open' 意味着你可以通过 hostElement.shadowRoot 访问内部
const shadowRoot = hostElement.attachShadow({ mode: 'open' });

// 3. 向 Shadow DOM 添加内容
shadowRoot.innerHTML = `
  <style>
    /* 这里的样式是完全隔离的，不会影响页面其他部分 */
    p {
      color: white;
      background: #333;
      padding: 10px;
      border-radius: 4px;
    }
  </style>
  <p>这是一个被 Shadow DOM 保护的段落。</p>
`;
```

---

### 2. 在 Web Components (Custom Elements) 中创建

在实际开发（如开发 UI 组件库）时，通常会将 Shadow DOM 与 **自定义元素** 结合使用。这种方式更符合面向对象编程的逻辑。

JavaScript

```
class MyVisualComponent extends HTMLElement {
  constructor() {
    super();
    // 在构造函数中创建 Shadow DOM
    const shadow = this.attachShadow({ mode: 'open' });
    
    const wrapper = document.createElement('div');
    wrapper.textContent = "组件内部内容";
    
    shadow.appendChild(wrapper);
  }
}

// 注册自定义标签
customElements.define('my-visual-component', MyVisualComponent);
```

然后在 HTML 中直接使用：`<my-visual-component></my-visual-component>`。

---

### 3. 使用 `<template>` 创建（推荐做法）

如果你觉得在 JS 里写字符串（`innerHTML`）很麻烦，可以使用 HTML 的 `<template>` 标签。这种方式在处理复杂的 3D 渲染容器或复杂的 UI 布局时非常清晰。

**HTML:**

HTML

```
<template id="my-template">
  <style>
    .box { border: 2px solid gold; padding: 20px; }
  </style>
  <div class="box">
    <h3>标题</h3>
    <slot></slot> </div>
</template>

<div id="host"></div>
```

**JavaScript:**

JavaScript

```
const host = document.querySelector('#host');
const template = document.querySelector('#my-template');

const shadowRoot = host.attachShadow({ mode: 'open' });
// 克隆模板内容并添加到 shadowRoot
shadowRoot.appendChild(template.content.cloneNode(true));
```

---

### 4. 关键注意事项

- **并非所有元素都能附加 Shadow DOM**：出于安全和架构考虑，浏览器不允许在某些元素上附加 Shadow DOM，例如 `<img>`、`<input>`、`<textarea>` 等。常用的通常是 `<div>`、`<section>`、`<span>` 或自定义元素。
    
- **Mode 的选择**：
    
    - `mode: 'open'`：最常用，允许外部 JS 通过 `host.shadowRoot` 访问。
        
    - `mode: 'closed'`：禁止外部访问，安全性更高但调试和交互更难。
        
- **事件冒泡**：Shadow DOM 内部触发的事件在经过边界时会被“重定向”，看起来像是从宿主元素发出的，这保护了组件的内部细节。