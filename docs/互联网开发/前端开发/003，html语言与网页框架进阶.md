# HTML语言进阶教程

## 第一部分：HTML5 语义化结构

HTML5 引入了许多语义化标签，使文档结构更清晰，更有利于搜索引擎优化和辅助技术理解。

### 文档结构标签

#### <header> - 页眉

用于定义页面或章节的头部内容，通常包含标题、导航链接等。

<header>
<h1>网站标题</h1>
<nav>导航菜单</nav>
</header>

#### <nav> - 导航

用于定义导航链接区域。

<nav>
<a href="/home">首页</a>
<a href="/about">关于</a>
</nav>

#### <main> - 主内容

用于定义页面的主内容区域，一个页面只能有一个 <main> 元素。

<main>
<h1>文章标题</h1>
<p>文章内容...</p>
</main>

#### <article> - 文章

用于定义独立的、可分发的内容单元，如博客文章、新闻报道等。

<article>
<h2>文章标题</h2>
<p>文章内容...</p>
</article>

#### <section> - 章节

用于将相关内容分组，通常配合标题使用。

<section>
<h2>章节标题</h2>
<p>章节内容...</p>
</section>

#### <aside> - 侧边栏

用于定义与主内容相关但可独立的辅助信息。

<aside>
<h3>相关文章</h3>
<ul>
<li>链接1</li>
<li>链接2</li>
</ul>
</aside>

#### <footer> - 页脚

用于定义页面或章节的底部内容，通常包含版权信息、联系方式等。

<footer>
<p>© 2024 公司名称</p>
</footer>

#### 综合示例

**header** - 页眉区域

**nav** - 导航区域

**main** - 主内容区域

**article** - 文章内容

**section** - 章节内容

**aside** - 侧边栏

**footer** - 页脚区域

**注意：**<article> 和 <section> 的区别：article 更强调独立性（一篇文章），section 更强调相关性（一章内容）。但两者都可以嵌套使用。

---

### 分组内容标签

#### <figure> 和 <figcaption> - 图片组

用于将媒体内容及其标题组合在一起。

<figure>
<img src="image.jpg" alt="图片描述">
<figcaption>图片标题说明</figcaption>
</figure>

[图片区域]

图1：示例图片说明

#### <details> 和 <summary> - 折叠内容

用于创建可折叠的详细信息区域。

<details>
<summary>点击展开/收起</summary>
<p>这里是隐藏的详细内容...</p>
</details>

点击展开/收起

这里是隐藏的详细内容，可以包含任意HTML元素。

* 列表项1
* 列表项2

#### <div> - 无语义容器

<div> 是最通用的容器，没有任何语义含义，仅用于布局和样式控制。

<div class="container">
<div class="header">头部</div>
<div class="content">内容</div>
</div>

**提示：**语义化标签的主要优势：
1. **SEO**：搜索引擎能更好理解页面结构
2. **可访问性**：屏幕阅读器能更准确解读内容
3. **代码可读性**：开发者更容易理解代码
4. **维护性**：便于团队协作和后期维护

---

### 进度与度量标签

#### <progress> - 进度条

用于显示任务的完成进度。

<!-- 不确定进度的进度条 -->
<progress></progress>
<!-- 确定进度的进度条 -->
<progress value="70" max="100"></progress>

加载中（不确定进度）：

下载进度（70%）：

#### <meter> - 度量衡

用于显示已知范围内的标量值，如磁盘使用量、评分等。

<meter value="2" min="0" max="10">2 out of 10</meter>
<meter value="0.5">50%</meter>

磁盘使用量：

评分（6/10）：

低值（黄色）：

高值（红色）：

**注意：**<progress> 用于显示"进行中"的任务，而 <meter> 用于显示已知范围内的静态值。

---

由于浏览器的原生渲染行为，low/high区域默认选择了黄色，无法通过css进行修改，我这里偷个小懒不改了，哎嘿

---

## 第二部分：高级表单与 HTML5 输入类型

### HTML5 输入类型

HTML5 新增了许多输入类型，提供了更好的数据验证和用户体验。

| 输入类型 | 说明 | 示例 |
| --- | --- | --- |
| email | 电子邮件地址 |  |
| url | 网址 |  |
| tel | 电话号码 |  |
| search | 搜索框 |  |
| number | 数字 |  |
| range | 范围滑块 |  |
| color | 颜色选择器 |  |
| date | 日期 |  |
| time | 时间 |  |
| datetime-local | 本地日期时间 |  |
| month | 月份 |  |
| week | 周 |  |

#### 代码示例

<input type="email" placeholder="请输入邮箱">
<input type="number" min="0" max="100">
<input type="range" min="0" max="100">
<input type="color">
<input type="date">

---

### 表单属性

#### 常用表单属性

| 属性 | 说明 | 示例 |
| --- | --- | --- |
| placeholder | 输入提示 |  |
| required | 必填项 |  |
| readonly | 只读 |  |
| disabled | 禁用 |  |
| autocomplete | 自动完成 |  |
| pattern | 验证正则 |  |
| min / max | 最小/最大值 |  |
| step | 步长 |  |

#### 代码示例

<input type="text" placeholder="请输入用户名" required>
<input type="number" min="0" max="100" step="10">
<input type="text" pattern="[0-9]{11}" title="请输入11位手机号">

