# 网页可访问性与现代HTML特性

## 第一部分：可访问性/ARIA与国际化

### 可访问性基础

网页可访问性（Web Accessibility）确保所有用户都能访问和使用网页内容。

#### 为什么需要可访问性？

**法律要求**：许多国家有相关的无障碍法规

**用户体验**：良好的可访问性设计对所有用户都有益

**SEO优化**：搜索引擎更好地理解页面内容

**扩大受众**：覆盖更多用户群体

#### 可访问性基本原则

| 原则 | 说明 |
| --- | --- |
| 可感知性 | 内容必须对用户是可感知的 |
| 可操作性 | 用户界面组件必须是可操作的 |
| 可理解性 | 界面信息必须是可理解的 |
| 健壮性 | 内容必须足够健壮，能被各种用户代理解释 |

#### 基本可访问性实践

<!-- 1. 使用语义化标签 -->
<header></header>
<nav></nav>
<main></main>
<button></button>
<!-- 2. 为图片添加alt属性 -->
<img src="photo.jpg" alt="一只可爱的橘猫">
<!-- 3. 确保足够的颜色对比度 -->
<div style="color: #333; background: #fff;">可读性好的文字</div>
<!-- 4. 提供清晰的焦点样式 -->
<a href="#" style="outline: 3px solid orange;">可聚焦的链接</a>
<!-- 5. 提供跳过导航的链接 -->
<a href="#main-content" class="skip-link">跳到主要内容</a>

示例：可访问的按钮和链接

普通按钮
可聚焦的链接

**提示：**使用 **Tab** 键可以在页面元素间切换焦点，这是很多用户导航网页的方式。

---

### ARIA角色与属性

ARIA（Accessible Rich Internet Applications）为HTML提供额外的语义信息，帮助辅助技术（如屏幕阅读器）理解页面内容。

#### ARIA角色（role）

角色告诉浏览器元素的作用是什么。

<div role="banner">页眉</div>
<div role="navigation">导航区域</div>
<div role="main">主要内容</div>
<div role="button">点击我</div>
<div role="alert">重要提示</div>

使用 role="button" 的元素：

点击我（使用div模拟的按钮）

#### 常用ARIA属性

| 属性 | 说明 | 示例 |
| --- | --- | --- |
| aria-label | 为元素提供可访问名称 | <button aria-label="关闭">×</button> |
| aria-describedby | 关联描述性文本 | <input aria-describedby="hint"> |
| aria-hidden | 隐藏元素不读 | <span aria-hidden="true">图标</span> |
| aria-expanded | 指示展开/收起状态 | <button aria-expanded="false">菜单</button> |
| aria-selected | 指示选中状态 | <tab aria-selected="true">选中</tab> |
| aria-pressed | 指示按下状态 | <button aria-pressed="false">切换按钮</button> |
| aria-current | 指示当前项 | <a aria-current="page">当前页</a> |

#### aria-label 使用示例

<!-- 搜索按钮只有图标，需要aria-label -->
<button aria-label="搜索">
<svg>...</svg>
</button>
<!-- 关闭按钮 -->
<button aria-label="关闭对话框">×</button>

🔍
×

#### 无障碍树形菜单示例

<ul role="tree">
<li role="treeitem" aria-expanded="true">文件夹
<ul>
<li role="treeitem">文件1</li>
<li role="treeitem">文件2</li>
</ul>
</li>
</ul>

**重要：**ARIA只是补充，不能替代语义化HTML。优先使用原生HTML元素（如 <button>、<nav>），只在必要时使用ARIA。

---

### 国际化（lang属性与字符编码）

国际化（i18n）确保网页能被不同语言和地区的用户正确理解。

#### lang 属性声明

使用 lang 属性告诉浏览器和辅助技术页面的主语言。

<!-- 声明页面语言为中文 -->
<html lang="zh-CN">
<!-- 英文页面 -->
<html lang="en">
<!-- 部分内容使用不同语言 -->
<p>
你好世界！
<span lang="en">Hello World!</span>
</p>

#### 常用语言代码

