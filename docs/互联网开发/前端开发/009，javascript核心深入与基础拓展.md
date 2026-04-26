# JavaScript核心深入与基础拓展

本章将深入探讨JavaScript的核心机制，包括作用域、闭包、this指向、原型链、执行上下文、事件循环等关键概念，以及DOM操作和BOM相关的实用知识。

---

## 一、核心概念深入

### 1.1 作用域与闭包

#### 词法作用域

#### 核心概念：作用域

**什么是作用域？**

作用域是变量和函数的可访问范围。它决定了代码中变量在哪里可以被访问，在哪里不可见。JavaScript采用**词法作用域**（也叫静态作用域），意味着函数的作用域在函数定义时就确定了，而不是在调用时确定。

**作用域的类型：**

| 类型 | 说明 | 示例 |
| --- | --- | --- |
| 全局作用域 | 在任何地方都能访问的变量 | 在顶层代码中声明的变量 |
| 函数作用域 | 只在函数内部可访问 | 函数内用var声明的变量 |
| 块级作用域 | 只在代码块内可访问（ES6） | let、const在{}内声明的变量 |

#### 词法作用域示例：

let globalVar = "全局变量";
function outer() {
let outerVar = "外部变量";
function inner() {
let innerVar = "内部变量";
// 可以访问所有外层作用域的变量
console.log(innerVar); // "内部变量"
console.log(outerVar); // "外部变量"
console.log(globalVar); // "全局变量"
}
inner();
// console.log(innerVar); // 报错！innerVar不在outer的作用域内
}
outer();

#### 作用域链：

当访问一个变量时，JavaScript引擎会沿着作用域链向外查找，直到找到该变量或到达全局作用域。

let name = "全局";
function fn1() {
let name = "fn1";
function fn2() {
let name = "fn2";
console.log(name); // "fn2" - 就近原则
}
fn2();
}
fn1();

#### 闭包的定义与应用场景

#### 核心概念：闭包

**什么是闭包？**

闭包是指有权访问另一个函数作用域中变量的函数。简单来说，当一个函数"记住"并可以访问它被创建时所在的词法作用域时，就产生了闭包。即使该函数在其词法作用域之外执行，它仍然可以访问那个作用域中的变量。

**闭包的形成条件：**

存在函数嵌套
内部函数引用了外部函数的变量
内部函数被传递到外部调用

#### 闭包的基本示例：

function createCounter() {
let count = 0; // 私有变量
return {
increment: function() {
count++;
return count;
},
decrement: function() {
count--;
return count;
},
getCount: function() {
return count;
}
};
}
const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount()); // 2
// count变量被"记住"了，形成了闭包

运行示例

#### 闭包的应用场景：

**1. 数据私有化（封装）**

function createPerson(name) {
let \_age = 0; // 私有变量，外部无法直接访问
return {
getName: () => name,
getAge: () => \_age,
setAge: (age) => {
if (age > 0 && age < 150) {
\_age = age;
}
}
};
}
const person = createPerson("张三");
person.setAge(25);
console.log(person.getAge()); // 25
// person.\_age 无法直接访问

**2. 函数工厂**

function createMultiplier(multiplier) {
return function(number) {
return number \* multiplier;
};
}
const double = createMultiplier(2);
const triple = createMultiplier(3);
console.log(double(5)); // 10
console.log(triple(5)); // 15

**3. 回调函数与事件处理**

function setupButtons() {
for (let i = 1; i <= 3; i++) {
// let形成了块级作用域，每次循环都是新的i
setTimeout(() => {
console.log("按钮" + i);
}, i \* 1000);
}
}
// 输出：按钮1、按钮2、按钮3（每秒一个）

**闭包的注意事项：**

闭包会持有对外部变量的引用，可能导致内存泄漏
在循环中使用闭包时，注意var和let的区别
及时释放不再需要的闭包引用

---

### 1.2 this 指向

#### 核心概念：this

**什么是this？**

this是函数执行时的上下文对象，它的值取决于函数是如何被调用的，而不是在哪里定义的。理解this的绑定规则是掌握JavaScript的关键之一。

#### 默认绑定、隐式绑定、显式绑定

**this绑定的四种规则：**

