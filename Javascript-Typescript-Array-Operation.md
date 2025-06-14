---
title: Javascript / Typescript 数组操作
date: 2025-06-14 12:01:17
categories: 学习笔记
tags: 
- Javascript
- Typescript
---

这篇笔记包含了 JavaScript 内置的数据结构`Array` 的默认操作函数

<!-- more -->
<!-- toc -->

在 JavaScript 中，`Array` 是一种用于按顺序存储多个值的数据结构。每个值都有一个对应的索引（从 0 开始）。与对象相比，数组更适合用来处理有序列表或集合。

- 数组可以包含任意类型的数据：数字、字符串、对象、甚至其他数组（嵌套数组）
- 数组的长度可以动态变化
- 大量内置方法方便增删改查

## 创建数组

```javascript
const arr1 = [1, 2, 3];
const arr2 = new Array(1, 2, 3);
const arr3 = Array.from('hello'); // ['h', 'e', 'l', 'l', 'o']
const arr4 = Array(5).fill(0);    // [0, 0, 0, 0, 0]
```



## 本属性

| 名称     | 作用 | 示例         |
| -------- | ---- | ------------ |
| `length` | 长度 | `arr.length` |
| 索引访问 | 取值 | `arr[0]`     |



## 添加 / 删除元素

| 方法        | 作用                 | 示例                    |
| ----------- | -------------------- | ----------------------- |
| `push()`    | 末尾添加             | `arr.push(4)`           |
| `pop()`     | 末尾删除             | `arr.pop()`             |
| `unshift()` | 开头添加             | `arr.unshift(0)`        |
| `shift()`   | 开头删除             | `arr.shift()`           |
| `splice()`  | 插入/删除/替换       | `arr.splice(1, 2, 'a')` |
| `slice()`   | 截取（不改变原数组） | `arr.slice(1, 3)`       |



## 遍历

| 方法          | 作用               | 示例                               |
| ------------- | ------------------ | ---------------------------------- |
| `forEach()`   | 遍历               | `arr.forEach(x => console.log(x))` |
| `map()`       | 返回新数组         | `arr.map(x => x * 2)`              |
| `filter()`    | 筛选               | `arr.filter(x => x > 2)`           |
| `reduce()`    | 累积               | `arr.reduce((a,b) => a+b, 0)`      |
| `some()`      | 是否至少有一个符合 | `arr.some(x => x > 2)`             |
| `every()`     | 是否全部符合       | `arr.every(x => x > 2)`            |
| `find()`      | 找到第一个符合     | `arr.find(x => x > 2)`             |
| `findIndex()` | 找到第一个符合索引 | `arr.findIndex(x => x > 2)`        |



## 排序与变换

| 方法        | 作用         | 示例                           |
| ----------- | ------------ | ------------------------------ |
| `sort()`    | 排序         | `arr.sort((a,b) => a - b)`     |
| `reverse()` | 反转         | `arr.reverse()`                |
| `concat()`  | 合并         | `arr.concat([4,5])`            |
| `join()`    | 转字符串     | `arr.join(',')`                |
| `flat()`    | 扁平化       | `[1, [2, [3]]].flat(2)`        |
| `flatMap()` | 映射后扁平化 | `arr.flatMap(x => [x, x * 2])` |



## 搜索元素

| 方法            | 作用           | 示例                 |
| --------------- | -------------- | -------------------- |
| `indexOf()`     | 查找值的索引   | `arr.indexOf(2)`     |
| `lastIndexOf()` | 从后往前查索引 | `arr.lastIndexOf(2)` |
| `includes()`    | 是否包含       | `arr.includes(2)`    |



## 其他

| 方法              | 作用     | 示例                   |
| ----------------- | -------- | ---------------------- |
| `fill()`          | 填充     | `arr.fill(0, 1, 3)`    |
| `copyWithin()`    | 复制覆盖 | `arr.copyWithin(0, 2)` |
| `toString()`      | 转字符串 | `arr.toString()`       |
| `Array.isArray()` | 是否数组 | `Array.isArray(arr)`   |
