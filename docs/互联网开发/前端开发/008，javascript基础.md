# JavaScript基础

JavaScript是一种动态类型、基于原型的脚本语言，是Web开发的核心技术之一。它让网页"活"起来，实现各种交互效果。

---

## 一、JavaScript的应用场景

JavaScript的应用范围非常广泛，远不止网页开发：

#### 网页交互

表单验证、动态内容、动画效果

#### 移动应用

React Native、Ionic跨平台开发

#### 桌面应用

Electron开发桌面软件

#### 服务端

Node.js后端开发

#### 生活中的JavaScript例子：

**微信小程序** - 使用JavaScript编写逻辑
**在线文档** - 腾讯文档、石墨文档的实时协作
**网页游戏** - 各种H5小游戏
**数据可视化** - ECharts图表库

在医院中，医院信息系统(HIS)、电子病历系统、在线问诊平台的前端交互，都离不开JavaScript

你可以理解为，网页前端的各种交互行为的逻辑都由javascript实现

---

## 二、变量与数据类型

#### 前置核心概念：变量与内存

**什么是变量？**

变量是程序中用于存储和引用数据的命名容器。可以把变量想象成一个"贴了标签的盒子"，标签是变量名，盒子里装的是数据值。通过变量名，我们可以随时找到并操作盒子里的数据。

**为什么变量要声明？**

声明变量是在告诉计算机："请为我准备一个存储空间，并给它起个名字"。声明的重要性在于：

**分配内存空间**：计算机需要知道要预留多少存储空间
**建立访问入口**：通过变量名可以找到对应的存储位置
**明确作用范围**：声明时确定变量在哪些代码区域有效
**避免命名冲突**：防止不小心覆盖其他数据

**什么是内存？**

内存是计算机用于临时存储正在运行的程序和数据的高速存储区域。程序运行时，所有的变量、函数、对象都存储在内存中。内存的特点是读写速度极快，但断电后数据会丢失。

**内存的主要分类：**

| 类型 | 特点 | 用途 |
| --- | --- | --- |
| **栈内存（Stack）** | 空间小、速度快、自动分配释放、按顺序存取 | 存储基本类型数据（数字、字符串、布尔值等）和函数调用的上下文 |
| **堆内存（Heap）** | 空间大、动态分配、需要手动或自动管理释放 | 存储引用类型数据（对象、数组、函数等复杂结构） |

**什么是数据类型？**

数据类型是对数据的分类，它定义了数据可以取哪些值、能进行哪些操作、以及占用多少存储空间。比如数字类型可以进行加减乘除，字符串类型可以拼接和截取。

**为什么数据要分类？**

**高效存储**：不同类型数据占用空间不同，分类存储更节省内存
**明确操作**：知道类型就知道能做什么操作（数字可以计算，字符串不能直接相减）
**防止错误**：类型检查可以在运行前发现潜在问题
**提高效率**：编译器/解释器可以根据类型优化代码执行

**什么是引用？**

引用是指向内存中某个对象的"地址"或"指针"。在JavaScript中，对象、数组等引用类型存储在堆内存中，变量保存的是这些数据的内存地址（引用），而不是数据本身。

// 基本类型：变量直接存储值
let a = 10;
let b = a; // b得到的是10的副本，a和b互不影响
b = 20;
console.log(a); // 10，a没有变化
// 引用类型：变量存储的是地址
let obj1 = { name: "张三" };
let obj2 = obj1; // obj2得到的是地址，指向同一个对象
obj2.name = "李四";
console.log(obj1.name); // "李四"，obj1也变了！

### 2.1 变量声明

变量是存储数据的容器。JavaScript有三种声明变量的方式：

| 关键字 | 作用域 | 是否可重新赋值 | 是否可重复声明 | 推荐程度 |
| --- | --- | --- | --- | --- |
| var | 函数作用域 | 是 | 是 | 不推荐 |
| let | 块级作用域 | 是 | 否 | 推荐 |
| const | 块级作用域 | 否 | 否 | 推荐（常量） |

#### 代码示例：

// var - 函数作用域，存在变量提升（不推荐使用）
var name = "张三";
// let - 块级作用域，可重新赋值
let age = 25;
age = 26; // 合法
// const - 块级作用域，不可重新赋值（常量）
const PI = 3.14159;
// PI = 3.14; // 报错！const不能重新赋值
// const声明对象时，属性可以修改
const user = { name: "李四" };
user.name = "王五"; // 合法，修改属性

