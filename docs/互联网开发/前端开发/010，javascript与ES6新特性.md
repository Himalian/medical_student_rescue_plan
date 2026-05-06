# JavaScript与ES6新特性

ES6（ECMAScript 2015）是JavaScript语言的一次重大更新，引入了大量新特性，使JavaScript更加强大和易用。本章将系统讲解ES6及后续版本的核心新特性。

---

## 一、变量声明

### 1.1 let 与 const

#### 核心概念：let 和 const

**为什么需要let和const？**

ES5中只有var声明变量，存在变量提升、无块级作用域等问题。let和const的引入解决了这些问题，使变量声明更加安全和可控。

**let、const、var 对比：**

| 特性 | var | let | const |
| --- | --- | --- | --- |
| 作用域 | 函数作用域 | 块级作用域 | 块级作用域 |
| 变量提升 | 是（初始化为undefined） | 否（暂时性死区） | 否（暂时性死区） |
| 重复声明 | 允许 | 不允许 | 不允许 |
| 重新赋值 | 允许 | 允许 | 不允许 |
| 全局变量挂载 | 挂载到window | 不挂载 | 不挂载 |

#### 块级作用域

```javascript
// var 没有块级作用域
if (true) {
var x = 10;
}
console.log(x); // 10 - 可以访问
// let 有块级作用域
if (true) {
let y = 20;
}
// console.log(y); // 报错！y is not defined
// 经典循环问题
for (var i = 0; i < 3; i++) {
setTimeout(() => console.log("var:" + i), 100);
}
// 输出：var:3, var:3, var:3
for (let j = 0; j < 3; j++) {
setTimeout(() => console.log("let:" + j), 100);
}
// 输出：let:0, let:1, let:2
```
运行示例

#### 暂时性死区（TDZ）

```javascript
// 暂时性死区：在声明之前访问let/const变量会报错
{
    // console.log(x); // 报错！Cannot access 'x' before initialization
    let x = 10;
    console.log(x); // 10
}
// const 必须初始化
const PI = 3.14159;
// const x; // 报错！Missing initializer
// const 引用不可变，但对象属性可变
const obj = { name: "张三" };
obj.name = "李四"; // 允许
// obj = {}; // 报错！不能重新赋值
// 冻结对象（使属性也不可变）
const frozenObj = Object.freeze({ value: 100 });
// frozenObj.value = 200; // 静默失败（严格模式下报错）
```

**最佳实践：**

默认使用 **const**，只有需要重新赋值时才用 **let**
避免使用 **var**
const 声明的对象，如需不可变，使用 Object.freeze()

---

## 二、模板字符串

#### 核心概念：模板字符串

**什么是模板字符串？**

