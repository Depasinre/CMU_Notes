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

## 4. Kth Smallest Element in a BST

Given the `root` of a binary search tree, and an integer `k`, return *the* `kth` *smallest value (**1-indexed**) of all the values of the nodes in the tree*.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg)

```
Input: root = [3,1,4,null,2], k = 1
Output: 1
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/01/28/kthtree2.jpg)

```
Input: root = [5,3,6,2,4,null,null,1], k = 3
Output: 3
```

**Constraints:**

- The number of nodes in the tree is `n`.
- `1 <= k <= n <= 104`
- `0 <= Node.val <= 104`

**Follow up:** If the BST is modified often (i.e., we can do insert and delete operations) and you need to find the kth smallest frequently, how would you optimize?

### My Solution

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} k
 * @return {number}
 */
var kthSmallest = function(root, k) {
    let stack = [];

    const traversal = (curr) => {
        if(!curr) return;

        traversal(curr.left);
        stack.push(curr.val);
        traversal(curr.right);
    }

    traversal(root);

    return stack[k - 1];
};
```

## 5. Symmetric Tree

Given the `root` of a binary tree, *check whether it is a mirror of itself* (i.e., symmetric around its center).

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)

```
Input: root = [1,2,2,3,4,4,3]
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg)

```
Input: root = [1,2,2,null,3,null,3]
Output: false
```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 1000]`.
- `-100 <= Node.val <= 100`

### My Solution

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
var isSymmetric = function(root) {
    const isMirror = (leftTree, rightTree) => {
        if(!leftTree && !rightTree) return true;
        if(!leftTree || !rightTree) return false;
        if(leftTree.val !== rightTree.val) return false;

        return isMirror(leftTree.left, rightTree.right) && isMirror(leftTree.right, rightTree.left)
    }
    
    return isMirror(root.left, root.right);
};
```

## 6. Binary Tree Level Order Traversal

Given the `root` of a binary tree, return *the level order traversal of its nodes' values*. (i.e., from left to right, level by level).

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
```

**Example 2:**

```
Input: root = [1]
Output: [[1]]
```

**Example 3:**

```
Input: root = []
Output: []
```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-1000 <= Node.val <= 1000`

### My Solution

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[][]}
 */
var levelOrder = function(root) {
    if(!root) return [];
    let result = [];
    let bfs = (root) => {
        let nodes = [root];

        while(nodes.length > 0){
            let nextLevel = [];
            let levelValue = [];

            nodes.forEach((node) => {
                levelValue.push(node.val);
                if(node.left) nextLevel.push(node.left);
                if(node.right) nextLevel.push(node.right);
            })

            nodes = nextLevel;
            result.push(levelValue);
        }
    }

    bfs(root);

    return result;
    
};
```

## 7. Permutations

Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in **any order**.

**Example 1:**

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**Example 2:**

```
Input: nums = [0,1]
Output: [[0,1],[1,0]]
```

**Example 3:**

```
Input: nums = [1]
Output: [[1]]
```

**Constraints:**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- All the integers of `nums` are **unique**.

### My Solution

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var permute = function(nums) {
    let solution = [];

    const backTrack = (remain, path) => {
        if(remain.length === 0){ 
            solution.push(path);
            return;
        }

        for(let i = 0; i < remain.length; i++){
            let newRemain = remain.slice(); // 避免使用引用拷贝
            let newPath = path.slice(); 
            newPath.push(remain[i]);
            newRemain.splice(i, 1);
            backTrack(newRemain, newPath);
        }
    }

    backTrack(nums, []);

    return solution;
    
};
```

### Optimized Solution

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var permute = function(nums) {
    let solution = [];

    const backTrack = (used, path) => {
        if(path.length === nums.length){
            solution.push([...path]); // 注意拷贝问题
            return;
        }

        for(let i = 0; i < nums.length; i++){
            if(used[i] === 1) continue;
            used[i] = 1;
            path.push(nums[i]);
            backTrack(used, path);
            used[i] = 0;
            path.pop();
        }

    }

    backTrack(new Array(nums.length).fill(0), []);

    return solution;
    
};
```

