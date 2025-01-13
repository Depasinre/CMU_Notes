---
title: 10-31 Coding Gym
date: 2024-10-31 18:15:56
categories: 学习笔记
tags: 
- Interview Preparation
- LeetCode
---

This note includes multiple leetcode algorithm problems we discuss in Coding Gym at 10/31, including topic: 

- Stack & Queue
- Monotonicity
- Tree
- Heap

<!-- more -->
<!-- toc -->

## 1. Min Stack

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:

- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element `val` onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

 

**Example 1:**

```
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
```

 

**Constraints:**

- `-231 <= val <= 231 - 1`
- Methods `pop`, `top` and `getMin` operations will always be called on **non-empty** stacks.
- At most `3 * 104` calls will be made to `push`, `pop`, `top`, and `getMin`.

### My Solution

```javascript
var MinStack = function() {
    this.stack = [];
    this.min = Infinity;
    
};

/** 
 * @param {number} val
 * @return {void}
 */
MinStack.prototype.push = function(val) {
    let obj = {val: val, currMin: null};
    this.min = Math.min(this.min, val);
    obj.currMin = this.min;
    this.stack.push(obj); 
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function() {
    let obj = this.stack.pop();
    if(this.min === obj.val){
        if(this.stack.length === 0){
            this.min = Infinity;
        } else {
            this.min = this.stack[this.stack.length - 1].currMin;
        }
        
    }

    return obj.val;
    
};

/**
 * @return {number}
 */
MinStack.prototype.top = function() {
    return this.stack[this.stack.length - 1].val;
    
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function() {
    return this.min;
    
};

/** 
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(val)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.getMin()
 */
```



## 2. Next Greater Element I

The **next greater element** of some element `x` in an array is the **first greater** element that is **to the right** of `x` in the same array.

You are given two **distinct 0-indexed** integer arrays `nums1` and `nums2`, where `nums1` is a subset of `nums2`.

For each `0 <= i < nums1.length`, find the index `j` such that `nums1[i] == nums2[j]` and determine the **next greater element** of `nums2[j]` in `nums2`. If there is no next greater element, then the answer for this query is `-1`.

Return *an array* `ans` *of length* `nums1.length` *such that* `ans[i]` *is the **next greater element** as described above.*

 

**Example 1:**

```
Input: nums1 = [4,1,2], nums2 = [1,3,4,2]
Output: [-1,3,-1]
Explanation: The next greater element for each value of nums1 is as follows:
- 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
- 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.
- 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
```

**Example 2:**

```
Input: nums1 = [2,4], nums2 = [1,2,3,4]
Output: [3,-1]
Explanation: The next greater element for each value of nums1 is as follows:
- 2 is underlined in nums2 = [1,2,3,4]. The next greater element is 3.
- 4 is underlined in nums2 = [1,2,3,4]. There is no next greater element, so the answer is -1.
```

 

**Constraints:**

- `1 <= nums1.length <= nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 104`
- All integers in `nums1` and `nums2` are **unique**.
- All the integers of `nums1` also appear in `nums2`.

 

**Follow up:** Could you find an `O(nums1.length + nums2.length)` solution?

### My Solution

```javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var nextGreaterElement = function(nums1, nums2) {
    let monoStack = [];

    let result = new Map();

    nums1.forEach((value) => result.set(value, -1));

    for(let i = 0; i < nums2.length; i++){
        while(monoStack.length > 0 && nums2[i] > monoStack[monoStack.length - 1]){
            let number = monoStack.pop();
            result.set(number, nums2[i]);
        }
        if(result.has(nums2[i])){
            monoStack.push(nums2[i]);
        }

    }

    let resultArray = [];

    for(let i = 0; i < nums1.length; i++){
        resultArray.push(result.get(nums1[i]));
    }

    return resultArray;
    
};
```

## 3. Daily Temperatures

Given an array of integers `temperatures` represents the daily temperatures, return *an array* `answer` *such that* `answer[i]` *is the number of days you have to wait after the* `ith` *day to get a warmer temperature*. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

 

**Example 1:**

```
Input: temperatures = [73,74,75,71,69,72,76,73]
Output: [1,1,4,2,1,1,0,0]
```

**Example 2:**

```
Input: temperatures = [30,40,50,60]
Output: [1,1,1,0]
```

**Example 3:**

```
Input: temperatures = [30,60,90]
Output: [1,1,0]
```

 

**Constraints:**

- `1 <= temperatures.length <= 105`
- `30 <= temperatures[i] <= 100`

### My Solution

```javascript
/**
 * @param {number[]} temperatures
 * @return {number[]}
 */
var dailyTemperatures = function(temperatures) {
    let stack = [];
    let result = new Array(temperatures.length).fill(0);

    for(let i = 0; i < temperatures.length; i++){
        while(stack.length > 0 && temperatures[i] > stack[stack.length - 1].temp){
            let obj = stack.pop();
            let distance = i - obj.index;
            result[obj.index] = distance;
        }
        stack.push({temp: temperatures[i], index: i});
    }
    return result;
};
```