| 语言 | 代码 |
| --- | --- |
| 中文（简体） | zh-CN |
| 中文（繁体） | zh-TW |
| 英语 | en |
| 日语 | ja |
| 韩语 | ko |
| 法语 | fr |
| 德语 | de |

#### 字符编码（charset）

声明正确的字符编码，确保文字正确显示。

<!-- 推荐：UTF-8，支持所有语言 -->
<meta charset="UTF-8">
<!-- 旧式写法（不推荐） -->
<meta http-equiv="Content-Type"
content="text/html; charset=UTF-8">

#### 方向性（dir属性）

用于从右到左阅读的语言（如阿拉伯语、希伯来语）。

<!-- 从左到右（默认） -->
<p dir="ltr">中文English</p>
<!-- 从右到左 -->
<p dir="rtl">مرحبا</p>
<!-- 自动检测（浏览器决定） -->
<p dir="auto">内容</p>

**提示：**始终使用 <meta charset="UTF-8"> 并保存文件为UTF-8编码，这样可以避免大多数乱码问题。

---

## 第二部分：现代HTML特性与Web组件

### 自定义元素（Custom Elements）

自定义元素允许创建自定义的HTML标签，是Web组件的核心技术之一。

#### 定义自定义元素

// 定义一个自定义元素
class MyElement extends HTMLElement {
connectedCallback() {
this.innerHTML = '<h2>我的自定义元素</h2>';
}
}
// 注册自定义元素
customElements.define('my-element', MyElement);

// 使用自定义元素
<my-element></my-element>

自定义元素示例（由JavaScript创建）：

#### 生命周期回调

| 回调 | 调用时机 |
| --- | --- |
| connectedCallback | 元素被添加到DOM |
| disconnectedCallback | 元素从DOM移除 |
| attributeChangedCallback | 属性变化时 |
| adoptedCallback | 移动到新文档时 |

---

### 模板与插槽

#### <template> 元素

<template> 用于定义可复用的HTML片段，不会被渲染。

<!-- 定义模板 -->
<template id="my-template">
<div class="card">
<h2></h2>
<p></p>
</div>
</template>
<!-- 使用模板 -->
<script>
const template = document.getElementById('my-template');
const clone = template.content.cloneNode(true);
document.body.appendChild(clone);
</script>

模板示例：

### 模板标题

这是模板中的内容

点击使用模板

#### <slot> 插槽

插槽允许在自定义元素中插入自定义内容。

// 定义带插槽的自定义元素
class MyCard extends HTMLElement {
connectedCallback() {
this.innerHTML = `
<div style="border: 1px solid #ddd; padding: 15px;">
<h2><slot name="title">默认标题</slot></h2>
<p><slot>默认内容</slot></p>
</div>
`;
}
}
customElements.define('my-card', MyCard);

<!-- 使用带插槽的自定义元素 -->
<my-card>
<span slot="title">自定义标题</span>
这是插入的内容
</my-card>

**提示：**模板和插槽是Web组件的三大基础技术之二，另一个是Shadow DOM。

---

### Shadow DOM

Shadow DOM 允许将隐藏的DOM树附加到元素上，实现样式和结构的封装。

#### 什么是Shadow DOM？

* 隔离样式：组件样式不会影响外部
* 隔离DOM：组件内部结构不会与外部冲突
* 私有实现：隐藏实现细节

#### 创建Shadow DOM

const host = document.getElementById('host-element');
const shadow = host.attachShadow({mode: 'open'});
shadow.innerHTML = `
<style>
.inner { color: blue; font-weight: bold; }
</style>
<div class="inner">这是Shadow DOM中的内容</div>
`;

Shadow DOM 示例：

外部内容

**说明：**很多原生HTML元素（如 <video>、<input>、<select>）内部都使用Shadow DOM实现，这就是为什么有时很难用CSS自定义它们的原因。

---

### 可编辑内容

contenteditable 属性让任意元素变为可编辑。

<!-- 让段落变为可编辑 -->
<p contenteditable="true">
点击这里编辑文本
</p>
<!-- 整个区域可编辑 -->
<div contenteditable="true"
style="border: 1px solid #ccc; padding: 10px;">
编辑这个区域的内容
</div>