| 绑定类型 | 触发场景 | this指向 |
| --- | --- | --- |
| 默认绑定 | 独立调用函数 | 全局对象（严格模式下是undefined） |
| 隐式绑定 | 作为对象的方法调用 | 调用该方法的对象 |
| 显式绑定 | 使用call/apply/bind | 指定的对象 |
| new绑定 | 使用new调用构造函数 | 新创建的对象 |

#### 默认绑定：

function showThis() {
console.log(this);
}
showThis(); // window（浏览器环境）
// 严格模式下
"use strict";
function showThisStrict() {
console.log(this);
}
showThisStrict(); // undefined

#### 隐式绑定：

const person = {
name: "张三",
greet: function() {
console.log("你好，我是" + this.name);
}
};
person.greet(); // "你好，我是张三"
// 隐式丢失问题
const greet = person.greet;
greet(); // "你好，我是undefined" - 丢失了this

#### 显式绑定（call、apply、bind）：

const person1 = { name: "张三" };
const person2 = { name: "李四" };
function greet(greeting, punctuation) {
console.log(greeting + "，我是" + this.name + punctuation);
}
// call - 立即调用，参数逐个传递
greet.call(person1, "你好", "！"); // "你好，我是张三！"
// apply - 立即调用，参数以数组形式传递
greet.apply(person2, ["欢迎", "~"]); // "欢迎，我是李四~"
// bind - 返回新函数，不立即调用
const greetZhang = greet.bind(person1, "早安");
greetZhang("！"); // "早安，我是张三！"

| 方法 | 执行时机 | 参数形式 | 返回值 |
| --- | --- | --- | --- |
| call() | 立即执行 | 逐个传递 | 函数返回值 |
| apply() | 立即执行 | 数组形式 | 函数返回值 |
| bind() | 返回新函数 | 逐个传递 | 新函数 |

运行示例

#### 箭头函数的 this

箭头函数没有自己的this，它会捕获定义时外层作用域的this值（词法this）。

const person = {
name: "张三",
// 普通函数
greetRegular: function() {
console.log("普通函数：" + this.name);
},
// 箭头函数 - this继承自外层
greetArrow: () => {
console.log("箭头函数：" + this.name); // this指向全局
},
// 正确用法：在普通函数内使用箭头函数
greetCorrect: function() {
const inner = () => {
console.log("正确用法：" + this.name);
};
inner();
}
};
person.greetRegular(); // "普通函数：张三"
person.greetArrow(); // "箭头函数：undefined"
person.greetCorrect(); // "正确用法：张三"

**箭头函数this的最佳实践：**

不要用箭头函数定义对象的方法
在回调函数中使用箭头函数可以保持外层this
箭头函数不能用作构造函数（不能用new调用）

---

### 1.3 原型与原型链

#### 核心概念：原型

**什么是原型？**

原型是JavaScript实现继承的机制。每个JavaScript对象在创建时会关联另一个对象，这个对象就是它的原型。当我们访问一个对象的属性时，如果该对象本身没有这个属性，JavaScript引擎会沿着原型链向上查找。

#### \_\_proto\_\_ 与 prototype

**关键概念：**

| 属性 | 所属 | 作用 |
| --- | --- | --- |
| prototype | 构造函数 | 定义由该构造函数创建的实例的共享属性和方法 |
| \_\_proto\_\_ | 对象实例 | 指向创建该对象的构造函数的prototype |
| constructor | prototype对象 | 指向关联的构造函数 |

// 构造函数
function Person(name, age) {
this.name = name;
this.age = age;
}
// 在原型上添加方法（所有实例共享）
Person.prototype.sayHello = function() {
console.log(`你好，我是${this.name}`);
};
// 创建实例
const person1 = new Person("张三", 25);
const person2 = new Person("李四", 30);
person1.sayHello(); // "你好，我是张三"
person2.sayHello(); // "你好，我是李四"
// 原型链关系
console.log(person1.\_\_proto\_\_ === Person.prototype); // true
console.log(Person.prototype.constructor === Person); // true
console.log(person1 instanceof Person); // true

运行示例

#### 原型链示意图：

person1实例

→

Person.prototype

→

Object.prototype

→

null

// 原型链查找
console.log(person1.name); // "张三" - 自身属性
console.log(person1.sayHello); // function - Person.prototype
console.log(person1.toString); // function - Object.prototype
console.log(person1.xxx); // undefined - 找不到

