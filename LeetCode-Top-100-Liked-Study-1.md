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

## 8. Search Insert Position

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

```
Input: nums = [1,3,5,6], target = 5
Output: 2
```

**Example 2:**

```
Input: nums = [1,3,5,6], target = 2
Output: 1
```

**Example 3:**

```
Input: nums = [1,3,5,6], target = 7
Output: 4
```

**Constraints:**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` contains **distinct** values sorted in **ascending** order.
- `-104 <= target <= 104`

### My Solution

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var searchInsert = function(nums, target) {
    let left = 0;
    let right = nums.length - 1;


    if(target > nums[right]){
        return right + 1;
    }

    if(target < nums[left]){
        return 0;
    }

    while (left <= right) {
        let mid = Math.floor((left + right) / 2);

        if (nums[mid] === target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }

    return left;

    
};
```

## 9. Path Sum III

Given the `root` of a binary tree and an integer `targetSum`, return *the number of paths where the sum of the values along the path equals* `targetSum`.

The path does not need to start or end at the root or a leaf, but it must go downwards (i.e., traveling only from parent nodes to child nodes).

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/04/09/pathsum3-1-tree.jpg)

```
Input: root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
Output: 3
Explanation: The paths that sum to 8 are shown.
```

**Example 2:**

```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
Output: 3
```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 1000]`.
- `-109 <= Node.val <= 109`
- `-1000 <= targetSum <= 1000`

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
 * @param {number} targetSum
 * @return {number}
 */
var pathSum = function(root, targetSum) {
    const prefixSumCount = new Map();
    prefixSumCount.set(0, 1); // the empty path

    const dfs = (root, currSum) => {
        if(!root) return 0;
        currSum += root.val;
        let res = prefixSumCount.get(currSum - targetSum) || 0;

        if(prefixSumCount.get(currSum)){
            prefixSumCount.set(currSum, prefixSumCount.get(currSum) + 1);
        } else {
            prefixSumCount.set(currSum, 1);
        }

        res += dfs(root.left, currSum);
        res += dfs(root.right, currSum);

        prefixSumCount.set(currSum, prefixSumCount.get(currSum) - 1);

        return res;

    }

    return dfs(root, 0);
};
```



## 10. Construct Binary Tree from Preorder and Inorder Traversal

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return *the binary tree*.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```

**Example 2:**

```
Input: preorder = [-1], inorder = [-1]
Output: [-1]
```

**Constraints:**

- `1 <= preorder.length <= 3000`
- `inorder.length == preorder.length`
- `-3000 <= preorder[i], inorder[i] <= 3000`
- `preorder` and `inorder` consist of **unique** values.
- Each value of `inorder` also appears in `preorder`.
- `preorder` is **guaranteed** to be the preorder traversal of the tree.
- `inorder` is **guaranteed** to be the inorder traversal of the tree.

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
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */
var buildTree = function(preorder, inorder) {
    // 数组为空代表子树不存在
    if (preorder.length === 0) return null;
    
    // 前序遍历的第一个节点是根节点
    const rootVal = preorder[0];
    
    // 中序遍历的根节点左边是左子树, 右边是右子树
    const inorderRootIndex = inorder.indexOf(rootVal);
    const inorderLeft = inorder.slice(0, inorderRootIndex);
    const inorderRight = inorder.slice(inorderRootIndex + 1);
	
    // 去除第一位的前序遍历, 前中序遍历左子树一样长度的数组是前序遍历的左子树, 剩余为右子树
    const preorderLeft = preorder.slice(1, inorderRootIndex + 1);
    const preorderRight = preorder.slice(inorderRootIndex + 1);
	
    // 对左子树和右子树递归构建
    const root = new TreeNode(rootVal, buildTree(preorderLeft,inorderLeft),  buildTree(preorderRight, inorderRight));

    return root;
};
```



## 11. Convert Sorted Array to Binary Search Tree

Given an integer array `nums` where the elements are sorted in **ascending order**, convert *it to a* **height-balanced** *binary search tree*.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/18/btree1.jpg)

```
Input: nums = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: [0,-10,5,null,-3,null,9] is also accepted:
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/02/18/btree.jpg)

```
Input: nums = [1,3]
Output: [3,1]
Explanation: [1,null,3] and [3,1] are both height-balanced BSTs.
```

**Constraints:**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` is sorted in a **strictly increasing** order.

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
 * @param {number[]} nums
 * @return {TreeNode}
 */