模板字符串使用反引号（`）包裹，支持插值表达式、多行字符串和标签模板，比传统字符串拼接更简洁、更强大。

#### 插值与多行字符串

```javascript
// 传统字符串拼接
const name = "张三";
const age = 25;
const oldWay = "姓名：" + name + "，年龄：" + age;
// 模板字符串插值
const newWay = `姓名：${name}，年龄：${age}`;
// 插值可以是任意表达式
const a = 10;
const b = 20;
console.log(`${a} + ${b} = ${a + b}`); // "10 + 20 = 30"
// 多行字符串
const html = `
<div class="card">
<h2>${name}</h2>
<p>年龄：${age}岁</p>
</div>
`;
// 嵌套模板
const items = ["苹果", "香蕉", "橙子"];
const list = `<ul>
${items.map(item => `<li>${item}</li>`).join("\n ")}
</ul>`;
```

运行示例

#### 标签模板

```javascript
// 标签模板：模板字符串可以紧跟在一个函数后面
function tag(strings, ...values) {
    console.log(strings); // 字符串数组
    console.log(values); // 插值数组
    return strings.reduce((result, str, i) =>
        result + str + (values[i] !== undefined ? values[i] : ""), "");
}
const name = "张三";
const age = 25;
tag`你好，${name}！你今年${age}岁了。`;
// 实际应用：HTML转义
function safeHtml(strings, ...values) {
    const escape = (str) => String(str)
        .replace(/&/g, "&amp;")
        .replace(/</g, "&gt;")
        .replace(/"/g, "&quot;");
    return strings.reduce((result, str, i) =>
        result + str + (values[i] !== undefined ? escape(values[i]) : ""), "");
}
const userInput = '<script>alert("XSS")</script>';
const safe = safeHtml`<div>${userInput}</div>`;
```

---

## 三、函数增强

### 3.1 箭头函数

#### 核心概念：箭头函数

**箭头函数的特点：**

语法更简洁
没有自己的this，继承外层作用域的this
没有arguments对象
不能作为构造函数（不能用new调用）
没有prototype属性

// 箭头函数语法
// 传统函数
const add1 = function(a, b) {
    return a + b;
};
// 箭头函数
const add2 = (a, b) => {
    return a + b;
};
// 简写：单行可省略大括号和return
const add3 = (a, b) => a + b;
// 单参数可省略括号
const double = x => x * 2;
// 返回对象需要加括号
const createPerson = (name, age) => ({ name, age });

#### this 绑定

```javascript
// 传统函数的this问题
const obj1 = {
    name: "张三",
    greet: function() {
        setTimeout(function() {
            console.log(this.name); // undefined（this丢失）
        }, 100);
    }
};
// 箭头函数继承this
const obj2 = {
    name: "张三",
    greet: function() {
        setTimeout(() => {
            console.log(this.name); // "张三"（继承外层this）
        }, 100);
    }
};
// 医疗场景：患者信息处理
const patient = {
    name: "李四",
    vitals: [120, 80, 72],
    analyze: function() {
        this.vitals.forEach((v, i) => {
            console.log(`${this.name}的第${i+1}项生命体征：${v}`);
        });
    }
};
```

运行示例

### 3.2 函数默认参数

```javascript
// ES5 默认参数
function greet1(name) {
    name = name || "访客"; // 问题：空字符串也会被替换
    return `你好，${name}！`;
}
// ES6 默认参数
function greet2(name = "访客") {
    return `你好，${name}！`;
}
greet2(); // "你好，访客！"
greet2("张三"); // "你好，张三！"
greet2(""); // "你好，！"（空字符串不会被替换）
// 默认参数可以是表达式
function getConfig(url, timeout = fetchTimeout(), headers = {}) {
    // ...
}
// 默认参数在参数列表中的位置
function fn(a, b = 2, c) { // c没有默认值，但在b之后
    console.log(a, b, c);
}
fn(1); // 1, 2, undefined
fn(1, undefined, 3); // 1, 2, 3
```

### 3.3 剩余参数与展开运算符

```javascript
// 剩余参数（Rest Parameters）
function sum(...numbers) {
    return numbers.reduce((total, n) => total + n, 0);
}
sum(1, 2, 3, 4, 5); // 15
// 剩余参数必须是最后一个参数
function log(prefix, ...messages) {
    messages.forEach(msg => console.log(prefix + msg));
}
// 展开运算符（Spread Operator）
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
// 数组展开
const merged = [...arr1, ...arr2]; // [1, 2, 3, 4, 5, 6]
const copied = [...arr1]; // [1, 2, 3]（浅拷贝）
// 函数调用时展开
Math.max(...arr1); // 3
// 对象展开
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, ...obj1 }; // { c: 3, a: 1, b: 2 }
// 医疗场景：合并患者信息
const baseInfo = { hospital: "中心医院", department: "内科" };
const patientInfo = { ...baseInfo, name: "王五", age: 45 };
```

运行示例

---

## 四、解构赋值

#### 核心概念：解构赋值

**什么是解构赋值？**

解构赋值是一种从数组或对象中提取值并赋给变量的语法。它可以让代码更简洁，特别是在处理复杂数据结构时。

#### 数组解构

```javascript
// 基本数组解构
const [a, b] = [1, 2];
console.log(a, b); // 1 2
// 跳过元素
const [x, , y] = [1, 2, 3];
console.log(x, y); // 1 3
// 剩余元素
const [first, ...rest] = [1, 2, 3, 4, 5];
console.log(first); // 1
console.log(rest); // [2, 3, 4, 5]
// 默认值
const [m = 10, n = 20] = [undefined, 5];
console.log(m, n); // 10 5
// 交换变量
let p = 1, q = 2;
[p, q] = [q, p];
console.log(p, q); // 2 1
```

#### 对象解构

```javascript
// 基本对象解构
const { name, age } = { name: "张三", age: 25 };
console.log(name, age); // "张三" 25
// 重命名变量
const { name: userName, age: userAge } = { name: "李四", age: 30 };
console.log(userName, userAge); // "李四" 30
// 默认值
const { x = 100, y = 200 } = { x: 10 };
console.log(x, y); // 10 200
// 剩余属性
const { a, ...others } = { a: 1, b: 2, c: 3 };
console.log(a); // 1
console.log(others); // { b: 2, c: 3 }
// 医疗场景：解构患者数据
const patient = {
    id: "P001",
    name: "王五",
    vitals: {
        bloodPressure: { systolic: 120, diastolic: 80 },
        heartRate: 72
    }
};
const { id, name: patientName, vitals: { bloodPressure: { systolic } } } = patient;
console.log(patientName, systolic); // "王五" 120
```

运行示例

#### 嵌套解构与默认值

```javascript
// 嵌套数组解构
const [[a1, a2], [b1, b2]] = [[1, 2], [3, 4]];
// 嵌套对象解构
const data = {
    user: {
        profile: {
            name: "张三",
            settings: {
                theme: "dark"
            }
        }
    }
};
const { user: { profile: { name, settings: { theme = "light" } = {} } = {} } = {} } = data;
// 函数参数解构
function createUser({ name, age = 0, role = "user" } = {}) {
    return { name, age, role };
}
createUser({ name: "张三" }); // { name: "张三", age: 0, role: "user" }
createUser(); // { name: undefined, age: 0, role: "user" }
```

---

## 五、对象字面量增强

```javascript
// 属性简写
const name = "张三";
const age = 25;
// ES5
const user1 = { name: name, age: age };
// ES6 简写
const user2 = { name, age };
// 方法简写
const obj = {
    // ES5
    sayHello: function() {
        return "Hello";
    },
    // ES6 简写
    greet() {
        return "Hi";
    }
};
// 计算属性名
const key = "dynamic";
const obj2 = {
    [key]: "value",
    [`prefix_${key}`]: "another value"
};
// { dynamic: "value", prefix_dynamic: "another value" }
// 医疗场景
const createPatient = (id, name, ward) => ({
    id,
    name,
    ward,
    [`ward_${ward}`]: true,
    getInfo() {
        return `患者${this.name}，编号${this.id}`;
    }
});
```

运行示例

---

## 六、数组与对象扩展

### 6.1 数组方法

**ES6+ 新增数组方法：**

| 方法 | 说明 | 返回值 |
| --- | --- | --- |
| forEach() | 遍历数组 | undefined |
| map() | 映射转换 | 新数组 |
| filter() | 过滤元素 | 新数组 |
| reduce() | 累积计算 | 累积值 |
| find() | 查找元素 | 元素或undefined |
| findIndex() | 查找索引 | 索引或-1 |
| some() | 是否存在满足条件的元素 | boolean |
| every() | 是否所有元素都满足条件 | boolean |
| includes() | 是否包含某值 | boolean |
| Array.from() | 从类数组创建数组 | 新数组 |
| Array.of() | 创建数组 | 新数组 |

```javascript
const patients = [
    { id: 1, name: "张三", age: 45, department: "内科" },
    { id: 2, name: "李四", age: 32, department: "外科" },
    { id: 3, name: "王五", age: 58, department: "内科" }
];
// forEach - 遍历
patients.forEach(p => console.log(p.name));
// map - 提取所有姓名
const names = patients.map(p => p.name);
// ["张三", "李四", "王五"]
// filter - 筛选内科患者
const internalMedicine = patients.filter(p => p.department === "内科");
// reduce - 计算平均年龄
const avgAge = patients.reduce((sum, p) => sum + p.age, 0) / patients.length;
// find - 查找特定患者
const patient = patients.find(p => p.id === 2);
// some/every - 检查条件
const hasElderly = patients.some(p => p.age >= 60); // false
const allAdults = patients.every(p => p.age >= 18); // true
// Array.from - 从类数组创建
const arr = Array.from("hello"); // ["h", "e", "l", "l", "o"]
const nums = Array.from({ length: 5 }, (_, i) => i + 1); // [1, 2, 3, 4, 5]
```

运行示例

### 6.2 对象方法

```javascript
const patient = {
    id: "P001",
    name: "张三",
    age: 45
};
// Object.keys - 获取所有键
Object.keys(patient); // ["id", "name", "age"]
// Object.values - 获取所有值
Object.values(patient); // ["P001", "张三", 45]
// Object.entries - 获取键值对数组
Object.entries(patient);
// [["id", "P001"], ["name", "张三"], ["age", 45]]
// Object.assign - 合并对象
const merged = Object.assign({}, patient, { department: "内科" });
// Object.fromEntries - 从键值对创建对象
const obj = Object.fromEntries([["a", 1], ["b", 2]]); // { a: 1, b: 2 }
```

运行示例

---

## 七、Symbol 类型

#### 核心概念：Symbol

**什么是Symbol？**

Symbol是ES6引入的一种新的原始数据类型，表示独一无二的值。每个Symbol值都是唯一的，主要用于创建对象的私有属性和内置行为。

```javascript
// 创建Symbol
const sym1 = Symbol();
const sym2 = Symbol("description"); // 带描述
console.log(sym1 === sym2); // false（每个Symbol都是唯一的）
// 用作对象属性（私有属性）
const ID = Symbol("id");
const user = {
    name: "张三",
    [ID]: 12345
};
Object.keys(user); // ["name"] - Symbol属性不会被枚举
user[ID]; // 12345
// 全局Symbol注册表
const globalSym1 = Symbol.for("app.id");
const globalSym2 = Symbol.for("app.id");
console.log(globalSym1 === globalSym2); // true
```

运行示例

---

## 八、Set 与 Map

### 8.1 Set

```javascript
// 创建Set
const set = new Set([1, 2, 3, 2, 1]);
console.log(set); // Set {1, 2, 3} - 自动去重
// 基本操作
set.add(4); // 添加元素
set.delete(2); // 删除元素
set.has(1); // true - 检查是否存在
set.size; // 3 - 元素数量
set.clear(); // 清空
// 数组去重
const arr = [1, 2, 2, 3, 3, 3];
const unique = [...new Set(arr)]; // [1, 2, 3]
// 遍历Set
for (const item of set) {
    console.log(item);
}
```

运行示例

### 8.2 Map

```javascript
// 创建Map
const map = new Map();
// 基本操作
map.set("name", "张三"); // 设置键值对
map.set("age", 25);
map.get("name"); // "张三" - 获取值
map.has("name"); // true - 检查是否存在
map.delete("age"); // 删除
map.size; // 1 - 元素数量
// Map的键可以是任意类型
const objKey = { id: 1 };
map.set(objKey, "对象作为键");
// 遍历Map
for (const [key, value] of map) {
    console.log(key, value);
}
// Map与Object对比
// Map: 键可以是任意类型，有序，有size属性
// Object: 键只能是字符串或Symbol，无序
```

运行示例

### 8.3 WeakSet 与 WeakMap

```javascript
// WeakMap - 键必须是对象，弱引用
const weakMap = new WeakMap();
let key = { id: 1 };
weakMap.set(key, "私有数据");
weakMap.get(key); // "私有数据"
key = null; // key会被垃圾回收，WeakMap中的条目自动清除
// WeakMap 应用场景：存储DOM元素的关联数据
const domData = new WeakMap();
const element = document.getElementById("myElement");
domData.set(element, { clickCount: 0 });
// 当element被删除时，关联数据自动释放
```

**WeakSet 与 WeakMap 的特点：**

只能存储对象作为键/值
弱引用：不影响垃圾回收
不可遍历（没有size属性、forEach等方法）
适用于缓存、私有数据存储等场景

---

## 九、迭代器与生成器

### 9.1 迭代器（Iterator）

#### 核心概念：迭代器

**什么是迭代器？**

迭代器是一种对象，它提供了一种统一的方式来遍历集合中的元素。任何具有 Symbol.iterator 属性的对象都是可迭代的。

```javascript
// 迭代器协议
const iterableObj = {
    data: [1, 2, 3],
    [Symbol.iterator]() {
        let index = 0;
        const data = this.data;
        return {
            next() {
                if (index < data.length) {
                    return { value: data[index++], done: false };
                }
                return { done: true };
            }
        };
    }
};
// 使用迭代器
for (const item of iterableObj) {
    console.log(item); // 1, 2, 3
}
// 手动调用迭代器
const iterator = iterableObj[Symbol.iterator]();
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
```

运行示例

### 9.2 for...of 循环

```javascript
// for...of 遍历可迭代对象
const arr = ["a", "b", "c"];
// 数组
for (const item of arr) {
    console.log(item); // a, b, c
}
// 字符串
for (const char of "hello") {
    console.log(char); // h, e, l, l, o
}
// Map
const map = new Map([["a", 1], ["b", 2]]);
for (const [key, value] of map) {
    console.log(key, value);
}
// for...of vs for...in
// for...of: 遍历值，适用于可迭代对象
// for...in: 遍历键（索引/属性名），适用于对象
```

### 9.3 生成器函数

```javascript
// 生成器函数使用 function* 声明
function* numberGenerator() {
    yield 1;
    yield 2;
    yield 3;
}
const gen = numberGenerator();
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: undefined, done: true }
// 生成器实现无限序列
function* idGenerator() {
    let id = 1;
    while (true) {
        yield id++;
    }
}
const ids = idGenerator();
console.log(ids.next().value); // 1
console.log(ids.next().value); // 2
// 生成器实现异步流程控制
function* fetchData() {
    try {
        const user = yield fetch("/api/user");
        const posts = yield fetch(`/api/posts/${user.id}`);
        return posts;
    } catch (e) {
        console.error(e);
    }
}
```

运行示例

---

## 十、类（class）

#### 核心概念：类

**ES6 类的本质：**

ES6 的 class 本质上是构造函数的语法糖，它让对象原型的写法更加清晰、更像面向对象编程的语法。类不会提升，必须先定义后使用。

### 10.1 类的声明与实例化

```javascript
// 类的声明
class Person {
    // 构造函数
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    // 实例方法
    greet() {
        return `你好，我是${this.name}，今年${this.age}岁`;
    }
    // 静态方法
    static create(name, age) {
        return new Person(name, age);
    }
}
// 实例化
const person = new Person("张三", 25);
console.log(person.greet()); // "你好，我是张三，今年25岁"
console.log(Person.create("李四", 30).greet());
```

运行示例

### 10.2 类的继承

```javascript
// 父类
class Animal {
    constructor(name) {
        this.name = name;
    }
    speak() {
        console.log(`${this.name}发出声音`);
    }
}
// 子类继承父类
class Dog extends Animal {
    constructor(name, breed) {
        super(name); // 调用父类构造函数
        this.breed = breed;
    }
    // 重写父类方法
    speak() {
        console.log(`${this.name}汪汪叫`);
    }
    // 子类特有方法
    fetch() {
        console.log(`${this.name}去捡球`);
    }
}
const dog = new Dog("旺财", "金毛");
dog.speak(); // "旺财汪汪叫"
dog.fetch(); // "旺财去捡球"
// 医疗场景：患者类继承
class Patient extends Person {
    constructor(name, age, medicalRecord) {
        super(name, age);
        this.medicalRecord = medicalRecord;
        this.admissions = [];
    }
    admit(department) {
        this.admissions.push({
            department,
            date: new Date()
        });
    }
}
```

运行示例

### 10.3 私有属性与方法

```javascript
// ES2022 私有属性（使用 # 前缀）
class BankAccount {
    // 私有属性
    #balance = 0;
    #pin;
    constructor(initialBalance, pin) {
        this.#balance = initialBalance;
        this.#pin = pin;
    }
    // 私有方法
    #validatePin(inputPin) {
        return inputPin === this.#pin;
    }
    deposit(amount) {
        if (amount > 0) {
            this.#balance += amount;
        }
    }
    withdraw(amount, pin) {
        if (this.#validatePin(pin) && amount <= this.#balance) {
            this.#balance -= amount;
            return amount;
        }
        return 0;
    }
    getBalance() {
        return this.#balance;
    }
}
const account = new BankAccount(1000, "1234");
// account.#balance; // 报错！私有属性无法外部访问
console.log(account.getBalance()); // 1000
```

---

## 十一、模块化

#### 核心概念：ES6 模块

**模块化的优势：**

代码分割：将代码拆分成独立的模块，便于维护
命名空间：避免全局变量污染
依赖管理：明确模块之间的依赖关系
按需加载：提高页面加载性能

### 11.1 导出（export）

```javascript
// ========== utils.js ==========
// 命名导出
export const PI = 3.14159;
export function sum(a, b) {
    return a + b;
}
export class Calculator {
    add(a, b) { return a + b; }
    subtract(a, b) { return a - b; }
}
// 统一导出
const name = "utils";
const version = "1.0.0";
export { name, version };
// 重命名导出
export { sum as addNumbers };
// 默认导出（每个模块只能有一个）
export default class Patient {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}
```

### 11.2 导入（import）

```javascript
// ========== main.js ==========
// 导入默认导出
import Patient from "./utils.js";
// 导入命名导出
import { PI, sum, Calculator } from "./utils.js";
// 导入并重命名
import { sum as add } from "./utils.js";
// 导入所有（作为命名空间）
import * as Utils from "./utils.js";
Utils.PI; // 3.14159
// 同时导入默认和命名导出
import Patient, { PI, sum } from "./utils.js";
// 动态导入（返回Promise）
import("./utils.js").then(module => {
    console.log(module.PI);
});
```

**模块化注意事项：**

ES6 模块默认运行在严格模式下
模块中的变量是局部的，不会污染全局作用域
import 是静态的，必须在模块顶层使用（动态导入除外）
导出的值是"活绑定"，导入方可以获取更新后的值

---

## 十二、Promise 基础

#### 核心概念：Promise

**什么是Promise？**

Promise是ES6引入的异步编程解决方案，它代表一个异步操作的最终完成（或失败）及其结果值。Promise有三种状态：pending（进行中）、fulfilled（已成功）、rejected（已失败），状态一旦改变就不会再变。

### 12.1 Promise 的创建与状态

```javascript
// 创建Promise
const promise = new Promise((resolve, reject) => {
    // 异步操作
    setTimeout(() => {
        const success = true;
        if (success) {
            resolve("操作成功！"); // 状态变为 fulfilled
        } else {
            reject("操作失败！"); // 状态变为 rejected
        }
    }, 1000);
});
// Promise状态
// pending: 初始状态，操作进行中
// fulfilled: 操作成功完成
// rejected: 操作失败
```

### 12.2 then、catch、finally

```javascript
// then - 处理成功
promise
    .then(result => {
        console.log("成功：" + result);
        return "处理后的结果"; // 可以返回值给下一个then
    })
    .then(data => {
        console.log(data); // "处理后的结果"
    });
