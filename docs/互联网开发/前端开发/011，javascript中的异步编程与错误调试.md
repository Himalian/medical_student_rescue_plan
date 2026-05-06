# JavaScript异步编程与错误调试

异步编程是JavaScript的核心特性之一，理解异步机制对于构建高性能的Web应用至关重要。本章将深入讲解异步编程的各种模式和错误处理技巧。

---

## 五、异步编程

### 5.1 回调函数

#### 核心概念：回调函数

**什么是回调函数？**

回调函数是作为参数传递给另一个函数的函数，在特定事件或条件发生时被调用。它是JavaScript异步编程的基础模式。

**回调函数的基本用法：**
```js
// 同步回调
const arr = [1, 2, 3];
arr.forEach(function(item) {
console.log(item); // 同步执行
});
// 异步回调 - 定时器
setTimeout(function() {
console.log("1秒后执行");
}, 1000);
// 异步回调 - 事件监听
document.getElementById("btn").addEventListener("click", function() {
console.log("按钮被点击");
});
// 医疗系统示例：获取患者数据
function getPatientData(patientId, callback) {
// 模拟异步请求
setTimeout(() => {
const data = {
id: patientId,
name: "张三",
age: 45,
diagnosis: "高血压"
};
callback(data); // 数据获取完成后调用回调
}, 1000);
}
getPatientData("P001", function(patient) {
console.log("患者信息：", patient);
});
```
### 5.2 回调地狱

#### 核心概念：回调地狱

**什么是回调地狱？**

当多个异步操作需要按顺序执行时，回调函数会层层嵌套，形成"金字塔"结构，这种代码难以阅读和维护，被称为"回调地狱"（Callback Hell）。

**回调地狱示例：**
```javascript
// 医疗系统：依次获取患者信息、病历、处方
getPatient("P001", function(patient) {
console.log("获取患者：", patient.name);
getMedicalRecord(patient.id, function(record) {
console.log("获取病历：", record);
getPrescription(record.id, function(prescription) {
console.log("获取处方：", prescription);
getMedicineList(prescription.id, function(medicines) {
console.log("获取药品列表：", medicines);
// 嵌套越来越深...
});
});
});
});
```
**回调地狱的问题：**

代码可读性差，难以理解
错误处理复杂，每层都需要单独处理
难以维护和扩展
代码缩进过深

---

### 5.3 Promise 进阶

#### 链式调用

#### 核心概念：Promise 链式调用

**链式调用的原理：**

Promise的then方法返回一个新的Promise，这使得我们可以将多个异步操作串联起来，避免回调地狱。每个then接收上一个Promise的返回值作为参数。

```javascript
// Promise 链式调用解决回调地狱
function getPatient(id) {
return new Promise(resolve => {
setTimeout(() => resolve({ id, name: "张三" }), 500);
});
}
function getMedicalRecord(patientId) {
return new Promise(resolve => {
setTimeout(() => resolve({ patientId, diagnosis: "高血压" }), 500);
});
}
function getPrescription(recordId) {
return new Promise(resolve => {
setTimeout(() => resolve({ recordId, medicines: ["降压药A", "降压药B"] }), 500);
});
}
// 链式调用 - 代码清晰多了
getPatient("P001")
.then(patient => {
console.log("患者：", patient);
return getMedicalRecord(patient.id);
})
.then(record => {
console.log("病历：", record);
return getPrescription(record.patientId);
})
.then(prescription => {
console.log("处方：", prescription);
})
.catch(error => {
console.error("出错了：", error);
});
```
#### Promise.all / Promise.race / Promise.allSettled / Promise.any

**Promise 静态方法对比：**

| 方法 | 说明 | 返回时机 | 返回结果 |
| --- | --- | --- | --- |
| Promise.all | 所有Promise都成功 | 全部完成或有一个失败 | 成功结果数组或第一个错误 |
| Promise.race | 第一个完成的Promise | 任一Promise完成 | 第一个完成的结果 |
| Promise.allSettled | 所有Promise完成 | 全部完成（无论成功失败） | 每个Promise的状态和结果 |
| Promise.any | 第一个成功的Promise | 任一成功或全部失败 | 第一个成功结果或AggregateError |