var sortedArrayToBST = function(nums) {
    const build = (left, right)=>{
        if(left > right) return null;
        const rootValIndex = Math.floor((left + right) / 2);
        const newLeftInterval = [left, rootValIndex - 1]
        const newRightInterval = [rootValIndex + 1, right];

        const root = new TreeNode(
            nums[rootValIndex],
            build(newLeftInterval[0], newLeftInterval[1]),
            build(newRightInterval[0], newRightInterval[1])
            )

        return root;
    }

    return build(0, nums.length - 1);
    
};
```

## 12. Course Schedule

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

**Example 1:**

```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
```

**Example 2:**

```
Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
```

**Constraints:**

- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= 5000`
- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`
- All the pairs prerequisites[i] are **unique**.

### My Solution

```javascript
/**
 * @param {number} numCourses
 * @param {number[][]} prerequisites
 * @return {boolean}
 */
var canFinish = function(numCourses, prerequisites) {
    // 转换为邻接表
    const adjs = new Map();
    prerequisites.forEach((edge) => {
        let adj = adjs.get(edge[0]);
        if(!adj) {
            adjs.set(edge[0], [edge[1]]);
        } else {
            adj.push(edge[1]);
        }
    })

    // 使用dfs 遍历邻接表
    const visited = new Map();

    const dfs = (node)=>{
        if(visited.get(node) && visited.get(node) === 1){
            return true;
        } else {
            // 正在进行检查
            visited.set(node, 1);
        }
        let adjNodes = adjs.get(node);
        for(let child of (adjNodes || [])){
            // 如果一个节点已经确定了安全无环, 跳过不进行递归
            if(visited.get(child) === 2) continue;

            if(dfs(child)){
                return true;
            }
        }
        // 检查完毕, 安全无环
        visited.set(node, 2);


        return false;
    }

    let containCycle = false;
    for(let key of adjs.keys()){
        if(dfs(key)){
            containCycle = true;
            break;
        }
    }
    
    return !containCycle;
    
};
```

## 13. Word Search

Given an `m x n` grid of characters `board` and a string `word`, return `true` *if* `word` *exists in the grid*.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true
```

**Example 3:**

![img](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)

```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false
```

**Constraints:**

- `m == board.length`
- `n = board[i].length`
- `1 <= m, n <= 6`
- `1 <= word.length <= 15`
- `board` and `word` consists of only lowercase and uppercase English letters.

**Follow up:** Could you use search pruning to make your solution faster with a larger `board`?

### My Solution

```javascript
/**
 * @param {character[][]} board
 * @param {string} word
 * @return {boolean}
 */

const isValid = (x, y, visited, board) => {
    return (
        x >= 0 && 
        x < board[0].length &&
        y >= 0 && 
        y < board.length &&
        !visited[y][x]
    );
};

var exist = function(board, word) {
    let solution = false;
    const findPath = (inputLetterIndex, visited, currentCheckingCoord) => {
        if (solution) return; // 如果已经找到结果了可以跳过
        const x =currentCheckingCoord[0];
        const y = currentCheckingCoord[1];
        visited[y][x] = true;
        const currentLetter = board[y][x];
        if (currentLetter !== word[inputLetterIndex]) {
            visited[y][x] = false;  // 如果不符合可以提前回溯剪枝
            return;
        }
        if(currentLetter === word[inputLetterIndex]){
            if(inputLetterIndex === (word.length - 1)){
                solution = true;
                return;
            }
            const directions = [[1, 0], [-1, 0], [0, 1], [0, -1]];
            for(let [dx, dy] of directions){
                const newX = x + dx;
                const newY = y + dy;
                if(isValid(newX, newY, visited, board)){
                    findPath(inputLetterIndex + 1, visited, [newX, newY]);
                } 
            }
            visited[y][x] = false; // 回溯
        }
    }

    for(let y = 0; y < board.length; y++){
        for(let x = 0; x < board[0].length; x++){
            if (board[y][x] === word[0]) { // 只对满足第一个字母的起点进行搜索
                const visited = 
                Array.from({ length: board.length }, () => new Array(board[0].length).fill(false));
                findPath(0, visited, [x, y]);
            }
        }
    }

    return solution;
    
};


```

## 14. Search in Rotated Sorted Array

