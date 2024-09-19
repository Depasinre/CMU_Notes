---
title: 前端面试准备杂项 (1)
date: 2024-09-04 17:41:43
categories: 学习笔记
tags: 
- Interview Preparation
---

This Note contains: 

- JavaScript 数据类型
- 浏览器进程

<!-- more -->
<!-- toc -->

## JavaScript 数据类型

JavaScript 中的简单数据类型包括以下几种：

- String：用于表示文本数据，用引号（单引号或双引号）包裹起来，例如："Hello, World!"。
- Number：用于表示数值数据，包括整数和浮点数（带小数点的数），例如：42、3.14。
- Boolean：用于表示逻辑值，只有两个可能的取值：true（真）和false（假）。
- undefined：表示未定义的值，通常表示未声明的变量或缺少返回值的函数。
- null：表示空值，用于显式地表示变量或对象没有值。
- Symbol：表示唯一的标识符，用于对象属性的键。
- BigInt：用于表示任意精度的整数。BigInt 是一种简单数据类型，在 ECMAScript 2020 中引入

JavaScript 还有引用类型 Object 可以存储键值对，也可以是其他数据结构如数组、函数等。它包括其他特殊类型，如 `Array`、`Function`、`Date`、`RegExp` 等。

### 如何进行数据类型检测? 

1. `typeof`

```javascript
console.log(typeof 2);               // number
console.log(typeof true);            // boolean
console.log(typeof 'str');           // string
console.log(typeof []);              // object    
console.log(typeof function(){});    // function
console.log(typeof {});              // object
console.log(typeof undefined);       // undefined
console.log(typeof null);            // object
```

这种方法无法识别Object类型的具体类型, 如Array, Object等

**typeof 会将null识别为object**

2. `instanceof`

```JavaScript
console.log(2 instanceof Number);                    // false
console.log(true instanceof Boolean);                // false 
console.log('str' instanceof String);                // false 
 
console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true
```

instanceof只能正确判断引用数据类型，而不能判断基本数据类型。instanceof 运算符可以用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。

3. `constructor`

```
console.log((2).constructor === Number); // true
console.log((true).constructor === Boolean); // true
console.log(('str').constructor === String); // true
console.log(([]).constructor === Array); // true
console.log((function() {}).constructor === Function); // true
console.log(({}).constructor === Object); 
```

4. `Object.prototype.toString.call()`

这是检测复杂类型（如数组、日期等）的通用方法。它通过返回类型的完整字符串来区分不同的对象类型。

```JavaScript
Object.prototype.toString.call([1, 2, 3]);  // "[object Array]"
Object.prototype.toString.call(new Date()); // "[object Date]"
Object.prototype.toString.call(null);       // "[object Null]"
Object.prototype.toString.call(undefined);  // "[object Undefined]"
Object.prototype.toString.call({});         // "[object Object]"
```

## 浏览器的进程和线程

现代浏览器（如 Chrome、Edge 等）采用多进程架构，每个主要模块通常都运行在独立的进程中。这种设计可以防止一个模块的崩溃影响整个浏览器的运行，提高稳定性和安全性。

- **浏览器进程**：这是核心进程，负责管理浏览器的所有其他进程和模块，如窗口、标签页、用户界面（UI）等。它还负责处理网络请求、下载、书签、历史记录管理等。
- **渲染进程（Renderer Process）**：每个浏览器标签页或 iframe 通常有自己的渲染进程。渲染进程负责将 HTML、CSS 和 JavaScript 转换为用户可以看到和交互的内容。这些进程通常是沙盒化的，以提高安全性（即使某个标签页崩溃也不会影响其他标签页）。
- **GPU 进程**：用于处理图形渲染任务，如 3D 渲染、硬件加速的 2D 渲染。浏览器在绘制页面时会依赖 GPU 进程来提高渲染性能。
- **插件进程**：浏览器中的插件（如 Flash 或其他扩展程序）运行在独立的进程中。这确保了插件崩溃时不会影响到浏览器的核心进程或渲染进程。
- **网络进程**：负责处理网络请求，如 HTTP 请求、响应等。它管理浏览器的所有网络通信，将结果交给渲染进程来展示内容。