**注意：**现代JavaScript开发中，优先使用const，需要重新赋值时使用let，尽量避免使用var。

### 2.2 基本数据类型（原始类型）

JavaScript有6种基本数据类型，它们直接存储在栈内存中：

String
字符串

Number
数字

Boolean
布尔值

Null
空值

Undefined
未定义

Symbol
符号(ES6)

#### 基本类型示例：

// 1. String 字符串 - 用引号包裹的文本
let hospital = "北京协和医院";
let department = '内科';
let intro = `欢迎来到${hospital}`; // 模板字符串（ES6）
// 2. Number 数字 - 整数和小数
let age = 30;
let temperature = 37.5;
let negative = -10;
let infinity = Infinity;
let notANumber = NaN; // Not a Number
// 3. Boolean 布尔值 - 只有 true 或 false
let isDoctor = true;
let isPatient = false;
// 4. Null - 空值，表示"无"或"空"
let emptyValue = null;
// 5. Undefined - 未定义，变量声明但未赋值
let notAssigned;
// console.log(notAssigned); // undefined
// 6. Symbol - ES6新增，表示独一无二的值
let sym = Symbol("description");

### 2.3 引用数据类型

引用类型存储在堆内存中，变量保存的是内存地址的引用：

Object
对象

Array
数组

Function
函数

#### 引用类型示例：

// 1. Object 对象 - 键值对集合
let patient = {
name: "张三",
age: 45,
gender: "男",
diagnosis: "高血压"
};
// 访问对象属性
console.log(patient.name); // "张三"
console.log(patient["age"]); // 45
// 2. Array 数组 - 有序的数据集合
let vitalSigns = [36.5, 72, 120, 80];
// 索引: 体温 心率 收缩压 舒张压
console.log(vitalSigns[0]); // 36.5（第一个元素）
console.log(vitalSigns.length); // 4（数组长度）
// 3. Function 函数 - 可执行的代码块
function greet(name) {
return "你好，" + name + "医生！";
}
console.log(greet("李")); // "你好，李医生！"

**理解引用类型：**基本类型复制的是值，引用类型复制的是引用（地址）。这就像复印文件和分享文件链接的区别。

### 2.4 typeof运算符

使用typeof可以检测数据类型：

console.log(typeof "hello"); // "string"
console.log(typeof 123); // "number"
console.log(typeof true); // "boolean"
console.log(typeof undefined); // "undefined"
console.log(typeof null); // "object" 这是一个历史遗留bug
console.log(typeof {}); // "object"
console.log(typeof []); // "object"（数组也是对象）
console.log(typeof function(){}); // "function"

---

## 三、运算符

### 3.1 算术运算符

| 运算符 | 描述 | 示例 | 结果 |
| --- | --- | --- | --- |
| + | 加法 | 5 + 3 | 8 |
| - | 减法 | 5 - 3 | 2 |
| \* | 乘法 | 5 \* 3 | 15 |
| / | 除法 | 6 / 2 | 3 |
| % | 取余（模） | 7 % 3 | 1 |
| \*\* | 幂运算（ES7） | 2 \*\* 3 | 8 |
| ++ | 自增 | let a=1; a++ | a=2 |
| -- | 自减 | let a=1; a-- | a=0 |

#### 算术运算示例：

let a = 10;
let b = 3;
console.log(a + b); // 13
console.log(a - b); // 7
console.log(a \* b); // 30
console.log(a / b); // 3.333...
console.log(a % b); // 1（10除以3余1）
console.log(a \*\* b); // 1000（10的3次方）
// 字符串拼接
console.log("体温：" + 37.5 + "℃"); // "体温：37.5℃"

运行示例

### 3.2 赋值运算符

| 运算符 | 描述 | 等价于 |
| --- | --- | --- |
| = | 赋值 | x = y |
| += | 加后赋值 | x = x + y |
| -= | 减后赋值 | x = x - y |
| \*= | 乘后赋值 | x = x \* y |
| /= | 除后赋值 | x = x / y |
| %= | 取余后赋值 | x = x % y |

let score = 80;
score += 10; // score = score + 10; 结果：90
score -= 5; // score = score - 5; 结果：85

### 3.3 比较运算符

