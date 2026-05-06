# HTML语言入门教程

下面是一个最基本的 HTML 文件示例：

```html
<!DOCTYPE html> <!-- 声明文档类型 -->
<html> <!-- 根元素 -->
<head> <!-- 头部信息 -->
    <meta charset="UTF-8">
    <title>页面标题</title>
</head>
<body> <!-- 可见内容 -->
    <p>页面内容</p>
</body>
</html>
```

---

## 第一部分：HTML简介

### 什么是 HTML？

HTML（HyperText Markup Language，超文本标记语言）是用于创建网页的标准标记语言。

**核心概念：**

* **标记语言**：不是编程语言，而是一种用来结构化信息的语言
* **超文本**：除了文本，还可以包含图片、链接、音视频等
* **浏览器负责解析**：浏览器读取 HTML 文件并渲染成可视化页面

### HTML 的基本结构

**结构说明：**

| 标签 | 作用 |
| --- | --- |
| `<!DOCTYPE html>` | 声明文档类型，告诉浏览器用 HTML5 标准解析 |
| `<html>` | 根元素，所有其他元素的容器 |
| `<head>` | 头部，包含页面的元信息（不显示在页面中） |
| `<body>` | 主体，包含所有可见的页面内容 |

**提示：**HTML 标签通常是成对出现的，如 `<html>` 和 `</html>`，但也有单标签，如 `<meta>` 和 `<br>`。

---

## 第二部分：常用的文本标签

### 标题标签（`h1`-`h6`）

标题标签用于定义标题，从 `h1` 到 `h6`，重要性依次递减。

```html
<h1>一级标题（最重要的标题）</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<h4>四级标题</h4>
<h5>五级标题</h5>
<h6>六级标题（最不重要的标题）</h6>
```
渲染效果如下：
# 一级标题（最重要的标题）

## 二级标题

### 三级标题

#### 四级标题

##### 五级标题

###### 六级标题（最不重要的标题）

**规范：**一个页面通常只有一个 `h1` 标签，用于页面主标题。搜索引擎会重点关注 `h1` 标签。

### 段落和换行

```html
<p>这是一个段落，浏览器会自动在段落前后添加间距。</p>
<p>这是另一个段落。</p>
<!-- 换行标签 -->
<p>第一行<br>第二行<br>第三行</p>
```

这是一个段落，浏览器会自动在段落前后添加间距。

这是另一个段落。

第一行  
第二行  
第三行

### 文本格式化标签

```html
<p>普通文本</p>
<p><strong>加粗文本（强调）</strong></p>
<p><b>加粗文本（视觉）</b></p>
<p><em>斜体文本（强调）</em></p>
<p><i>斜体文本（视觉）</i></p>
<p><u>下划线文本</u></p>
<p><del>删除线文本</del></p>
<p>上标：x<sup>2</sup></p>
<p>下标：H<sub>2</sub></p>
```

普通文本

**加粗文本（强调）**

**加粗文本（视觉）**

*斜体文本（强调）*

*斜体文本（视觉）*

<u>下划线文本</u>

~~删除线文本~~

上标：x<sup>2</sup>

下标：H<sub>2</sub>

**提示：**`<strong>` 和 `<em>` 有语义意义（表示强调），而 `<b>` 和 `<i>` 只有视觉效果。搜索引擎更看重有语义的标签。

### 列表标签

#### 无序列表

```html
<ul>
    <li>苹果</li>
    <li>香蕉</li>
    <li>橙子</li>
</ul>
```

* 苹果
* 香蕉
* 橙子

#### 有序列表

```html
<ol>
    <li>第一步：打开冰箱</li>
    <li>第二步：放入大象</li>
    <li>第三步：关上冰箱</li>
</ol>
```

1. 第一步：打开冰箱
2. 第二步：放入大象
3. 第三步：关上冰箱

### 超链接

```html
<!-- 跳转到其他页面 -->
<a href="https://www.baidu.com">跳转到百度</a>

<!-- 在新标签页打开 -->
<a href="https://www.google.com" target="_blank">在新标签页打开 Google</a>

<!-- 跳转到页面内位置（锚点） -->
<a href="#section1">跳转到第一部分</a>
<h2 id="section1">第一部分</h2>
```