#### 继承机制

**ES5原型链继承：**

// 父类
function Animal(name) {
this.name = name;
}
Animal.prototype.eat = function() {
console.log(this.name + "正在吃东西");
};
// 子类
function Dog(name, breed) {
Animal.call(this, name); // 继承属性
this.breed = breed;
}
// 继承方法
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;
Dog.prototype.bark = function() {
console.log(this.name + "正在汪汪叫");
};
const dog = new Dog("旺财", "金毛");
dog.eat(); // "旺财正在吃东西"
dog.bark(); // "旺财正在汪汪叫"

**ES6 class继承（语法糖）：**

class Animal {
constructor(name) {
this.name = name;
}
eat() {
console.log(this.name + "正在吃东西");
}
}
class Dog extends Animal {
constructor(name, breed) {
super(name); // 调用父类构造函数
this.breed = breed;
}
bark() {
console.log(this.name + "正在汪汪叫");
}
}
const dog = new Dog("旺财", "金毛");
dog.eat(); // "旺财正在吃东西"
dog.bark(); // "旺财正在汪汪叫"

---

### 1.4 执行上下文与调用栈

#### 核心概念：执行上下文

**什么是执行上下文？**

执行上下文是JavaScript代码执行时的环境抽象概念。每当JavaScript引擎执行代码时，都会创建一个执行上下文。执行上下文决定了代码可以访问哪些变量、函数和对象。

#### 变量提升

在代码执行前，JavaScript引擎会先进行变量提升：将变量声明和函数声明提升到作用域顶部。

// 变量提升示例
console.log(a); // undefined（不是报错！）
var a = 10;
// 等价于：
// var a;
// console.log(a);
// a = 10;
// 函数提升（整个函数体都提升）
foo(); // "hello" - 可以调用
function foo() {
console.log("hello");
}
// 函数表达式不会提升
// bar(); // 报错！
var bar = function() {
console.log("world");
};

**let和const不会提升（暂时性死区）：**

// 暂时性死区（Temporal Dead Zone）
// console.log(b); // 报错！Cannot access 'b' before initialization
let b = 20;
console.log(b); // 20

#### 执行上下文创建阶段

**执行上下文的生命周期：**

创建阶段

→

执行阶段

→

回收阶段

**创建阶段完成的工作：**

1. 创建变量对象（Variable Object）
    - 收集var声明的变量（初始化为undefined）
    - 收集函数声明
    - 收集let/const声明（uninitialized）
2. 建立作用域链
3. 确定this的值

function test(a, b) {
var c = 10;
function d() {}
var e = function() {};
}
// 执行上下文创建阶段（伪代码）：
// testExecutionContext = {
// variableObject: {
// arguments: { 0: a, 1: b, length: 2 },
// a: undefined,
// b: undefined,
// c: undefined,
// d: function d() {}, // 函数声明完整提升
// e: undefined
// },
// scopeChain: [...],
// this: window
// }

#### 调用栈：

调用栈是JavaScript引擎追踪函数执行状态的栈结构。每当函数被调用，就会创建一个新的执行上下文并压入栈；函数执行完毕后，执行上下文从栈中弹出。

function first() {
console.log("first开始");
second();
console.log("first结束");
}
function second() {
console.log("second开始");
third();
console.log("second结束");
}
function third() {
console.log("third执行");
}
first();
// 调用栈变化：
// 1. [global]
// 2. [global, first]
// 3. [global, first, second]
// 4. [global, first, second, third]
// 5. [global, first, second]
// 6. [global, first]
// 7. [global]
// 输出：
// "first开始"
// "second开始"
// "third执行"
// "second结束"
// "first结束"

运行示例

---

### 1.5 事件循环（初步）

#### 核心概念：事件循环

**什么是事件循环？**

JavaScript是单线程语言，意味着同一时间只能执行一个任务。事件循环是JavaScript实现异步编程的核心机制，它允许JavaScript在等待某些操作完成时（如网络请求、定时器），继续执行其他代码。

#### 同步与异步

**同步代码：**按顺序执行，前一个任务完成后才执行下一个。

**异步代码：**不会阻塞后续代码执行，在将来某个时间点执行。