```javascript
// Promise.all - 并行请求，全部成功才继续
const patientPromise = fetch("/api/patient/001");
const doctorPromise = fetch("/api/doctor/D001");
const roomPromise = fetch("/api/room/R001");
Promise.all([patientPromise, doctorPromise, roomPromise])
.then([patient, doctor, room] => {
console.log("所有数据加载完成");
})
.catch(error => {
console.error("有一个请求失败：", error);
});
// Promise.race - 谁快用谁
const fastServer = Promise.race([
fetch("https://server1.api/data"),
fetch("https://server2.api/data")
]);
// Promise.allSettled - 获取所有结果
Promise.allSettled([
fetch("/api/service1"),
fetch("/api/service2")
]).then(results => {
results.forEach(result => {
if (result.status === "fulfilled") {
console.log("成功：", result.value);
}
});
});
// Promise.any - 只要有一个成功就行
Promise.any([
fetch("/api/backup1"),
fetch("/api/main")
]).then(result => {
console.log("至少有一个成功了");
});

演示 Promise.all
演示 Promise.race
演示 Promise.allSettled

#### 自定义 Promise

**实现一个简易版 Promise：**

class MyPromise {
constructor(executor) {
this.state = "pending";
this.value = undefined;
this.callbacks = [];
const resolve = (value) => {
if (this.state !== "pending") return;
this.state = "fulfilled";
this.value = value;
this.callbacks.forEach(cb => cb.onFulfilled(value));
};
const reject = (reason) => {
if (this.state !== "pending") return;
this.state = "rejected";
this.value = reason;
this.callbacks.forEach(cb => cb.onRejected(reason));
};
try {
executor(resolve, reject);
} catch (error) {
reject(error);
}
}
then(onFulfilled, onRejected) {
if (this.state === "fulfilled") {
onFulfilled(this.value);
} else if (this.state === "rejected") {
onRejected(this.value);
} else {
this.callbacks.push({ onFulfilled, onRejected });
}
}
}
// 使用自定义 Promise
const p = new MyPromise((resolve, reject) => {
setTimeout(() => resolve("成功！"), 1000);
});
p.then(value => console.log(value));

---

### 5.4 async/await

#### 语法与使用

#### 核心概念：async/await

**async/await 是什么？**

async/await 是 ES2017 引入的异步编程语法糖，它让异步代码看起来像同步代码，大大提高了代码的可读性。async 函数返回一个 Promise，await 用于等待 Promise 完成。

// async 函数基本语法
async function getData() {
return "Hello"; // 自动包装成 Promise
}
getData().then(value => console.log(value)); // "Hello"
// await 等待 Promise 完成
async function fetchPatient() {
const response = await fetch("/api/patient/001");
const patient = await response.json();
return patient;
}
// 对比：Promise 链式调用 vs async/await
// Promise 方式
getPatient("P001")
.then(patient => getMedicalRecord(patient.id))
.then(record => getPrescription(record.id))
.then(prescription => console.log(prescription));
// async/await 方式 - 更像同步代码
async function getPatientPrescription() {
const patient = await getPatient("P001");
const record = await getMedicalRecord(patient.id);
const prescription = await getPrescription(record.id);
console.log(prescription);
}

#### 错误处理（try...catch）

async function fetchPatientSafely() {
try {
const response = await fetch("/api/patient/001");
if (!response.ok) {
throw new Error("HTTP错误: " + response.status);
}
const patient = await response.json();
return patient;
} catch (error) {
console.error("获取患者信息失败：", error);
return null;
}
}
// 并行请求的错误处理
async function fetchAllData() {
try {
const [patients, doctors, rooms] = await Promise.all([
fetch("/api/patients").then(r => r.json()),
fetch("/api/doctors").then(r => r.json()),
fetch("/api/rooms").then(r => r.json())
]);
return { patients, doctors, rooms };
} catch (error) {
console.error("获取数据失败：", error);
throw error;
}
}

#### 结合 Promise

// async/await 与 Promise.all 结合
async function loadDashboard() {
const [userInfo, notifications, messages] = await Promise.all([
fetch("/api/user").then(r => r.json()),
fetch("/api/notifications").then(r => r.json()),
fetch("/api/messages").then(r => r.json())
]);
return { userInfo, notifications, messages };
}
// 带超时的请求
async function fetchWithTimeout(url, timeout = 5000) {
const controller = new AbortController();
const timeoutId = setTimeout(() => controller.abort(), timeout);
try {
const response = await fetch(url, { signal: controller.signal });
return await response.json();
} finally {
clearTimeout(timeoutId);
}
}

演示 async/await
演示并行请求

---

### 5.5 事件循环深入

#### 宏任务与微任务队列

#### 核心概念：宏任务与微任务

**任务队列的分类：**

JavaScript 事件循环中有两种任务队列：宏任务队列（Macro Task Queue）和微任务队列（Micro Task Queue）。微任务优先级高于宏任务，每次执行完一个宏任务后，会先清空所有微任务。

**宏任务与微任务分类：**

| 类型 | 包含 |
| --- | --- |
| 宏任务 | script（整体代码）、setTimeout、setInterval、setImmediate（Node.js）、I/O、UI渲染 |
| 微任务 | Promise.then/catch/finally、process.nextTick（Node.js）、MutationObserver、queueMicrotask |

// 事件循环执行顺序
console.log("1. 同步代码开始");
setTimeout(() => {
console.log("4. setTimeout 宏任务");
}, 0);
Promise.resolve()
.then(() => {
console.log("3. Promise 微任务");
});
console.log("2. 同步代码结束");
// 输出顺序：1 -> 2 -> 3 -> 4

#### 执行顺序分析

// 复杂的事件循环示例
console.log("1");
setTimeout(() => {
console.log("2");
Promise.resolve().then(() => console.log("3"));
}, 0);
Promise.resolve()
.then(() => {
console.log("4");
setTimeout(() => console.log("5"), 0);
})
.then(() => console.log("6"));
console.log("7");
// 输出顺序：1, 7, 4, 6, 2, 3, 5

同步代码

->

微任务队列

->

UI渲染

->

宏任务队列

->

循环...

演示事件循环

---

### 5.6 异步迭代

#### for await...of

#### 核心概念：异步迭代

**for await...of 是什么？**

for await...of 是 ES2018 引入的语法，用于遍历异步可迭代对象。它会等待每个 Promise 完成后再处理下一个，非常适合处理流式数据或分批请求。

// 异步生成器函数
async function\* fetchPatientsInBatches(ids, batchSize = 10) {
for (let i = 0; i < ids.length; i += batchSize) {
const batch = ids.slice(i, i + batchSize);
const patients = await fetchBatch(batch);
yield patients;
}
}
// 使用 for await...of 遍历
async function processAllPatients(patientIds) {
for await (const batch of fetchPatientsInBatches(patientIds)) {
for (const patient of batch) {
console.log("处理患者：", patient.name);
}
}
}
// 模拟异步数据流
async function\* medicalDataStream() {
const data = [
{ type: "vital", heartRate: 72 },
{ type: "vital", heartRate: 75 },
{ type: "alert", message: "心率偏高" },
{ type: "vital", heartRate: 68 }
];
for (const item of data) {
await new Promise(r => setTimeout(r, 500));
yield item;
}
}
async function monitorPatient() {
for await (const data of medicalDataStream()) {
if (data.type === "alert") {
console.warn("警报：", data.message);
} else {
console.log("心率：", data.heartRate);
}
}
}

---

## 六、错误处理与调试

### 6.1 错误对象

#### 核心概念：JavaScript 错误类型

**内置错误对象：**

JavaScript 提供了多种内置错误类型，每种类型代表不同类型的错误。了解这些错误类型有助于更精确地定位和处理问题。

**常见错误类型：**

| 错误类型 | 说明 | 常见场景 |
| --- | --- | --- |
| Error | 基础错误类型 | 自定义错误 |
| SyntaxError | 语法错误 | 代码解析错误 |
| TypeError | 类型错误 | 对错误类型执行操作 |
| ReferenceError | 引用错误 | 访问未定义的变量 |
| RangeError | 范围错误 | 数值超出有效范围 |
| URIError | URI错误 | URI处理函数错误 |
| AggregateError | 聚合错误 | 多个错误集合（ES2021） |

// 各种错误类型示例
// TypeError - 类型错误
const num = 123;
// num.toUpperCase(); // TypeError: num.toUpperCase is not a function
// ReferenceError - 引用错误
// console.log(undefinedVar); // ReferenceError: undefinedVar is not defined
// RangeError - 范围错误
// const arr = new Array(-1); // RangeError: Invalid array length
// 自定义错误
class MedicalError extends Error {
constructor(message, code) {
super(message);
this.name = "MedicalError";
this.code = code;
this.timestamp = new Date();
}
}
function validatePatient(patient) {
if (!patient.id) {
throw new MedicalError("患者ID不能为空", "INVALID\_PATIENT\_ID");
}
if (!patient.name) {
throw new MedicalError("患者姓名不能为空", "INVALID\_PATIENT\_NAME");
}
}

---

### 6.2 异常捕获

#### try...catch...finally

// try...catch 基本用法
try {
const data = JSON.parse(jsonString);
processData(data);
} catch (error) {
if (error instanceof SyntaxError) {
console.error("JSON格式错误：", error.message);
} else {
console.error("处理数据时出错：", error);
}
}
// try...catch...finally
async function loadPatientData(patientId) {
let loading = true;
try {
const response = await fetch(`/api/patients/${patientId}`);
const data = await response.json();
return data;
} catch (error) {
console.error("加载患者数据失败：", error);
throw error;
} finally {
loading = false; // 无论成功失败都执行
console.log("加载完成");
}
}
// 嵌套 try...catch
async function processMedicalRecord(recordId) {
try {
const record = await fetchRecord(recordId);
try {
const diagnosis = analyzeRecord(record);
return diagnosis;
} catch (analyzeError) {
console.error("分析病历失败：", analyzeError);
return { error: "分析失败", record };
}
} catch (fetchError) {
console.error("获取病历失败：", fetchError);
throw new Error("无法处理病历");
}
}

#### 抛出错误（throw）

// throw 语句
function divide(a, b) {
if (b === 0) {
throw new Error("除数不能为零");
}
return a / b;
}
// 医疗系统：验证并抛出自定义错误
class ValidationError extends Error {
constructor(field, message) {
super(`验证失败: ${field} - ${message}`);
this.name = "ValidationError";
this.field = field;
}
}
function validatePrescription(prescription) {
if (!prescription.patientId) {
throw new ValidationError("patientId", "患者ID不能为空");
}
if (!prescription.medicines || prescription.medicines.length === 0) {
throw new ValidationError("medicines", "药品列表不能为空");
}
return true;
}
// 使用示例
try {
validatePrescription({ patientId: "", medicines: [] });
} catch (error) {
if (error instanceof ValidationError) {
console.error(`字段 ${error.field} 验证失败：${error.message}`);
}
}
```
---