[跳转到百度](https://www.baidu.com)

[在新标签页打开 Google](https://www.google.com)

[跳转到下一行](#section1)

<h2 id="section1">将会跳转到此行</h2>

### 图片标签

```html
<!-- 基本用法 -->
<img src="图片地址" alt="图片描述">

<!-- 完整示例 -->
<img src="https://ask-fd.zol-img.com.cn/g5/M00/09/09/ChMkJlpCjsGIQjpbAAOIUBAwhTUAAjhIgBBhQsAA4ho721.jpg"
     alt="占位图片"
     width="300"
     height="150">
```

![占位图片](https://ask-fd.zol-img.com.cn/g5/M00/09/09/ChMkJlpCjsGIQjpbAAOIUBAwhTUAAjhIgBBhQsAA4ho721.jpg)

**特点：**`<img>` 是单标签，不需要闭合。`alt` 属性用于图片无法显示时的替代文本，也利于搜索引擎理解图片内容。

---

## 第三部分：HTML 标签属性

### 什么是属性？

属性是标签的额外信息，用于描述标签的特征。属性写在开始标签中。

```html
<标签名 属性名="属性值">内容</标签名>
```

### 通用属性（所有标签都支持）

| 属性 | 说明 | 示例 |
| --- | --- | --- |
| `id` | 元素的唯一标识 | `id="header"` |
| `class` | 元素的类名（用于 CSS） | `class="container"` |
| `style` | 内联样式 | `style="color:red;"` |
| `title` | 鼠标悬停显示的提示 | `title="详细信息"` |

### 常用属性示例

```html
<!-- id 属性：唯一标识 -->
<div id="main-content">主要内容区域</div>

<!-- class 属性：用于样式分组 -->
<p class="highlight">这段文字有高亮样式</p>

<!-- style 属性：直接添加样式 -->
<p style="color: blue; font-size: 20px;">蓝色 20 像素的文字</p>

<!-- title 属性：鼠标悬停提示 -->
<p title="这是一段提示文字">鼠标悬停在我上面看看</p>
```

主要内容区域（`id="main-content"`）

这段文字有高亮样式（`class="highlight"`）

蓝色 20 像素的文字（`style="color: blue;"`）

鼠标悬停在我上面看看

### 特殊属性

#### 链接的 `href` 属性

```html
<a href="https://www.example.com">外部链接</a>
<a href="page.html">本地链接</a>
<a href="#top">锚点链接</a>
<a href="javascript:void(0);">不跳转</a>
```

#### 图片的 `src` 和 `alt` 属性

```html
<img src="logo.png" alt="网站 Logo">
```

#### 输入框的 `value` 属性

```html
<input type="text" value="默认值">
<input type="password" placeholder="请输入密码">
```

**提示：**属性值通常用引号括起来，单引号或双引号都可以。推荐使用双引号。

---

## 第四部分：块元素与行内元素

### 概念区分

HTML 元素可以分为两大类：**块元素**和**行内元素**。

| 特征 | 块元素（Block） | 行内元素（Inline） |
| --- | --- | --- |
| 是否独占一行 | 是，独占一行 | 否，与其他元素共享一行 |
| 可以设置宽高 | 可以 | 不行（`width`/`height` 无效） |
| 可以设置 `margin`/`padding` | 四周都可以 | 只有左右有效 |
| 常见标签 | `div`, `p`, `h1`-`h6`, `ul`, `ol`, `li` | `span`, `a`, `strong`, `em`, `img` |

### 块元素示例

```html
<div>这是一个 div 块元素</div>
<p>这是一个段落块元素</p>
<h3>这是一个标题块元素</h3>
```

这是一个 `div` 块元素

这是一个段落块元素

### 这是一个标题块元素

### 行内元素示例

```html
<span>这是 span 行内元素</span>
<strong>加粗</strong>
<em>斜体</em>
<a href="#">链接</a>
```

这是 `span` 行内元素
**加粗**
*斜体*
[链接](#)

### 两者的重要区别

```html
<!-- 块元素可以包含行内元素 -->
<div>
    <p>段落</p>
    <a href="#">链接</a>
</div>

<!-- 行内元素通常不包含块元素 -->
<span>
    <p>这是不规范的写法！</p>
</span>
```

### 特殊元素：行内块元素

`<img>` 和 `<input>` 是特殊的行内块元素，既可以与其他元素在同一行，又可以设置宽高。

```html
<img src="..." width="100" height="100">
<input type="text">
<input type="text">
```

---

## 第五部分：HTML 表单

### 表单简介

表单用于收集用户输入的数据，是网页与用户交互的重要方式。

### 表单基本结构

```html
<form action="提交地址" method="提交方式">
    <!-- 表单控件 -->
    <input type="text">
    <button>提交</button>
</form>
```

| 属性 | 说明 | 可选值 |
| --- | --- | --- |
| `action` | 表单提交的服务器地址 | URL 路径 |
| `method` | 提交方式 | `GET` / `POST` |
| `target` | 提交后在哪里显示响应 | `_self` / `_blank` |

### 各种输入类型

#### 文本输入
```html
<label>用户名：</label>
<input type="text" placeholder="请输入用户名">
```

#### 密码输入
```html
<label>密码：</label>
<input type="password" placeholder="请输入密码">
```

#### 单选按钮
```html
<label>性别：</label>
<input type="radio" name="gender" value="male"> 男
<input type="radio" name="gender" value="female"> 女
```

#### 复选框
```html
<label>爱好：</label>
<input type="checkbox" name="hobby" value="reading"> 唱歌
<input type="checkbox" name="hobby" value="sports"> 跳舞
<input type="checkbox" name="hobby" value="music"> rap
```

#### 下拉选择
```html
<label>城市：</label>
<select>
    <option>请选择</option>
    <option>北京</option>
    <option>上海</option>
    <option>广州</option>
    <option>深圳</option>
</select>
```

#### 文本域
```html
<label>简介：</label>
<textarea placeholder="请输入个人简介"></textarea>
```

### 表单控件详解

| 控件 | 标签 | 说明 |
| --- | --- | --- |
| 单行输入框 | `<input type="text">` | 输入单行文本 |
| 密码框 | `<input type="password">` | 输入密码（显示为圆点） |
| 单选按钮 | `<input type="radio">` | 同一组 `name` 只能选一个 |
| 复选框 | `<input type="checkbox">` | 可以选中多个 |
| 下拉框 | `<select>` | 下拉选择 |
| 文本域 | `<textarea>` | 输入多行文本 |
| 提交按钮 | `<button type="submit">` | 提交表单 |
| 重置按钮 | `<button type="reset">` | 清空表单内容 |

### 表单的 `name` 属性

**重要：**表单控件必须有 `name` 属性，提交时才会发送给服务器！

```html
<!-- 正确：有 name 属性 -->
<input type="text" name="username">

<!-- 错误：没有 name 属性，不会提交 -->
<input type="text">
```

### 完整的表单示例

```html
<form action="/submit" method="POST">
    <fieldset>
        <legend>用户注册</legend>
        <p>
            <label for="username">用户名：</label>
            <input type="text" id="username" name="username" required>
        </p>
        <p>
            <label for="email">邮箱：</label>
            <input type="email" id="email" name="email" required>
        </p>
        <p>
            <button type="submit">注册</button>
            <button type="reset">重置</button>
        </p>
    </fieldset>
</form>
```

---

## 总结

本教程涵盖了 HTML 的基础知识：

| 章节 | 知识点 |
| --- | --- |
| 第一部分 | HTML 简介、基本结构 |
| 第二部分 | 文本标签（标题、段落、列表、链接、图片） |
| 第三部分 | HTML 属性（`id`、`class`、`style`、`title` 等） |
| 第四部分 | 块元素与行内元素的区别 |
| 第五部分 | 表单元素（输入框、按钮、下拉框等） |

**下一步学习：**学习 CSS 来美化页面或 HTML 进阶