console.log("1. 开始");
setTimeout(() => {
console.log("2. 定时器回调");
}, 0);
console.log("3. 结束");
// 输出顺序：
// 1. 开始
// 3. 结束
// 2. 定时器回调

运行示例

#### 宏任务与微任务概念

**任务队列分类：**

| 类型 | 包含 | 执行时机 |
| --- | --- | --- |
| **宏任务（Macro Task）** | setTimeout、setInterval、setImmediate、I/O、UI渲染 | 每次事件循环执行一个 |
| **微任务（Micro Task）** | Promise.then/catch/finally、process.nextTick、MutationObserver | 在当前宏任务结束后立即执行所有微任务 |

console.log("1. 脚本开始");
setTimeout(() => {
console.log("2. setTimeout"); // 宏任务
}, 0);
Promise.resolve()
.then(() => {
console.log("3. Promise 1"); // 微任务
})
.then(() => {
console.log("4. Promise 2"); // 微任务
});
console.log("5. 脚本结束");
// 输出顺序：
// 1. 脚本开始
// 5. 脚本结束
// 3. Promise 1
// 4. Promise 2
// 2. setTimeout

运行示例

#### 事件循环流程图：

执行同步代码

→

检查微任务队列

→

执行所有微任务

→

UI渲染

→

执行一个宏任务

→
循环...

---

### 1.6 严格模式

**"use strict" 的作用与限制：**

严格模式通过在脚本或函数开头添加 "use strict" 来启用，它可以帮助你写出更安全、更规范的代码。

| 限制项 | 说明 | 示例 |
| --- | --- | --- |
| 禁止意外创建全局变量 | 未声明的变量不能赋值 | `a = 10; // 报错` |
| 禁止this指向全局对象 | 独立调用的函数中this为undefined | `function f() { console.log(this); }` |
| 禁止重复参数名 | 函数参数不能重名 | `function f(a, a) {} // 报错` |
| 禁止删除不可删除的属性 | delete不能删除变量、函数等 | `delete Object.prototype; // 报错` |
| 禁止八进制字面量 | 不能使用0开头的八进制数 | `var n = 010; // 报错` |
| 禁止with语句 | with语句会使代码难以优化 | `with(obj) {} // 报错` |
| 保留字限制 | 不能使用未来可能的关键字 | `var let = 10; // 报错` |

"use strict";
// 1. 变量必须先声明
// a = 10; // ReferenceError: a is not defined
// 2. 不能删除变量
let x = 10;
// delete x; // SyntaxError
// 3. 不能重名参数
// function f(a, a) {} // SyntaxError
// 4. this不会指向全局
function showThis() {
console.log(this); // undefined
}
showThis();
// 5. 只读属性不能修改
const obj = {};
Object.defineProperty(obj, "name", { value: "张三", writable: false });
// obj.name = "李四"; // TypeError

**严格模式的优点：**

消除JavaScript中一些不安全、不合理的语法
提高编译器效率，增加运行速度
为未来新版本的JavaScript做铺垫
帮助发现潜在的错误

---

## 二、DOM 操作与 BOM

### 2.1 DOM 基础

#### 核心概念：DOM

**什么是DOM？**

DOM（Document Object Model，文档对象模型）是HTML文档的编程接口。它将HTML文档表示为一个节点树，JavaScript可以通过DOM API来访问和操作文档的内容、结构和样式。

#### 节点树与节点类型

**DOM节点类型：**

| 节点类型 | nodeType值 | 说明 |
| --- | --- | --- |
| 元素节点 | 1 | HTML元素，如 <div>、<p> |
| 属性节点 | 2 | 元素的属性，如 class、id |
| 文本节点 | 3 | 元素中的文本内容 |
| 注释节点 | 8 | HTML注释 |
| 文档节点 | 9 | 整个文档（document） |

// 获取节点类型
const div = document.querySelector("div");
console.log(div.nodeType); // 1（元素节点）
console.log(div.nodeName); // "DIV"

#### 获取元素

**常用获取元素的方法：**