There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return *the index of* `target` *if it is in* `nums`*, or* `-1` *if it is not in* `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

**Example 3:**

```
Input: nums = [1], target = 0
Output: -1
```

**Constraints:**

- `1 <= nums.length <= 5000`
- `-104 <= nums[i] <= 104`
- All values of `nums` are **unique**.
- `nums` is an ascending array that is possibly rotated.
- `-104 <= target <= 104`

### My Solution

这道题的核心思路在于, 符合题意条件下, 如果只有一个拐点, 那么在二分的时候, 左右两边一定可以确认有一边是有序数组. 则可以根据这一边的有序数组来进行搜索 (因为有序数组任意截取也将继续为有序数组), 如果目标值不在有序数组中, 则对包含拐点的另一边重复上面的判断

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function(nums, target) {
    let left = 0;
    let right = nums.length - 1;

    while (left <= right){
        let mid = Math.floor(right + left / 2);
        if (nums[mid] === target) {
            return mid; // 找到目标
        }
        if (nums[left] <= nums[mid]) { // 左半边是有序的
            if(target < nums[mid] && target >= nums[left]){ // 在左半边
                right = mid - 1;
            } else { // 在右半边
                left = mid + 1;
            }
        } else {  // 右半边是有序的
            if(target > nums[mid] && target <= nums[right]){ // 在右半边
                left = mid + 1;
            } else { // 在右半边
                right = mid - 1;
            }
        }
    }

    return -1;
    
};
```



## 15. Invert Binary Tree

Given the `root` of a binary tree, invert the tree, and return *its root*.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

```
Input: root = [2,1,3]
Output: [2,3,1]
```

**Example 3:**

```
Input: root = []
Output: []
```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
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
 * @return {TreeNode}
 */
var invertTree = function(root) {
    const dfs = (node) => {
        if(!node) return;
        let tempNode = node.left;
        node.left = node.right;
        node.right = tempNode;
        dfs(node.left);
        dfs(node.right);
    };

    dfs(root);
    return root;
};
```

## 16. Flatten Binary Tree to Linked List

Given the `root` of a binary tree, flatten the tree into a "linked list":

- The "linked list" should use the same `TreeNode` class where the `right` child pointer points to the next node in the list and the `left` child pointer is always `null`.
- The "linked list" should be in the same order as a [**pre-order** **traversal**](https://en.wikipedia.org/wiki/Tree_traversal#Pre-order,_NLR) of the binary tree.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg)

```
Input: root = [1,2,5,3,4,null,6]
Output: [1,null,2,null,3,null,4,null,5,null,6]
```

**Example 2:**

```
Input: root = []
Output: []
```

**Example 3:**

```
Input: root = [0]
Output: [0]
```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-100 <= Node.val <= 100`

**Follow up:** Can you flatten the tree in-place (with `O(1)` extra space)?

### My Solution

**思路:** 

```
   1
  / \
 2   5
/ \   \
3  4   6
```

- `node = 1`，有左子树 `2`
  - 找到 `2` 的最右节点是 `4`
  - 把 `5` 接到 `4.right`
  - 把 `2` 挂到 `1.right`
  - `1.left = null`

此时：

```
1
 \
  2
 / \
3   4
      \
       5
        \
         6
```

- 接着 `node = 2`，重复一样操作..

**代码实现**

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
 * @return {void} Do not return anything, modify root in-place instead.
 */
var flatten = function(root) {

    let node = root;
    if(!node) return;
    
    while (node){
        // 如果当前节点存在左子树, 则需要将其挪到右边
        if(node.left){
            
            // 找到左子树中最靠右的节点
            let rightMost = node.left;
            while(rightMost.right){
                rightMost = rightMost.right;
            }
            
            // 将根的右子树挪到该最右点的右子树, 然后将整个左子树挪到右子树上
            rightMost.right = node.right;
            node.right = node.left;
            node.left = null;
        }
        // 一直向右检查, 直到尽头, 在这个过程中, 所有左子树都被挪到了右边
        node = node.right;
    }
    
};
```

## 17. Intersection of Two Linked Lists

Given the heads of two singly linked-lists `headA` and `headB`, return *the node at which the two lists intersect*. If the two linked lists have no intersection at all, return `null`.

For example, the following two linked lists begin to intersect at node `c1`:

![img](https://assets.leetcode.com/uploads/2021/03/05/160_statement.png)

The test cases are generated such that there are no cycles anywhere in the entire linked structure.

**Note** that the linked lists must **retain their original structure** after the function returns.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/05/160_example_1_1.png)

```
Input: listA = [4,1,8,4,5], listB = [5,6,1,8,4,5]
Output: Intersected at '8'
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/03/05/160_example_2.png)

