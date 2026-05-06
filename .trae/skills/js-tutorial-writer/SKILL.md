---
name: "js-tutorial-writer"
description: "创建带有医疗主题示例的 JavaScript 教程 HTML 文件。当用户想要创建或更新涵盖异步编程、错误处理或调试主题的 JavaScript 教程内容时调用。"
---

# JavaScript 教程编写器

此技能用于创建具有医疗主题的综合性 JavaScript 教程 HTML 文件，遵循特定的格式约定。

## 文件结构

每个教程文件应遵循以下结构：
1. HTML5 文档类型声明，设置中文语言
2. 内嵌 CSS 样式（所有文件保持一致）
3. 使用 h2/h3/h4 标题的结构化内容
4. 带有语法高亮 span 的代码示例
5. 带有按钮的交互式演示区域
6. 用于演示的 JavaScript 函数

## 样式约定

### CSS 类名
- `.demo-box` - 白色背景的代码示例容器
- `.code` - 深色背景的代码块，带语法高亮
- `.knowledge-box` - 蓝色渐变的核心概念框
- `.tip` - 黄色背景的提示框
- `.warning` - 红色背景的警告框
- `.output-box` - 深色终端风格的输出显示
- `.flow-container` / `.flow-step` - 流程图容器

### 代码语法高亮
使用以下 span 类名进行代码着色：
- `.code-keyword` - 关键字（const、let、function、async 等）
- `.code-string` - 字符串字面量
- `.code-number` - 数值
- `.code-comment` - 注释
- `.code-function` - 函数名
- `.code-variable` - 变量名
- `.code-class` - 类名

## 内容准则

### 禁止使用表情符号
内容中不要包含任何表情符号，保持文本专业。

### 换行方式
在段落内的列表项使用 `<br>` 标签，而不是 `<li>`。

### 医疗主题
所有示例应与医疗/健康场景相关：
- 患者数据管理
- 病历记录
- 处方管理
- 医院系统
- 医生/患者交互

### 示例模式
```html
<div class="demo-box">
<p><strong>主题标题：</strong></p>
<div class="code">
<span class="code-comment">// 医疗系统示例</span>
<span class="code-keyword">function</span> <span class="code-function">getPatient</span>(<span class="code-variable">id</span>) {
    <span class="code-keyword">return</span> <span class="code-keyword">new</span> <span class="code-class">Promise</span>(<span class="code-variable">resolve</span> => {
        <span class="code-function">setTimeout</span>(() => <span class="code-function">resolve</span>({ <span class="code-variable">id</span>, <span class="code-variable">name</span>: <span class="code-string">"张三"</span> }), <span class="code-number">500</span>);
    });
}
</div>
</div>
```

## 知识问答模块

在教程中穿插常见问题解答，使用 `.knowledge-box` 样式，用简明易懂的语言解释概念。示例应通用化，不针对特定群体。

### 问答格式
```html
<div class="knowledge-box">
<h4>问题：什么是回调函数？</h4>
<p><strong>解答：</strong></p>
<p>
    回调函数就是作为参数传递给另一个函数的函数。当某个操作完成后，这个函数会被"回调"执行。<br>
    生活中的例子：你去餐厅点餐，留下电话号码。餐做好了，餐厅打电话通知你。这个电话号码就是"回调函数"。<br>
    代码中的例子：setTimeout 的第一个参数就是回调函数，定时器结束后会被调用。
</p>
</div>
```

### 问答放置位置

根据问题主题，放置在对应章节之后：

### 演示函数模式
```html
<div class="render" id="demo-name">
    <button onclick="demoFunction()">演示按钮</button>
    <div id="demo-output" class="output-box"></div>
</div>

<script>
function demoFunction() {
    const output = document.getElementById('demo-output');
    output.innerHTML = '结果:\n';
    // 演示逻辑在这里
}
</script>
```

## 需要避免的常见问题

1. **ID 不匹配**：确保 HTML 元素 id 与 JavaScript 中完全一致
   - 错误：`id="event-loop-output"` 配合 `getElementById('eventloop-output')`
   - 正确：两者使用相同的字符串

2. **缺少闭合标签**：始终正确闭合所有 HTML 元素

3. **脚本位置**：将 `<script>` 放在 `</body>` 之前

4. **异步输出**：记住异步操作在函数返回后才完成

## 文件命名约定

文件应使用数字和中文描述命名：
- `008，javascript基础.html`
- `009，javascript核心深入与基础拓展.html`
- `010，javascript与ES6新特性.html`
- `011，javascript中的异步编程与错误调试.html`

## 总结部分

每个文件应以知识框结束，总结关键要点：

```html
<div class="knowledge-box">
<h4>本章小结</h4>
<p><strong>要点一：</strong></p>
<p>
    要点内容<br>
    使用br换行
</p>
</div>
```
