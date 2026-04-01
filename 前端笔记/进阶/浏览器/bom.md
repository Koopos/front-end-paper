
BOM（Browser Object Model，浏览器对象模型）提供了与浏览器窗口进行交互的对象。与 DOM 不同，BOM 并没有正式的标准，但现代浏览器都实现了一套通用的 API。

BOM 的核心是 **`window` 对象**，它是所有其他对象的“家长”。

---

### 1. window 对象 (核心)

它是浏览器的全局对象。在网页中定义的任何变量和函数，都会变成 `window` 的属性和方法。

- **功能：** 控制窗口大小、位置、打开/关闭新窗口。
    
- **常见方法：** `alert()`, `confirm()`, `setTimeout()`, `setInterval()`。
    

### 2. location 对象 (地址栏)

用于获取或设置当前页面的 URL，并且可以用来重定向页面。

- **常用属性：**
    
    - `location.href`: 整个 URL。
        
    - `location.hostname`: 主机名。
        
    - `location.pathname`: 路径名。
        
- **常用方法：** `location.reload()` (刷新), `location.replace()` (替换当前历史记录)。
    

### 3. navigation 对象 (浏览器信息)

包含有关浏览器的信息，常用于识别浏览器类型、版本和操作系统。

- **常用属性：**
    
    - `navigator.userAgent`: 浏览器的用户代理字符串。
        
    - `navigator.onLine`: 检查用户是否联网。
        
    - `navigator.clipboard`: 访问剪贴板 API。
        

### 4. history 对象 (历史记录)

允许你与浏览器的会话历史记录进行交互（即浏览器的“前进”和“后退”按钮）。

- **常用方法：** `history.back()`, `history.forward()`, `history.go(-1)`。
    
- **现代 Web 开发：** 配合 `pushState` 和 `replaceState` 实现单页应用（SPA）的路由跳转而不刷新页面。
    

### 5. screen 对象 (屏幕信息)

包含关于用户屏幕分辨率、颜色深度等信息。

- **常用属性：** `screen.width`, `screen.height`, `screen.availWidth` (可用宽度，扣除任务栏)。
    

### 6. document 对象 (连接点)

虽然 `document` 是 DOM 的核心，但它在逻辑上也属于 `window` 对象的一个属性（`window.document`），是 BOM 与 DOM 的交叉点。

---

### 总结对比表

|对象|职责|典型用途|
|---|---|---|
|**window**|全局容器 / 窗口控制|设置定时器、弹窗、获取视口大小|
|**location**|URL 与导航|页面跳转、解析网址参数|
|**navigator**|浏览器与设备信息|判断设备类型、检测联网状态|
|**history**|浏览历史|实现“返回上一页”功能|
|**screen**|显示器属性|获取屏幕分辨率进行埋点统计|