// 通过ID获取（返回单个元素）
const elemById = document.getElementById("myId");
// 通过类名获取（返回HTMLCollection）
const elemsByClass = document.getElementsByClassName("myClass");
// 通过标签名获取（返回HTMLCollection）
const elemsByTag = document.getElementsByTagName("div");
// 通过选择器获取（返回第一个匹配元素）
const elem = document.querySelector(".myClass");
const elem2 = document.querySelector("#myId");
const elem3 = document.querySelector("div.container > p");
// 通过选择器获取所有匹配元素（返回NodeList）
const elems = document.querySelectorAll(".myClass");

| 方法 | 返回类型 | 特点 |
| --- | --- | --- |
| getElementById | Element | 最快，只能用ID |
| getElementsByClassName | HTMLCollection（动态） | 实时更新 |
| getElementsByTagName | HTMLCollection（动态） | 实时更新 |
| querySelector | Element | CSS选择器，返回第一个 |
| querySelectorAll | NodeList（静态） | CSS选择器，返回所有 |

#### 遍历节点

const elem = document.querySelector(".container");
// 父节点
elem.parentNode; // 父节点（任何类型）
elem.parentElement; // 父元素节点
// 子节点
elem.childNodes; // 所有子节点（包含文本、注释等）
elem.children; // 所有子元素节点
elem.firstChild; // 第一个子节点
elem.firstElementChild; // 第一个子元素
elem.lastChild; // 最后一个子节点
elem.lastElementChild; // 最后一个子元素
// 兄弟节点
elem.previousSibling; // 前一个兄弟节点
elem.previousElementSibling; // 前一个兄弟元素
elem.nextSibling; // 后一个兄弟节点
elem.nextElementSibling; // 后一个兄弟元素

---

### 2.2 操作 DOM

#### 修改内容

const elem = document.getElementById("content");
// innerHTML - 获取或设置HTML内容
console.log(elem.innerHTML);
elem.innerHTML = "<strong>新内容</strong>";
// textContent - 获取或设置纯文本内容
console.log(elem.textContent);
elem.textContent = "纯文本内容"; // 会转义HTML标签

**innerHTML vs textContent：**

**innerHTML**：解析HTML标签，有XSS风险，性能较低
**textContent**：纯文本，更安全，性能更好

#### 修改属性

const img = document.querySelector("img");
// 获取属性
img.getAttribute("src"); // 获取src属性
img.src; // 直接访问属性（推荐）
// 设置属性
img.setAttribute("src", "new.jpg");
img.src = "new.jpg"; // 直接设置（推荐）
// 判断是否有属性
img.hasAttribute("alt"); // true/false
// 移除属性
img.removeAttribute("title");
// 自定义属性（data-\*）
img.dataset.id; // 获取data-id属性
img.dataset.userName; // 获取data-user-name属性（驼峰命名）
img.dataset.id = "123"; // 设置data-id

#### 修改样式

const box = document.getElementById("box");
// 通过style属性修改（行内样式）
box.style.width = "200px";
box.style.backgroundColor = "red"; // 驼峰命名
box.style.cssText = "width: 200px; height: 100px;";
// 通过classList操作类名（推荐）
box.classList.add("active"); // 添加类
box.classList.remove("active"); // 移除类
box.classList.toggle("active"); // 切换类
box.classList.contains("active"); // 是否包含类
box.classList.replace("old", "new"); // 替换类
// 通过className（会覆盖所有类）
box.className = "box active highlight";

| 方法 | 适用场景 |
| --- | --- |
| style属性 | 动态计算样式、少量样式修改 |
| classList | 切换状态、添加/移除类（推荐） |
| className | 完全替换所有类名 |

#### 创建、插入、删除元素

// 创建元素
const div = document.createElement("div");
div.id = "newDiv";
div.textContent = "新创建的div";
// 创建文本节点
const text = document.createTextNode("文本内容");
// 插入元素
const container = document.getElementById("container");
container.appendChild(div); // 添加到末尾
container.insertBefore(div, container.firstChild); // 添加到开头
// 新增方法（更直观）
container.append(div); // 末尾添加（支持多个参数）
container.prepend(div); // 开头添加
container.after(div); // 在元素后面添加
container.before(div); // 在元素前面添加
// 删除元素
container.removeChild(div); // 传统方法
div.remove(); // 新方法（推荐）
// 克隆元素
const clone = div.cloneNode(); // 浅克隆（只克隆元素本身）
const cloneDeep = div.cloneNode(true); // 深克隆（包含子元素）