在每个进程中，又包含多个线程来处理具体的任务。以渲染进程为例，它包含了多个关键线程：

- **GUI 渲染线程**：负责渲染浏览器的界面和内容。当 HTML 和 CSS 被解析后，GUI 渲染线程生成最终的页面布局和绘制内容。这个线程和 JavaScript 线程是互斥的，即在 JavaScript 执行时，GUI 渲染会被暂停，防止页面在修改 DOM 时发生冲突。
- **JavaScript 引擎线程**：这个线程专门负责执行页面中的 JavaScript 代码。以 Chrome 为例，它使用的是 V8 引擎来执行 JavaScript。JavaScript 是单线程的，这意味着一次只能执行一个任务，但可以通过异步操作（如 `setTimeout`、`Promise` 或 `async/await`）来进行任务调度。
- **事件触发线程**：负责管理事件循环，如用户交互（点击、输入）、定时器（`setTimeout`、`setInterval`）等。当事件触发时，它会将回调函数放入任务队列中，等待 JavaScript 引擎线程处理。
- **定时器线程**：专门用于管理定时器函数的执行，如 `setTimeout` 和 `setInterval`。当计时结束后，定时器线程会将相应的回调放入事件队列中。
- **HTTP 请求线程**：处理异步 HTTP 请求（如 `XMLHttpRequest` 或 `fetch`）。当请求完成后，数据被传回渲染进程，并通过回调将结果交给 JavaScript 引擎线程处理。

**进程（Process）**：进程是操作系统中运行程序的实例。进程之间是相互独立的，进程的崩溃不会影响其他进程。进程具有独立的内存空间，因此进程之间的通信比较复杂（如 IPC）。

**线程（Thread）**：线程是进程内部的执行单元。一个进程可以包含多个线程，线程共享进程的内存空间。线程之间的通信比较容易，因为它们共享内存，但这也可能带来竞争条件和锁定等问题。

### Web Worker

1. **多线程**：Web Worker 是在主线程之外运行的独立线程。它不会阻塞主线程，因此可以用来执行耗时的操作（如大规模计算、数据处理等），同时保持用户界面的流畅。
2. **不共享上下文**：Web Worker 和主线程之间不共享相同的 JavaScript 执行环境。Web Worker 中不能直接访问 DOM，也不能访问主线程中的全局对象 `window`、`document`、`localStorage` 等。这确保了 Web Worker 的运行是完全独立的，避免了竞争条件和资源共享带来的复杂性。
3. **通过消息传递通信**：Web Worker 和主线程之间的通信是通过消息传递机制（`postMessage()` 和 `onmessage`）进行的。它们可以发送和接收字符串、对象、数组等可序列化的数据。

#### Web Worker 的使用场景

- **计算密集型任务**：例如大数据处理、加密解密、图像处理等需要大量 CPU 资源的任务。
- **复杂的算法**：如路径规划、排序算法等，在前端进行复杂计算时，使用 Web Worker 可以避免页面卡顿。
- **定时任务**：一些需要后台持续运行的任务，例如轮询请求等。
- **文件处理**：文件的大规模处理，如读取、写入或操作本地文件（结合 `File API`）等。

#### Web Worker 的限制

1. **不能访问 DOM**: Web Worker 无法直接访问主线程中的 DOM 元素或更新页面的内容。它只能处理数据，然后通过消息将结果发送回主线程，由主线程负责页面更新。
2. **无全局对象访问**：Worker 线程没有 `window`、`document`、`localStorage` 等全局对象的访问权限。但它可以使用 `navigator` 和 `location`（只读的）对象。
3. **受限于相同来源策略**：与主线程中的 JavaScript 代码一样，Web Worker 也受相同来源策略的限制，不能访问其他域名下的资源，除非通过 CORS。
4. **网络请求**：虽然 Worker 线程可以发起网络请求（如 `fetch` 或 `XMLHttpRequest`），但无法直接操作 `localStorage`、`sessionStorage` 或直接与浏览器的缓存交互。
5. **开销问题**：每个 Web Worker 都是一个独立的线程，启动和管理线程是有开销的。因此，创建大量 Web Worker 可能会导致性能下降。