**提示：**required 属性会在提交时自动验证，pattern 属性使用正则表达式进行验证。

---

### 数据列表 <datalist>

<datalist> 为输入框提供预定义的建议选项。

<input list="browsers" name="browser">
<datalist id="browsers">
<option value="Chrome">
<option value="Firefox">
<option value="Safari">
<option value="Edge">
</datalist>

选择浏览器：

---

### 输出元素 <output>

<output> 用于显示计算结果或用户操作的输出。

<form oninput="result.value = parseInt(a.value) + parseInt(b.value)">
<input type="number" id="a" value="0"> +
<input type="number" id="b" value="0"> =
<output name="result"></output>
</form>

+
 =
30

---

## 第三部分：嵌入内容

### <iframe> 嵌入页面

<iframe> 用于在当前页面中嵌入另一个页面。

<iframe
src="https://www.example.com"
width="600"
height="400"
title="示例页面"
frameborder="0">
</iframe>

这是一个iframe嵌入示例：

#### 常用属性

| 属性 | 说明 |
| --- | --- |
| src | 嵌入页面的URL |
| width/height | iframe的宽高 |
| frameborder | 边框（0或1） |
| allowfullscreen | 允许全屏 |
| sandbox | 安全沙箱限制 |

**安全提示：**使用 sandbox 属性可以限制iframe的行为，提高安全性。例如：sandbox="allow-same-origin allow-scripts" 只允许同源和脚本。

---

### Canvas 与 SVG

#### <canvas> - 画布

canvas 是HTML5新增的元素，通过JavaScript绘制图形、动画等。

<canvas id="myCanvas" width="400" height="200">
您的浏览器不支持 canvas
</canvas>
<script>
var canvas = document.getElementById('myCanvas');
var ctx = canvas.getContext('2d');
ctx.fillStyle = '#3498db';
ctx.fillRect(10, 10, 150, 100);
ctx.fillStyle = '#e74c3c';
ctx.beginPath();
ctx.arc(300, 60, 40, 0, Math.PI \* 2);
ctx.fill();
</script>

您的浏览器不支持 canvas

#### <svg> - 可缩放矢量图形

SVG 使用XML格式定义矢量图形，可以无限缩放而不失真。

<svg width="200" height="150">
<rect x="10" y="10" width="80" height="60"
fill="#3498db" rx="10"/>
<circle cx="150" cy="40" r="30"
fill="#e74c3c"/>
<text x="50" y="130">SVG 示例</text>
</svg>

SVG 矢量图形

**Canvas vs SVG 对比：**

* **Canvas**：位图，适合大量图形、游戏、动画，像素操作
* **SVG**：矢量图，适合图标、图表、UI元素，可缩放不失真

---

## 第四部分：元数据与 SEO 基础

### 文档元数据

元数据用于描述网页的信息，帮助浏览器和搜索引擎理解页面内容。

#### <meta> 标签

<!-- 字符编码 -->
<meta charset="UTF-8">
<!-- 视口设置（响应式） -->
<meta name="viewport"
content="width=device-width, initial-scale=1.0">
<!-- 页面描述 -->
<meta name="description"
content="这是一个关于Web开发的教程网站">
<!-- 关键词（已被大多数搜索引擎忽略） -->
<meta name="keywords"
content="HTML, CSS, JavaScript, Web开发">
<!-- 作者 -->
<meta name="author"
content="张三">
<!-- 页面刷新 -->
<meta http-equiv="refresh"
content="30">

#### <link> 标签 - 链接资源

<!-- 引入CSS -->
<link rel="stylesheet" href="style.css">
<!-- 引入图标 -->
<link rel="icon" href="favicon.ico">
<!-- 预连接外部域 -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<!-- 预加载资源 -->
<link rel="preload" href="image.png" as="image">

#### <title> - 页面标题

<title>网站名称 - 页面标题</title>

**重要：**<title> 标签是搜索引擎优化最重要的元素之一，应该包含主要关键词，长度控制在50-60个字符以内。

---

### SEO 相关知识

#### 语义化HTML对SEO的影响

搜索引擎通过分析HTML结构来理解页面内容，语义化标签可以帮助搜索引擎更好地索引页面。

| 标签 | SEO意义 |
| --- | --- |
| <title> | 显示在搜索结果标题，极其重要 |
| <h1> - <h6> | 定义内容层次结构，<h1>最重要 |
| <meta name="description"> | 显示在搜索结果摘要中 |
| <header> | 页面头部信息权重较高 |
| <nav> | 帮助理解网站导航结构 |
| <main> | 明确主要内容区域 |
| <article> | 独立内容，利于索引 |
| <alt> 属性 | 图片SEO的关键属性 |

#### 图片优化

<img src="product.jpg"
alt="药物产品图片"
title="点击查看详情"
loading="lazy"
width="800"
height="600">

* **alt**：图片的替代文字，搜索引擎能识别图片内容
* **title**：鼠标悬停时显示的文字
* **loading="lazy"**：延迟加载，提高页面性能
* **width/height**：避免页面布局跳动

#### 链接优化

<!-- 好的链接写法 -->
<a href="/products/medicine"
title="查看我们的药品系列">
药品系列
</a>

以上是HTML语言进阶教程的全部内容