可编辑内容示例（点击文本可以编辑）：

点击这里编辑这段文本...

整个区域都可以编辑，尝试在这里输入文字！

#### 相关JavaScript属性

// 检测元素是否可编辑
element.isContentEditable
// 获取/设置编辑权限
element.contentEditable = "true"; // 启用
element.contentEditable = "false"; // 禁用
element.contentEditable = "inherit"; // 继承父元素

**提示：**contenteditable 与 <textarea> 或 <input> 不同，它允许丰富的HTML内容编辑，但需要自己处理保存逻辑。

---

## 第三部分：HTML与CSS/JS集成

### 内联样式与样式表

#### 1. 内联样式（inline style）

直接写在元素的 style 属性中。

<p style="color: red; font-size: 16px;">红色文字</p>
<div style="background: blue; padding: 20px;">蓝色背景</div>

红色文字（内联样式）

蓝色背景（内联样式）

#### 2. 内部样式表（internal style）

写在 <head> 的 <style> 标签中。

<style>
.highlight {
background: yellow;
padding: 5px;
}
</style>

#### 3. 外部样式表（external style）

单独的 .css 文件，通过 <link> 引入。

<link rel="stylesheet" href="styles.css">

#### 样式优先级

| 方式 | 优先级 | 示例 |
| --- | --- | --- |
| !important | 最高 | color: red !important; |
| 内联样式 | 高 | <p style="..."> |
| ID选择器 | 中 | #header { } |
| 类/属性/伪类 | 中低 | .btn { } |
| 元素选择器 | 低 | p { } |

**最佳实践：**优先使用外部样式表，保持HTML和CSS分离，提高代码可维护性。

---

### 脚本嵌入

#### 1. 内部脚本（inline script）

直接在HTML中嵌入JavaScript代码。

<script>
function sayHello() {
alert('你好！');
}
</script>
<button onclick="sayHello()">点击我</button>

#### 2. 外部脚本（external script）

引用单独的 .js 文件。

<script src="app.js"></script>

#### 脚本加载位置

<!-- 放在body末尾（推荐） -->
<body>
<!-- HTML内容 -->
<script src="app.js"></script>
</body>
<!-- 使用 defer（推荐） -->
<head>
<script defer src="app.js"></script>
</head>
<!-- 使用 async -->
<head>
<script async src="app.js"></script>
</head>

#### defer vs async

| 属性 | 行为 | 适用场景 |
| --- | --- | --- |
| defer | 等HTML解析完成后执行，按顺序 | 大多数脚本 |
| async | 下载完立即执行，不阻塞HTML | 独立脚本（如统计） |
| 无属性 | 阻塞HTML解析 | 尽量避免 |

点击按钮测试脚本：

内联脚本
外部脚本效果

---

### 事件处理

#### 1. HTML属性方式（不推荐）

<button onclick="handleClick()">点击</button>

#### 2. DOM属性方式

<button id="myBtn">点击</button>
<script>
const btn = document.getElementById('myBtn');
btn.onclick = function() {
alert('点击了！');
};
</script>

#### 3. addEventListener 方式（推荐）

<button id="myBtn2">点击</button>
<script>
const btn = document.getElementById('myBtn2');
// 添加事件监听
btn.addEventListener('click', function(event) {
console.log('点击了！', event);
});
// 添加多个监听器
btn.addEventListener('click', () => {
alert('第二个监听器');
});
</script>

事件处理示例：

点击测试事件
多事件监听器

点击按钮查看效果...

#### 常用事件类型

| 类别 | 事件 | 说明 |
| --- | --- | --- |
| 鼠标 | click, dblclick, mouseenter, mouseleave, mousemove | 鼠标操作 |
| 键盘 | keydown, keyup, keypress | 键盘操作 |
| 表单 | submit, change, input, focus, blur | 表单操作 |
| 文档 | DOMContentLoaded, load, resize, scroll | 文档状态 |
| 触摸 | touchstart, touchmove, touchend | 触屏设备 |

**最佳实践：**使用 addEventListener 而非 onclick，可以添加多个监听器，更灵活，且更容易管理内存。

---

以上是网页可访问性与现代HTML特性的全部内容