### 6.3 调试技巧

#### console 对象方法

**console 常用方法：**

| 方法 | 说明 | 用途 |
| --- | --- | --- |
| console.log() | 普通输出 | 一般调试信息 |
| console.error() | 错误输出 | 错误信息（红色显示） |
| console.warn() | 警告输出 | 警告信息（黄色显示） |
| console.table() | 表格输出 | 数组或对象以表格形式显示 |
| console.time() | 计时开始 | 测量代码执行时间 |
| console.timeEnd() | 计时结束 | 输出执行时间 |
| console.trace() | 堆栈跟踪 | 显示调用堆栈 |
| console.group() | 分组开始 | 组织相关日志 |
| console.dir() | 目录显示 | 显示对象属性 |

```js
// console.table - 表格显示
const patients = [
{ id: "P001", name: "张三", age: 45 },
{ id: "P002", name: "李四", age: 32 }
];
console.table(patients);
// console.time - 性能测量
console.time("数据处理");
for (let i = 0; i < 10000; i++) {
// 处理数据
}
console.timeEnd("数据处理"); // 输出：数据处理: 2.345ms
// console.trace - 堆栈跟踪
function funcA() {
funcB();
}
function funcB() {
funcC();
}
function funcC() {
console.trace("调用堆栈");
}
// console.group - 分组输出
console.group("患者信息");
console.log("姓名：张三");
console.log("年龄：45");
console.groupEnd();
// console.dir - 显示对象结构
console.dir(document.body);

#### 浏览器开发者工具（Sources 面板断点调试）

**断点调试步骤：**

/\*
\* 使用浏览器开发者工具调试：
\*
\* 1. 打开开发者工具：F12 或 Ctrl+Shift+I (Mac: Cmd+Option+I)
\* 2. 切换到 Sources 面板
\* 3. 在代码行号处点击设置断点
\* 4. 刷新页面或触发相关代码
\* 5. 代码执行到断点处暂停
\*
\* 调试时可用的操作：
\* - Continue (F8): 继续执行到下一个断点
\* - Step Over (F10): 执行当前行，不进入函数
\* - Step Into (F11): 进入函数内部
\* - Step Out (Shift+F11): 跳出当前函数
\*
\* 调试面板：
\* - Watch: 监视变量值
\* - Call Stack: 调用堆栈
\* - Scope: 当前作用域变量
\*/
// 示例：调试患者数据处理
function processPatientData(data) {
let result = []; // 在此处设置断点
for (const patient of data) {
const processed = {
id: patient.id,
fullName: patient.firstName + " " + patient.lastName,
age: calculateAge(patient.birthDate)
};
result.push(processed);
}
return result;
}
```