```
Input: listA = [1,9,1,2,4], listB = [3,2,4]
Output: Intersected at '2'
```

**Example 3:**

![img](https://assets.leetcode.com/uploads/2021/03/05/160_example_3.png)

```
Input: listA = [2,6,4], listB = [1,5]
Output: null
```

**Constraints:**

- The number of nodes of `listA` is in the `m`.
- The number of nodes of `listB` is in the `n`.
- `1 <= m, n <= 3 * 104`
- `1 <= Node.val <= 105`

**Follow up:** Could you write a solution that runs in `O(m + n)` time and use only `O(1)` memory?

### My Solution

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} headA
 * @param {ListNode} headB
 * @return {ListNode}
 */
var getIntersectionNode = function(headA, headB) {
    if (!headA || !headB) return null;
    
    let pointerA = headA;
    let pointerB = headB;

    while (pointerA != pointerB){
        pointerA = pointerA? pointerA.next : headB;
        pointerB = pointerB? pointerB.next : headA;
    }

    return pointerA;
};
```

## 18. Reverse Linked List

Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```
Input: head = [1,2]
Output: [2,1]
```

**Example 3:**

```
Input: head = []
Output: []
```

**Constraints:**

- The number of nodes in the list is the range `[0, 5000]`.
- `-5000 <= Node.val <= 5000`

### My Solution

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function(head) {
    if(!head) return head;
    if(!head.next) return head;

    let front = head.next;
    let back = head;

    head.next = null;

    while (front){
        let next = front.next;
        front.next = back;
        back = front;
        front = next;
    }

    return back;

};
```

## 19. Add Two Numbers

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/02/addtwonumber1.jpg)

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```

**Example 2:**

```
Input: l1 = [0], l2 = [0]
Output: [0]
```

**Example 3:**

```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

**Constraints:**

- The number of nodes in each linked list is in the range `[1, 100]`.
- `0 <= Node.val <= 9`
- It is guaranteed that the list represents a number that does not have leading zeros.

### My Solution

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {

    let carry = 0;

    let p1 = l1;
    let p2 = l2;
    let head = new ListNode();
    let lastResult = head;

    while ( p1 || p2 || (carry != 0)){
        let p1Digit = p1 ? p1.val : 0;
        let p2Digit = p2 ? p2.val : 0;
        let digitSum = p1Digit + p2Digit + carry;
        let digit = digitSum % 10;
        carry = Math.floor(digitSum / 10);

        let newNode = new ListNode(digit);
        lastResult.next = newNode;

        p1 = p1? p1.next : null;
        p2 = p2? p2.next : null;
        lastResult = newNode;
    }

    return head.next;
    
};
```



## 20. Rotting Oranges

You are given an `m x n` `grid` where each cell can have one of three values:

- `0` representing an empty cell,
- `1` representing a fresh orange, or
- `2` representing a rotten orange.

Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

Return *the minimum number of minutes that must elapse until no cell has a fresh orange*. If *this is impossible, return* `-1`.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/02/16/oranges.png)

```
Input: grid = [[2,1,1],[1,1,0],[0,1,1]]
Output: 4
```

**Example 2:**

```
Input: grid = [[2,1,1],[0,1,1],[1,0,1]]
Output: -1
Explanation: The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.
```

**Example 3:**

```
Input: grid = [[0,2]]
Output: 0
Explanation: Since there are already no fresh oranges at minute 0, the answer is just 0 
```

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10`
- `grid[i][j]` is `0`, `1`, or `2`.

### My Solution