// catch - 处理错误
promise
    .then(result => {
        // 处理成功
    })
    .catch(error => {
        console.error("错误：" + error);
    });
// finally - 无论成功失败都执行
promise
    .then(result => console.log(result))
    .catch(error => console.error(error))
    .finally(() => {
        console.log("操作完成（无论成功或失败）");
    });
```

运行示例

### 12.3 错误处理

```javascript
// 模拟医疗数据获取
function fetchPatientData(id) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (id > 0) {
                resolve({
                    id,
                    name: "张三",
                    age: 45,
                    department: "内科"
                });
            } else {
                reject(new Error("无效的患者ID"));
            }
        }, 500);
    });
}
// 使用Promise
fetchPatientData(1)
    .then(patient => {
        console.log("患者信息：", patient);
        return fetchPatientData(2); // 链式调用
    })
    .then(patient2 => {
        console.log("第二位患者：", patient2);
    })
    .catch(error => {
        console.error("获取失败：", error.message);
    });
// Promise.all - 并行执行多个Promise
Promise.all([
    fetchPatientData(1),
    fetchPatientData(2),
    fetchPatientData(3)
]).then(patients => {
    console.log("所有患者：", patients);
}).catch(error => {
    console.error("至少一个请求失败");
});
// Promise.race - 返回最先完成的Promise
Promise.race([
    fetchPatientData(1),
    new Promise((_, reject) =>
        setTimeout(() => reject(new Error("请求超时")), 300)
    )
]);
```

运行示例

---

## 十三、总结

#### ES6+ 核心新特性回顾

| 类别 | 特性 | 主要用途 |
| --- | --- | --- |
| 变量声明 | let、const | 块级作用域、常量声明 |
| 字符串 | 模板字符串 | 字符串插值、多行文本 |
| 函数 | 箭头函数、默认参数、rest/spread | 简洁语法、this绑定、灵活参数 |
| 解构 | 数组解构、对象解构 | 快速提取数据 |
| 对象 | 属性简写、计算属性 | 简化对象定义 |
| 数据结构 | Symbol、Set、Map | 唯一值、集合、键值对 |
| 流程控制 | 迭代器、生成器 | 自定义遍历、惰性计算 |
| 面向对象 | class、extends | 类定义、继承 |
| 模块化 | export、import | 代码组织、依赖管理 |
| 异步编程 | Promise | 异步操作、链式调用 |

**学习建议：**

ES6+ 特性是现代 JavaScript 开发的基础，务必熟练掌握
优先使用 const/let 替代 var
善用解构和展开运算符简化代码
理解 Promise 是学习 async/await 的基础
模块化是构建大型应用的必备技能

---

## 十四、学习资源

[ECMAScript 6 入门 - 阮一峰](https://es6.ruanyifeng.com/)
[MDN JavaScript 文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)
[现代 JavaScript 教程](https://javascript.info/)
[ECMAScript 规范（需要梯子访问）](https://www.ecma-international.org/ecma-262/)