**调试技巧：**
条件断点：右键断点可设置条件，只有满足条件时才暂停
日志点（Logpoint）：不暂停执行，只输出日志
异常断点：在 Sources 面板右侧可设置在异常处暂停

#### 使用 debugger 语句

```javascript
// debugger 语句 - 代码中的断点
function complexCalculation(data) {
let result = 0;
for (let i = 0; i < data.length; i++) {
if (i === 5) {
debugger; // 开发者工具打开时，执行到这里会暂停
}
result += data[i];
}
return result;
}
// 结合条件使用 debugger
function validateInput(value) {
if (value < 0) {
debugger; // 只在异常值时暂停
console.warn("检测到负值：", value);
}
return value;
}
```

**注意：** debugger 语句只在开发者工具打开时生效。生产环境应移除所有 debugger 语句。

演示 console 方法
演示 debugger

---

#### 本章小结

**异步编程要点：**

- 回调函数是异步编程的基础，但容易形成回调地狱
- Promise 提供了更优雅的异步处理方式，支持链式调用
- Promise.all/race/allSettled/any 提供了不同的并行处理策略
- async/await 让异步代码写起来像同步代码，可读性更好
- 理解事件循环机制（宏任务、微任务）对调试异步代码很重要
for await...of 可以遍历异步数据流

**错误处理要点：**

- 了解不同类型的错误对象有助于精确定位问题
- try...catch...finally 提供了完整的异常处理机制
- 自定义错误类可以携带更多业务相关信息
- console 对象提供了丰富的调试方法
浏览器开发者工具是调试的利器
debugger 语句可以在代码中设置断点