| 运算符 | 描述 | 示例 | 结果 |
| --- | --- | --- | --- |
| == | 相等（会类型转换） | 5 == "5" | true ⚠️ |
| === | 严格相等（不转换） | 5 === "5" | false ✅ |
| != | 不相等（会类型转换） | 5 != "5" | false |
| !== | 严格不相等 | 5 !== "5" | true ✅ |
| > | 大于 | 5 > 3 | true |
| < | 小于 | 5 < 3 | false |
| >= | 大于等于 | 5 >= 5 | true |
| <= | 小于等于 | 5 <= 3 | false |

**重要：**始终使用===和!==进行严格比较，避免隐式类型转换带来的意外结果！

// == 会进行类型转换
console.log(5 == "5"); // true（字符串被转为数字）
console.log(0 == false); // true
console.log("" == false); // true
// === 严格比较，不转换类型
console.log(5 === "5"); // false（类型不同）
console.log(0 === false); // false
console.log("" === false); // false

运行示例

### 3.4 逻辑运算符

#### 💡 核心概念：逻辑门

**什么是逻辑门？**

逻辑门是数字电路的基本构建单元，用于实现布尔运算。在计算机底层，所有的计算最终都由逻辑门完成。逻辑门接收一个或多个二进制输入（0或1，对应false或true），产生一个二进制输出。

**常见逻辑门的自然语言表述：**

| 逻辑门 | 符号 | 自然语言表述 | 真值表 |
| --- | --- | --- | --- |
| **与门（AND）** | && | "两个条件都满足时，结果才成立" | 1 AND 1 = 1，其余为0 |
| **或门（OR）** | || | "至少有一个条件满足，结果就成立" | 0 OR 0 = 0，其余为1 |
| **非门（NOT）** | ! | "条件取反，真变假、假变真" | NOT 1 = 0，NOT 0 = 1 |
| **异或门（XOR）** | ^ | "两个条件不同时成立，相同时不成立" | 1 XOR 0 = 1，0 XOR 1 = 1，其余为0 |
| **与非门（NAND）** | — | "与门结果取反"，是最通用的逻辑门 | 0 NAND 0 = 1，其余与AND相反 |
| **或非门（NOR）** | — | "或门结果取反" | 1 NOR 1 = 0，其余与OR相反 |

**生活中的逻辑门例子：**

**与门**：电梯只有在门关好**且**按了楼层按钮后才会移动
**或门**：手机可以用密码**或**指纹解锁
**非门**：灯开关打开时灯亮，关闭时灯灭
**异或门**：两个开关控制一盏灯，状态不同时灯亮

| 运算符 | 描述 | 示例 |
| --- | --- | --- |
| && | 逻辑与（都为真才为真） | true && false → false |
| || | 逻辑或（有一个为真就为真） | true || false → true |
| ! | 逻辑非（取反） | !true → false |

let hasLicense = true;
let hasCar = false;
// 逻辑与：两个条件都满足
if (hasLicense && hasCar) {
console.log("可以开车");
} else {
console.log("条件不满足");
}
// 逻辑或：满足一个条件即可
if (hasLicense || hasCar) {
console.log("至少有一个条件满足");
}
// 短路求值
let name = "";
let displayName = name || "匿名用户"; // "匿名用户"

### 3.5 三元运算符

三元运算符是if...else的简写形式：

// 语法：条件 ? 真值 : 假值
let temperature = 38.5;
let status = temperature >= 37.3 ? "发热" : "正常";
console.log(status); // "发热"
// 等价于：
if (temperature >= 37.3) {
status = "发热";
} else {
status = "正常";
}

运行示例

### 3.6 位运算符

位运算符对数字的二进制位进行操作：

| 运算符 | 描述 | 示例 |
| --- | --- | --- |
| & | 按位与 | 5 & 3 → 1 |
| | | 按位或 | 5 | 3 → 7 |
| ^ | 按位异或 | 5 ^ 3 → 6 |
| ~ | 按位非 | ~5 → -6 |
| << | 左移 | 5 << 1 → 10 |
| >> | 右移 | 5 >> 1 → 2 |

// 实用技巧：使用 ~~ 快速取整
console.log(~~3.7); // 3
console.log(~~-3.7); // -3
// 使用 ^ 交换两个数
let a = 5, b = 3;
a = a ^ b;
b = a ^ b;
a = a ^ b;
// a=3, b=5

### 3.7 instanceof运算符

instanceof用于检测对象是否是某个构造函数的实例：

