DOM（Document Object Model，文档对象模型）是 HTML 和 XML 文档的编程接口。它将网页文档表示为一个**树状结构**，让 JavaScript 可以动态地访问、修改页面的内容、结构和样式。

如果说 HTML 是网页的“源代码”，那么 DOM 就是浏览器在内存中实时维护的“操作面板”。

---

### 1. DOM 树结构 (The DOM Tree)

在 DOM 中，每一个 HTML 标签、属性、甚至一段文字都被看作是一个**节点 (Node)**。这些节点按照层级关系组织在一起。

- **Document (根节点)：** 代表整个网页，是访问所有元素的入口。
    
- **Element (元素节点)：** HTML 标签，如 `<div>`, `<a>`, `<p>`。
    
- **Attribute (属性节点)：** 标签的属性，如 `href`, `src`, `class`。
    
- **Text (文本节点)：** 标签内部包含的文字。
    

### 2. 核心 DOM 对象与常用 API

作为前端开发者，你主要通过以下几类方法来操控 DOM：

#### A. 查找元素 (Selectors)

- `document.getElementById('id')`: 通过 ID 获取单个元素。
    
- `document.querySelector('.class / #id')`: 使用 CSS 选择器获取第一个匹配项（非常常用）。
    
- `document.querySelectorAll('div')`: 获取所有匹配的元素集合。
    

#### B. 修改内容与属性

- `element.innerHTML`: 获取或设置 HTML 内容（支持标签）。
    
- `element.textContent`: 获取或设置纯文本。
    
- `element.setAttribute('name', 'value')`: 修改属性。
    
- `element.classList.add('active')`: 动态操作 CSS 类名。
    

#### C. 节点操作 (增删改)

- `document.createElement('div')`: 创建一个新标签。
    
- `element.appendChild(newNode)`: 将新节点插入到父元素末尾。
    
- `element.remove()`: 删除当前元素。