```javascript
/**
 * @param {number[][]} grid
 * @return {number}
 */
var orangesRotting = function(grid) {
    
    const m = grid.length; 
    const n = grid[0].length;
    let directions = [
        [1, 0],
        [0, 1],
        [-1, 0],
        [0, -1]
    ];

    let minutes = 0;
    let queue = [];
    let numsFresh = 0; // 剩余新鲜橙子的数量
	
    // 记录新鲜橙子的数量, 并且把所有腐烂橙子记录到待检查的队列中
    for(let i = 0; i < m; i++){
        for(let j = 0; j < n; j++){
            if(grid[i][j] === 2){
                queue.push([i, j]);
            }
            if(grid[i][j] === 1){
                numsFresh += 1;
            }
        }
    }
	
    // 判断bfs条件, 只有不越界并且是新鲜橙子才可以在bfs中传染
    const valid = (x, y) => {
        return (
            x >= 0 && x < n &&
            y >= 0 && y < m &&
            grid[y][x] === 1
        );
    }
	
    // bfs算法
    while (queue.length){
        
        let newQueue = [];
        queue.forEach(([y, x]) => {
            directions.forEach(([dy, dx]) => {
                let newX = x + dx;
                let newY = y + dy;
                if(valid(newX, newY)){
                    newQueue.push([newY, newX]);
                    grid[newY][newX] = 2;
                    numsFresh -= 1;
                }
            });
        });

        if(newQueue.length != 0){
            minutes += 1;
        }
        queue = newQueue;
    }
    	
    // 如果还有剩余新鲜的橙子, 返回-1
    return numsFresh === 0 ? minutes : -1;
};
```



## 21. Merge Two Sorted Lists

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*. 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

**Example 2:**

```
Input: list1 = [], list2 = []
Output: []
```

**Example 3:**

```
Input: list1 = [], list2 = [0]
Output: [0]
```

**Constraints:**

- The number of nodes in both lists is in the range `[0, 50]`.
- `-100 <= Node.val <= 100`
- Both `list1` and `list2` are sorted in **non-decreasing** order.

### My Solution

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} list1
 * @param {ListNode} list2
 * @return {ListNode}
 */
var mergeTwoLists = function(list1, list2) {

    let pointer1 = list1;
    let pointer2 = list2;

    let dummy = new ListNode();
    let curr = dummy;

    while (pointer1 && pointer2){
        if (pointer1.val < pointer2.val){
            curr.next = pointer1;
            pointer1 = pointer1.next;
        } else {
            curr.next = pointer2;
            pointer2 = pointer2.next;
        }

        curr = curr.next;
    }

    if(pointer1){
        curr.next = pointer1;
    } else if(pointer2){
        curr.next = pointer2;
    }

    return dummy.next;
};
```



## 22. Copy List with Random Pointer

A linked list of length `n` is given such that each node contains an additional random pointer, which could point to any node in the list, or `null`.

Construct a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) of the list. The deep copy should consist of exactly `n` **brand new** nodes, where each new node has its value set to the value of its corresponding original node. Both the `next` and `random` pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. **None of the pointers in the new list should point to nodes in the original list**.

For example, if there are two nodes `X` and `Y` in the original list, where `X.random --> Y`, then for the corresponding two nodes `x` and `y` in the copied list, `x.random --> y`.

Return *the head of the copied linked list*.

The linked list is represented in the input/output as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]` where:

- `val`: an integer representing `Node.val`
- `random_index`: the index of the node (range from `0` to `n-1`) that the `random` pointer points to, or `null` if it does not point to any node.

Your code will **only** be given the `head` of the original linked list.

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/12/18/e1.png)

```
Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2019/12/18/e2.png)

```
Input: head = [[1,1],[2,1]]
Output: [[1,1],[2,1]]
```

**Example 3:**

**![img](https://assets.leetcode.com/uploads/2019/12/18/e3.png)**

```
Input: head = [[3,null],[3,0],[3,null]]
Output: [[3,null],[3,0],[3,null]]
```

**Constraints:**

- `0 <= n <= 1000`
- `-104 <= Node.val <= 104`
- `Node.random` is `null` or is pointing to some node in the linked list.

### My Solution

```javascript
/**
 * // Definition for a _Node.
 * function _Node(val, next, random) {
 *    this.val = val;
 *    this.next = next;
 *    this.random = random;
 * };
 */

/**
 * @param {_Node} head
 * @return {_Node}
 */
var copyRandomList = function(head) {
    let nodeMap = new Map(); // 老node : 新node

    let pointer = head;
    while (pointer) {
        nodeMap.set(pointer, new _Node(pointer.val));
        pointer = pointer.next;
    }
    let curr = head;
    while(curr){
        let newNode = nodeMap.get(curr);
        newNode.next = curr.next ? nodeMap.get(curr.next) : null;
        newNode.random = curr.random ? nodeMap.get(curr.random) : null;

        curr = curr.next;
    }

    return nodeMap.get(head);
    
};
```