let arr = [1, 2, 3];
let obj = {};
console.log(arr instanceof Array); // true
console.log(arr instanceof Object); // true
console.log(obj instanceof Array); // false
// 更可靠的数组检测方法
console.log(Array.isArray(arr)); // true

---

## 四、流程控制

#### 核心概念：流程控制语句的作用

**什么是流程控制？**

流程控制是指程序根据条件决定执行哪些代码、跳过哪些代码、重复执行哪些代码的机制。默认情况下，程序从上到下顺序执行每一行代码，但流程控制语句可以改变这个执行顺序。

**各语句的作用：**

**条件判断语句：**

| 语句 | 作用 | 适用场景 |
| --- | --- | --- |
| if...else | 根据条件真假选择执行不同的代码分支 | 条件判断、范围判断、复杂逻辑判断 |
| switch | 根据表达式的值匹配多个固定选项 | 多选一的场景（如状态机、菜单选择） |

**循环语句：**

| 语句 | 作用 | 适用场景 |
| --- | --- | --- |
| for | 已知循环次数时使用，包含初始化、条件、迭代三部分 | 遍历数组、执行固定次数的操作 |
| while | 先判断条件，条件为真时重复执行 | 不确定循环次数，需要满足某条件时停止 |
| do...while | 先执行一次，再判断条件是否继续 | 至少需要执行一次的场景（如菜单选择） |
| for...of | 遍历可迭代对象的值（ES6） | 遍历数组、字符串等，直接获取元素值 |
| for...in | 遍历对象的属性名（键） | 遍历对象的所有可枚举属性 |

**跳转语句：**

| 语句 | 作用 | 使用说明 |
| --- | --- | --- |
| break | 立即跳出当前循环或switch语句 | 提前结束循环，不再执行后续迭代 |
| continue | 跳过本次循环的剩余代码，进入下一次迭代 | 只跳过当前这一次，循环继续 |
| label | 给代码块命名，配合break/continue跳转到指定位置 | 用于嵌套循环中跳出外层循环（较少使用） |

### 4.1 条件语句

#### if...else 语句

let bloodPressure = 145; // 收缩压
if (bloodPressure >= 140) {
console.log("高血压");
} else if (bloodPressure >= 120) {
console.log("正常高值");
} else if (bloodPressure >= 90) {
console.log("正常");
} else {
console.log("低血压");
}

运行示例

#### switch 语句

当有多个固定值需要判断时，使用switch更清晰：

let bloodType = "A";
switch (bloodType) {
case "A":
console.log("A型血：可输A型、O型血");
break;
case "B":
console.log("B型血：可输B型、O型血");
break;
case "AB":
console.log("AB型血：万能受血者");
break;
case "O":
console.log("O型血：万能供血者");
break;
default:
console.log("未知血型");
}

运行示例

**注意：**每个case后面要加break，否则会继续执行下一个case（称为"穿透"）。

### 4.2 循环语句

#### for 循环

// 基本for循环
for (let i = 1; i <= 5; i++) {
console.log("第" + i + "次循环");
}
// 遍历数组
let symptoms = ["发热", "咳嗽", "乏力"];
for (let i = 0; i < symptoms.length; i++) {
console.log(symptoms[i]);
}

运行示例

#### while 循环

// while：先判断后执行
let count = 0;
while (count < 3) {
console.log("count = " + count);
count++;
}

#### do...while 循环

// do...while：先执行后判断（至少执行一次）
let num = 0;
do {
console.log("num = " + num);
num++;
} while (num < 3);

#### for...of 和 for...in（ES6）

let arr = ["苹果", "香蕉", "橙子"];
// for...of：遍历值
for (let fruit of arr) {
console.log(fruit); // 苹果、香蕉、橙子
}
// for...in：遍历索引/键
for (let index in arr) {
console.log(index); // 0、1、2
}
// 遍历对象
let patient = { name: "张三", age: 30 };
for (let key in patient) {
console.log(key + ": " + patient[key]);
}

运行示例

### 4.3 break、continue 和 label

// break：跳出整个循环
for (let i = 1; i <= 10; i++) {
if (i === 5) break;
console.log(i); // 1, 2, 3, 4
}
// continue：跳过本次迭代
for (let i = 1; i <= 5; i++) {
if (i === 3) continue;
console.log(i); // 1, 2, 4, 5
}
// label：标记循环，用于跳出多层循环
outer:
for (let i = 0; i < 3; i++) {
for (let j = 0; j < 3; j++) {
if (i === 1 && j === 1) {
break outer; // 跳出外层循环
}
console.log(`i=${i}, j=${j}`);
}
}