---

### 2.3 事件处理

#### 核心概念：事件

**什么是事件？**

事件是用户或浏览器执行的某种动作，如点击、鼠标移动、键盘按键等。JavaScript可以通过事件监听器来响应这些动作，实现交互功能。

#### 事件监听

const btn = document.getElementById("myBtn");
// 方式一：addEventListener（推荐）
btn.addEventListener("click", function(event) {
console.log("按钮被点击了");
console.log(event); // 事件对象
});
// 方式二：onclick属性（不推荐）
btn.onclick = function() {
console.log("点击");
};
// 移除事件监听
function handleClick() {
console.log("点击");
}
btn.addEventListener("click", handleClick);
btn.removeEventListener("click", handleClick);

| 方式 | 优点 | 缺点 |
| --- | --- | --- |
| addEventListener | 可添加多个监听器、可移除、支持事件捕获 | 语法稍复杂 |
| onclick | 语法简单 | 只能绑定一个、无法移除、不支持捕获 |

#### 事件对象

btn.addEventListener("click", function(e) {
e.target; // 触发事件的元素
e.currentTarget; // 绑定事件的元素
e.type; // 事件类型
e.timeStamp; // 事件发生时间
e.clientX; // 鼠标X坐标（视口）
e.clientY; // 鼠标Y坐标（视口）
e.pageX; // 鼠标X坐标（文档）
e.pageY; // 鼠标Y坐标（文档）
e.key; // 按下的键
e.keyCode; // 键码（已废弃）
});

#### 事件流（捕获、冒泡）

#### 事件传播的三个阶段

1. **捕获阶段**：事件从window向下传播到目标元素
2. **目标阶段**：事件到达目标元素
3. **冒泡阶段**：事件从目标元素向上传播到window

// addEventListener的第三个参数
elem.addEventListener("click", handler, false); // 冒泡阶段触发（默认）
elem.addEventListener("click", handler, true); // 捕获阶段触发

外层div（点击查看事件流）

中层div

内层div

演示事件流
清除日志

#### 事件委托

#### 什么是事件委托？

事件委托是利用事件冒泡机制，将事件监听器绑定在父元素上，通过判断event.target来处理子元素的事件。这样可以减少事件监听器的数量，提高性能。

// 不使用事件委托（为每个li添加监听器）
document.querySelectorAll("li").forEach(li => {
li.addEventListener("click", handler);
});
// 使用事件委托（只在父元素添加一个监听器）
document.querySelector("ul").addEventListener("click", function(e) {
if (e.target.tagName === "LI") {
console.log("点击了：" + e.target.textContent);
}
});

**事件委托的优点：**

减少内存消耗（只需一个监听器）
动态添加的元素也能响应事件
代码更简洁

#### 阻止默认行为与传播

elem.addEventListener("click", function(e) {
e.preventDefault(); // 阻止默认行为（如链接跳转、表单提交）
e.stopPropagation(); // 阻止事件冒泡
e.stopImmediatePropagation(); // 阻止事件冒泡，并阻止当前元素的其他监听器
});
// 示例：阻止链接跳转
document.querySelector("a").addEventListener("click", function(e) {
e.preventDefault(); // 阻止链接跳转
console.log("链接被点击，但不会跳转");
});

---

### 2.4 BOM（浏览器对象模型）

#### 核心概念：BOM

**什么是BOM？**

BOM（Browser Object Model，浏览器对象模型）提供了与浏览器窗口交互的对象和方法。BOM的核心是window对象，它既是全局对象，也是浏览器窗口的接口。

#### window 对象

// window是全局对象，所有全局变量都是它的属性
var globalVar = "全局变量";
console.log(window.globalVar); // "全局变量"
// 定时器
const timerId = setTimeout(() => {
console.log("延迟执行");
}, 1000);
clearTimeout(timerId); // 取消定时器
const intervalId = setInterval(() => {
console.log("重复执行");
}, 1000);
clearInterval(intervalId); // 取消定时器
// 对话框
alert("提示信息"); // 警告框
const result = confirm("确定吗？"); // 确认框（返回true/false）
const name = prompt("请输入姓名："); // 输入框（返回输入内容或null）
// 窗口操作
window.open("https://example.com"); // 打开新窗口
window.close(); // 关闭窗口
window.scrollTo(0, 0); // 滚动到指定位置
window.print(); // 打印

