---
title: JavaScript / Typescript 哈希表操作
date: 2025-06-14 12:00:55
categories: 学习笔记
tags: 
- JavaScript
- TypeScript
---

这篇笔记包含了 JavaScript 内置的数据结构`Map` 的默认操作函数

<!-- more -->
<!-- toc -->

在 JavaScript 中，`Map` 是一种用于存储键值对的数据结构，和普通对象（`Object`）相比，它具有以下优势：

- 任意类型的键（对象、函数、基本类型）都可以用作键。
- 保留插入顺序。
- 内置多种操作方法。

## 创建 Map

```javascript
const map = new Map();
const map2 = new Map([['key1', 'value1'], ['key2', 'value2']]);
```



## 核心方法

| 方法               | 作用             | 例子                   |
| ------------------ | ---------------- | ---------------------- |
| `.set(key, value)` | 添加或更新键值对 | `map.set('a', 1)`      |
| `.get(key)`        | 获取值           | `map.get('a') // 1`    |
| `.has(key)`        | 判断是否有此键   | `map.has('a') // true` |
| `.delete(key)`     | 删除此键         | `map.delete('a')`      |
| `.clear()`         | 清空所有         | `map.clear()`          |
| `.size`            | 获取数量         | `map.size`             |



## 遍历方法

| 方法         | 作用              | 例子                                   |
| ------------ | ----------------- | -------------------------------------- |
| `.keys()`    | 返回所有键        | `for (let key of map.keys()) {}`       |
| `.values()`  | 返回所有值        | `for (let value of map.values()) {}`   |
| `.entries()` | 返回 [key, value] | `for (let [k, v] of map.entries()) {}` |
| `.forEach()` | 回调遍历          | `map.forEach((v, k) => {})`            |



## 小技巧

- Map 保持插入顺序
- Map 键可以是任何类型（对象、函数、NaN、undefined 都行）

> **使用对象或引用类型作为键**
>
> ```javascript
> const objKey = { id: 1 };
> map.set(objKey, 'hello');
> console.log(map.get(objKey)); // 'hello'
> ```

- 用 `Object.fromEntries(map)` 可以把 Map 转为普通对象

```javascript
const obj = Object.fromEntries(map);
```