运行示例

---

## 五、函数基础

#### 核心概念：为什么要声明函数？

**什么是函数？**

函数是一段可重复使用的代码块，用于完成特定的任务。函数可以接收输入（参数），进行处理，并返回输出（返回值）。

**为什么要声明函数？**

**代码复用**：将常用功能封装成函数，避免重复编写相同代码。只需定义一次，可以在多处调用。
**模块化设计**：将复杂问题分解为多个小函数，每个函数负责一个独立功能，使程序结构清晰。
**便于维护**：修改功能只需改函数内部，所有调用处自动生效，降低维护成本。
**提高可读性**：函数名可以描述功能意图，比一大段代码更容易理解。
**便于测试**：独立的函数可以单独测试，更容易发现和定位问题。
**作用域隔离**：函数内部定义的变量不会污染全局命名空间，避免变量冲突。

**函数的本质：**

函数在JavaScript中是一等公民，占据最重要的位置，这意味着函数可以：

赋值给变量
作为参数传递给其他函数（回调函数）
作为其他函数的返回值
存储在数据结构中

// 函数作为参数传递
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(function(n) {
return n \* 2;
});
// 函数作为返回值
function createMultiplier(factor) {
return function(number) {
return number \* factor;
};
}
const double = createMultiplier(2);
console.log(double(5)); // 10

### 5.1 函数声明与表达式

// 1. 函数声明（会被提升）
function calculateBMI(weight, height) {
return weight / (height \* height);
}
// 2. 函数表达式（不会被提升）
const calculateBMI2 = function(weight, height) {
return weight / (height \* height);
};
// 3. 箭头函数（ES6，简洁写法）
const calculateBMI3 = (weight, height) => {
return weight / (height \* height);
};
// 箭头函数简写（单行可省略大括号和return）
const square = x => x \* x;
// 使用函数
let bmi = calculateBMI(70, 1.75);
console.log("BMI: " + bmi.toFixed(2)); // BMI: 22.86

运行示例

### 5.2 参数

#### 默认参数（ES6）

function greet(name = "访客", greeting = "你好") {
return `${greeting}，${name}！`;
}
console.log(greet()); // "你好，访客！"
console.log(greet("张医生")); // "你好，张医生！"
console.log(greet("李护士", "欢迎")); // "欢迎，李护士！"

#### 剩余参数（ES6）

// 使用 ...收集剩余参数
function sum(...numbers) {
let total = 0;
for (let num of numbers) {
total += num;
}
return total;
}
console.log(sum(1, 2, 3)); // 6
console.log(sum(1, 2, 3, 4, 5)); // 15

#### arguments对象

// arguments是类数组对象，包含所有传入的参数
function showArguments() {
console.log(arguments.length); // 参数个数
console.log(arguments[0]); // 第一个参数
}
showArguments("a", "b", "c");
// 输出: 3
// 输出: "a"

**推荐：**现代JavaScript中，优先使用剩余参数...args而不是arguments，因为剩余参数是真正的数组。

### 5.3 返回值

函数使用return返回结果，没有return则返回undefined：

function checkTemperature(temp) {
if (temp >= 37.3) {
return "发热";
} else if (temp < 36.0) {
return "体温过低";
}
return "正常";
}
let result = checkTemperature(38.5);
console.log(result); // "发热"
// 函数没有return时返回undefined
function noReturn() {
console.log("我什么也不返回");
}
console.log(noReturn()); // undefined

### 5.4 作用域

#### 全局作用域

// 在任何函数外部声明的变量是全局变量
var globalVar = "我是全局变量";
let globalLet = "我也是全局变量";
function test() {
console.log(globalVar); // 可以访问
console.log(globalLet); // 可以访问
}

#### 函数作用域

function outer() {
let outerVar = "外部函数变量";
function inner() {
let innerVar = "内部函数变量";
console.log(outerVar); // 可以访问（闭包）
console.log(innerVar); // 可以访问
}
inner();
// console.log(innerVar); // 报错！无法访问
}
outer();

运行示例

#### 块级作用域（let/const）

