---
title: LeetCode Top 100 Liked 学习 - 1
date: 2025-05-14 10:04:39
categories: 学习笔记
tags: 
- LeetCode
---

This note includes multiple leetcode algorithm problems with my solution

<!-- more -->
<!-- toc -->

## 1. Letter Combinations of a Phone Number

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

<img src="https://assets.leetcode.com/uploads/2022/03/15/1200px-telephone-keypad2svg.png" alt="img" style="zoom:50%;" />


**Example 1:**

```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**Example 2:**

```
Input: digits = ""
Output: []
```

**Example 3:**

```
Input: digits = "2"
Output: ["a","b","c"]
```

**Constraints:**

- `0 <= digits.length <= 4`
- `digits[i]` is a digit in the range `['2', '9']`.

### My Solution

```javascript
/**
 * @param {string} digits
 * @return {string[]}
 */
var letterCombinations = function(digits) {
    if(digits === "") return [];
    let solution = [];
    const digitsArr = digits.split("");
    let map = {};
    map["2"] = ["a", "b", "c"];
    map["3"] = ["d", "e", "f"];
    map["4"] = ["g", "h", "i"];
    map["5"] = ["j", "k", "l"];
    map["6"] = ["m", "n", "o"];
    map["7"] = ["p", "q", "r", "s"];
    map["8"] = ["t", "u", "v"];
    map["9"] = ["w", "x", "y", "z"];

    function backTrace(index, path){
        if(path.length === digitsArr.length){  // finish to the leaf node
            solution.push(path);
            return;
        }

        let letters = map[digitsArr[index]]
        letters.forEach((letter)=>{
            backTrace(index + 1, path + letter);
        })        
    }

    backTrace(0, "");

    return solution
};
```


## 2. Generate Parentheses

Given `n` pairs of parentheses, write a function to *generate all combinations of well-formed parentheses*.

**Example 1:**

```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

**Example 2:**

```
Input: n = 1
Output: ["()"]
```

**Constraints:**

- `1 <= n <= 8`

### My Solution

```javascript
/**
 * @param {number} n
 * @return {string[]}
 */
var generateParenthesis = function(n) {
    let solution = []
    if(n === 0) return [];

    function BackTrace(left, right, comb){
        if(left === 0 && right === 0){
            solution.push(comb);
            return;
        }

        if(left > 0){
            BackTrace(left - 1, right, comb+'(');
        }

        if(right > left){
            BackTrace(left, right - 1, comb+')');
        }
    }

    BackTrace(n, n, "");

    return solution;
    
};
```

## 3. Combination Sum

Given an array of **distinct** integers `candidates` and a target integer `target`, return *a list of all **unique combinations** of* `candidates` *where the chosen numbers sum to* `target`*.* You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
```

**Example 3:**

```
Input: candidates = [2], target = 1
Output: []
```

**Constraints:**

- `1 <= candidates.length <= 30`
- `2 <= candidates[i] <= 40`
- All elements of `candidates` are **distinct**.
- `1 <= target <= 40`

### My Solution

```javascript
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
var combinationSum = function(candidates, target) {
    let solution = [];

    const BackTracking = (currentResult, sum, startIndex) => {
        if(sum === target){
            solution.push(currentResult);
            return;
        }

        if(sum > target){
            return;
        }

        for(let i = startIndex; i < candidates.length; i++){
            const value = candidates[i];
            if(!((value + sum) > target)){
                let newResult = [...currentResult, value];
                let newSum = sum + value;
                BackTracking(newResult, newSum, i);
            }
        }
    }

    BackTracking([], 0, 0);

    return solution;
};
```

> 优化空间: 
>
> - 可以先排序, 在`sum > target` 的时候就可以直接break, 可以更早终止无效分支。
>
> - 可以避免数组的不断复制（`[...currentResult, value]`）带来的额外空间开销。可以尝试用回溯的“回弹法”：
>
>   ```javascript
>   currentResult.push(value);
>   BackTracking(currentResult, sum + value, i);
>   currentResult.pop(); // 回弹
>   ```