#### navigator 对象

// 用户代理信息
navigator.userAgent; // 浏览器信息字符串
navigator.language; // 浏览器语言
navigator.platform; // 操作系统平台
navigator.onLine; // 是否在线
navigator.cookieEnabled; // 是否启用cookie
// 地理位置API
navigator.geolocation.getCurrentPosition(function(position) {
console.log("纬度：" + position.coords.latitude);
console.log("经度：" + position.coords.longitude);
});

#### location 对象

// URL信息
location.href; // 完整URL
location.protocol; // 协议（http:或https:）
location.host; // 主机名和端口
location.hostname; // 主机名
location.port; // 端口
location.pathname; // 路径
location.search; // 查询字符串（?后面的内容）
location.hash; // 锚点（#后面的内容）
// URL操作
location.href = "https://example.com"; // 跳转（有历史记录）
location.assign("https://example.com"); // 同上
location.replace("https://example.com"); // 跳转（无历史记录）
location.reload(); // 刷新页面
location.reload(true); // 强制刷新（清除缓存）

#### history 对象

history.length; // 历史记录数量
history.back(); // 后退
history.forward(); // 前进
history.go(-1); // 后退一页
history.go(1); // 前进一页
// HTML5新增：pushState和replaceState
history.pushState({page: 1}, "title", "/page1");
history.replaceState({page: 2}, "title", "/page2");
// 监听popstate事件
window.addEventListener("popstate", function(e) {
console.log(e.state);
});

#### screen 对象

screen.width; // 屏幕宽度
screen.height; // 屏幕高度
screen.availWidth; // 可用屏幕宽度
screen.availHeight; // 可用屏幕高度
screen.colorDepth; // 颜色深度
screen.pixelDepth; // 像素深度

#### 本地存储

#### 本地存储对比

| 特性 | localStorage | sessionStorage | cookie |
| --- | --- | --- | --- |
| 存储大小 | 约5MB | 约5MB | 约4KB |
| 生命周期 | 永久存储 | 会话期间（关闭标签页清除） | 可设置过期时间 |
| 发送到服务器 | 否 | 否 | 是（每次请求都发送） |
| 作用域 | 同源窗口共享 | 仅当前标签页 | 同源窗口共享 |

// localStorage 和 sessionStorage API相同
localStorage.setItem("name", "张三");
localStorage.getItem("name"); // "张三"
localStorage.removeItem("name");
localStorage.clear(); // 清空所有
// 存储对象需要序列化
const user = { name: "张三", age: 25 };
localStorage.setItem("user", JSON.stringify(user));
const savedUser = JSON.parse(localStorage.getItem("user"));
// cookie操作
document.cookie = "name=张三; expires=Fri, 31 Dec 2025 23:59:59 GMT; path=/";
document.cookie; // 获取所有cookie

---

## 三、本章小结

#### 核心知识点回顾：

**核心概念深入：**

**作用域与闭包**：词法作用域决定了变量的可访问范围，闭包是函数"记住"其词法作用域的能力
**this指向**：默认绑定、隐式绑定、显式绑定、new绑定四种规则，箭头函数没有自己的this
**原型与原型链**：\_\_proto\_\_指向原型对象，prototype是构造函数的属性，原型链实现继承
**执行上下文**：变量提升、执行上下文创建阶段、调用栈管理函数调用
**事件循环**：同步任务在主线程执行，异步任务通过事件循环处理，宏任务和微任务有不同优先级
**严格模式**：消除不合理语法、提高性能、为未来版本铺路

**DOM操作与BOM：**

**DOM基础**：节点树结构、获取元素方法、遍历节点
**操作DOM**：修改内容、属性、样式，创建/插入/删除元素
**事件处理**：事件监听、事件对象、事件流、事件委托
**BOM**：window、navigator、location、history、screen对象，本地存储

**学习建议：**

1. 深入理解JavaScript的核心机制，这些是框架和高级特性的基础
2. 多动手实践DOM操作，这是前端开发的基本功
3. 理解事件机制对于构建交互式应用至关重要
4. 闭包和原型链是面试高频考点，务必掌握

---