if (true) {
var varVariable = "var声明的变量";
let letVariable = "let声明的变量";
}
console.log(varVariable); // "var声明的变量"（函数作用域，泄漏出去了）
// console.log(letVariable); // 报错！（块级作用域）
for (let i = 0; i < 3; i++) {
setTimeout(() => console.log(i), 100);
}
// 输出: 0, 1, 2（let每次循环创建新的作用域）
for (var j = 0; j < 3; j++) {
setTimeout(() => console.log(j), 100);
}
// 输出: 3, 3, 3（var共享同一个变量）

---

## 六、内置对象与常用方法

### 6.1 字符串（String）

let str = " Hello, JavaScript! ";
// 常用方法
console.log(str.length); // 22（长度）
console.log(str.trim()); // "Hello, JavaScript!"（去除首尾空格）
console.log(str.toLowerCase()); // " hello, javascript! "
console.log(str.toUpperCase()); // " HELLO, JAVASCRIPT! "
// 查找
console.log(str.indexOf("Java")); // 9（首次出现的位置）
console.log(str.includes("Java")); // true（是否包含）
// 截取
console.log(str.substring(2, 7)); // "Hello"（从索引2到7）
console.log(str.slice(-6, -1); // "ript!"（支持负索引）
// 分割与替换
let words = "苹果,香蕉,橙子".split(","); // ["苹果", "香蕉", "橙子"]
let newStr = str.replace("Java", "Type"); // "Hello, TypeScript!"

运行示例

### 6.2 数组（Array）

let arr = [1, 2, 3, 4, 5];
// 常用方法
console.log(arr.length); // 5（长度）
console.log(arr.push(6)); // 6（末尾添加，返回新长度）
console.log(arr.pop()); // 6（删除末尾元素）
console.log(arr.unshift(0)); // 6（开头添加）
console.log(arr.shift()); // 0（删除开头元素）
// 高阶方法（ES5+）
let doubled = [1, 2, 3].map(x => x \* 2); // [2, 4, 6]
let evens = [1, 2, 3, 4].filter(x => x % 2 === 0); // [2, 4]
let sum = [1, 2, 3].reduce((a, b) => a + b); // 6
let found = [1, 2, 3].find(x => x > 1); // 2

运行示例

### 6.3 数字（Number）与Math

// Number方法
console.log(parseInt("42px")); // 42（解析整数）
console.log(parseFloat("3.14")); // 3.14（解析浮点数）
console.log(isNaN(NaN)); // true
console.log((3.14159).toFixed(2)); // "3.14"
// Math对象
console.log(Math.round(4.5)); // 5（四舍五入）
console.log(Math.floor(4.9)); // 4（向下取整）
console.log(Math.ceil(4.1)); // 5（向上取整）
console.log(Math.abs(-5)); // 5（绝对值）
console.log(Math.max(1, 5, 3)); // 5（最大值）
console.log(Math.min(1, 5, 3)); // 1（最小值）
console.log(Math.random()); // 0-1之间的随机数
console.log(Math.floor(Math.random() \* 10)); // 0-9的随机整数

运行示例

### 6.4 日期（Date）

let now = new Date();
console.log(now); // 当前日期时间
// 创建指定日期
let birthday = new Date(2000, 0, 1); // 2000年1月1日
// 获取日期各部分
console.log(now.getFullYear()); // 年份
console.log(now.getMonth()); // 月份（0-11，0是1月）
console.log(now.getDate()); // 日期（1-31）
console.log(now.getDay()); // 星期（0-6，0是周日）
console.log(now.getHours()); // 小时
console.log(now.getMinutes()); // 分钟
// 格式化
console.log(now.toLocaleDateString("zh-CN")); // "2024/1/15"

---

## 七、类型转换

#### 核心概念：为什么需要类型转换？

**为什么各种变量类型之间不能直接互通而是需要转换？**

不同类型的数据在内存中的存储方式和表示形式完全不同：

**存储格式不同**：数字以二进制数值存储，字符串以字符编码序列存储，对象以引用地址存储
**操作规则不同**：数字可以加减乘除，字符串可以拼接截取，对象可以访问属性
**内存占用不同**：一个数字可能只占8字节，一个对象可能占用数百字节

因此，当需要在不同类型之间操作时，必须先统一类型，这就是类型转换的意义。

**区分出各种不同类型变量的意义：**

| 意义 | 说明 | 例子 |
| --- | --- | --- |
| **明确数据性质** | 类型告诉程序员和计算机这个数据代表什么 | 数字代表数量，字符串代表文本，布尔代表是非 |
| **确定可用操作** | 不同类型支持不同的操作方法 | 数字可以计算，数组可以遍历，对象可以存取属性 |
| **优化存储空间** | 根据类型分配恰当的内存空间 | 布尔值只需1位，大对象需要更多空间 |
| **防止运行时错误** | 类型检查可以在早期发现不合理操作 | 字符串不能直接减法，undefined不能访问属性 |
| **提高代码可读性** | 类型信息让代码意图更清晰 | 看到boolean就知道是判断条件，看到number就知道是数值 |

**类型转换的两种方式：**

**显式转换**：程序员主动调用转换函数，如 `Number("123")`，意图明确、代码清晰
**隐式转换**：JavaScript引擎自动完成，如 `"1" + 2`，方便但容易产生意外结果

**最佳实践：**推荐使用显式转换，代码意图更清晰，避免隐式转换带来的意外结果。

### 7.1 显式类型转换

// 转字符串
console.log(String(123)); // "123"
console.log((123).toString()); // "123"
console.log(123 + ""); // "123"（隐式转换）
// 转数字
console.log(Number("123")); // 123
console.log(parseInt("123.9")); // 123
console.log(parseFloat("123.9")); // 123.9
console.log("123" - 0); // 123（隐式转换）
// 转布尔值
console.log(Boolean(1)); // true
console.log(Boolean(0)); // false
console.log(Boolean("")); // false
console.log(Boolean(null)); // false
console.log(Boolean(undefined)); // false

### 7.2 隐式类型转换

// + 运算符：倾向于转字符串
console.log("1" + 2); // "12"
console.log(1 + true); // 2（true转为1）
// -、\*、/ 运算符：倾向于转数字
console.log("5" - 2); // 3
console.log("6" / "2"); // 3
// == 比较会进行类型转换
console.log(1 == "1"); // true
console.log(0 == false); // true

**最佳实践：**使用严格相等===避免隐式转换，需要转换时显式调用转换函数。

---

## 八、错误处理

try {
// 可能出错的代码
let result = JSON.parse("invalid json");
} catch (error) {
// 处理错误
console.log("出错了：" + error.message);
} finally {
// 无论是否出错都会执行
console.log("清理工作");
}
// 抛出错误
function divide(a, b) {
if (b === 0) {
throw new Error("除数不能为0");
}
return a / b;
}

---

## 九、ES6+新特性简介

### 9.1 解构赋值

// 数组解构
const [a, b, c] = [1, 2, 3];
// 对象解构
const { name, age } = { name: "张三", age: 25 };
// 默认值
const [x = 10] = [];

### 9.2 展开运算符

// 数组展开
const arr1 = [1, 2];
const arr2 = [3, 4];
const merged = [...arr1, ...arr2]; // [1, 2, 3, 4]
// 对象展开
const obj1 = { a: 1 };
const obj2 = { ...obj1, b: 2 }; // { a: 1, b: 2 }

### 9.3 模块化

// 导出（module.js）
export const PI = 3.14;
export function add(a, b) { return a + b; }
// 导入（main.js）
import { PI, add } from './module.js';

---

## 十、实战练习

#### 练习：患者信息管理系统

下面是一个简单的患者信息管理示例，综合运用了前面学习的知识：

// 患者数据
const patients = [
{ id: 1, name: "张三", age: 45, diagnosis: "高血压" },
{ id: 2, name: "李四", age: 32, diagnosis: "糖尿病" },
{ id: 3, name: "王五", age: 28, diagnosis: "感冒" }
];
// 查找患者
function findPatient(id) {
return patients.find(p => p.id === id);
}
// 添加患者
function addPatient(name, age, diagnosis) {
const newId = patients.length > 0
? patients[patients.length - 1].id + 1
: 1;
patients.push({ id: newId, name, age, diagnosis });
}
// 计算平均年龄
const avgAge = patients.reduce((sum, p) => sum + p.age, 0) / patients.length;

运行示例

---

## 总结

**恭喜你完成了JavaScript基础学习！**

本节我们学习了：

JavaScript的应用场景和重要性
变量声明（var/let/const）和数据类型
各类运算符的使用
流程控制语句
函数的定义和作用域
内置对象的常用方法
类型转换和错误处理