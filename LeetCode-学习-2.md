---
title: LeetCode 学习 - 2
date: 2024-10-02 19:59:21
categories: 学习笔记
tags: 
- Interview Preparation
- LeetCode
---

This note includes multiple leetcode algorithm problems with my solution

<!-- more -->
<!-- toc -->

## 1. Swap Nodes in Pairs

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

**Example 1:**

​	**Input:** head = [1,2,3,4]

​	**Output:** [2,1,4,3]

**Explanation:**

![img](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

**Example 2:**

​	**Input:** head = []

​	**Output:** []

**Example 3:**

​	**Input:** head = [1]

​	**Output:** [1]

**Example 4:**

​	**Input:** head = [1,2,3]

​	**Output:** [2,1,3]

 

**Constraints:**

- The number of nodes in the list is in the range `[0, 100]`.
- `0 <= Node.val <= 100`

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
var swapPairs = function(head) {
    if(head === null){
        return head;
    }

    if (head.next === null){
        return head;
    }
    
    let result = head.next;

    let prev = head;
    let curr = head.next;

    let next1 = null;
    let next2 = null;

    while(curr && curr.next){      
        next1 = curr.next;
        next2 = curr.next.next;
        curr.next = prev;
        if(next1 && next2){
            prev.next = next2;
            curr = next2;
            prev = next1;
        } else if (next1){
            prev.next = next1;
            curr = next2;
            prev = next1;
        }
    }

    if(curr){
        curr.next = prev;
        prev.next = null;
    }



    return result;
};
```

### Solution With More Clear Logic

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
var swapPairs = function(head) {
    // Check edge case: linked list has 0 or 1 nodes, just return
    if (!head || !head.next) {
        return head;
    }
    
    let dummy = head.next;              // Step 5
    let prev = null;                    // Initialize for step 3
    while (head && head.next) {
        if (prev) {
            prev.next = head.next;      // Step 4
        }
        prev = head;                    // Step 3
        
        let nextNode = head.next.next;  // Step 2
        head.next.next = head;          // Step 1
        
        head.next = nextNode;           // Step 6
        head = nextNode;                // Move to next pair (Step 3)szzzzz
    }
    
    return dummy;
};
```

## 2.  Reverse Linked List II

Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return *the reversed list*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)

```
Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]
```

**Example 2:**

```
Input: head = [5], left = 1, right = 1
Output: [5]
```

 

**Constraints:**

- The number of nodes in the list is `n`.
- `1 <= n <= 500`
- `-500 <= Node.val <= 500`
- `1 <= left <= right <= n`

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
 * @param {number} left
 * @param {number} right
 * @return {ListNode}
 */
var reverseBetween = function(head, left, right) {
    if (left === right) {
        return head;
    }

    let dummy = new ListNode(0);
    dummy.next = head;
    let prev = dummy;

    // 移动 prev 指针到 left 位置的前一个节点
    for (let i = 1; i < left; i++) {
        prev = prev.next;
    }

    // curr 指向要翻转的子链表的第一个节点
    let curr = prev.next;

    // 开始翻转子链表
    for (let i = 0; i < right - left; i++) {
        let nextNode = curr.next;
        curr.next = nextNode.next;
        nextNode.next = prev.next;
        prev.next = nextNode;
    }

    return dummy.next;
};

```

## 3. Remove Nth Node From End of List

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

**Example 2:**

```
Input: head = [1], n = 1
Output: []
```

**Example 3:**

```
Input: head = [1,2], n = 1
Output: [1]
```

 

**Constraints:**

- The number of nodes in the list is `sz`.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

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
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {

    let dummy = new ListNode(0, head);
    let fast = dummy;
    let slow = dummy;

    for (let i = 0; i < n; i++){
        fast = fast.next;
    }

    while(fast.next){
        fast= fast.next;
        slow = slow.next;
    }

    if(slow.next){
        slow.next = slow.next.next;
    } else {
        slow.next = null;
    }
    return dummy.next;
    
};
```

## 4. Valid Parentheses

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type. 

**Example 1:**

​	**Input:** s = "()"

​	**Output:** true

**Example 2:**

​	**Input:** s = "()[]{}"

​	**Output:** true

**Example 3:**

​	**Input:** s = "(]"

​	**Output:** false

**Example 4:**

​	**Input:** s = "([])"

​	**Output:** true 

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of parentheses only `'()[]{}'`.

### My Solution: 

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
    let stack = [];

    for(let i = 0; i < s.length; i++){
        let top = stack[stack.length - 1];
        if(s[i] === ')'){
            if(top === '('){
                stack.pop();
            } else {
                return false;
            }
        } else if(s[i] === ']'){
            if(top === '['){
                stack.pop();
            } else {
                return false;
            }
        } else if (s[i] === '}'){
            if(top === '{'){
                stack.pop();
            } else {
                return false;
            }
        } else {
            stack.push(s[i]);
        }        
    }

    if (stack.length != 0){
        return false;
    }
    return true; 
};
```

### Solution with more clear code with JavaScript Object

```javascript
var isValid = function(s) {
    let stack = [];
    let map = {
        ')': '(',
        ']': '[',
        '}': '{'
    };

    for(let i = 0; i < s.length; i++){
        if (s[i] in map) {  // 如果是右括号
            let top = stack.length === 0 ? '#' : stack.pop(); // 栈空时设置默认值
            if (top !== map[s[i]]) {
                return false;
            }
        } else {
            stack.push(s[i]);  // 左括号直接入栈
        }
    }

    return stack.length === 0;  // 检查栈是否为空
};
```

##  5. Remove All Adjacent Duplicates In String

You are given a string `s` consisting of lowercase English letters. A **duplicate removal** consists of choosing two **adjacent** and **equal** letters and removing them.

We repeatedly make **duplicate removals** on `s` until we no longer can.

Return *the final string after all such duplicate removals have been made*. It can be proven that the answer is **unique**.

**Example 1:**

```
Input: s = "abbaca"
Output: "ca"
Explanation: 
For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".
```

**Example 2:**

```
Input: s = "azxxzy"
Output: "ay"
```

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of lowercase English letters.

### My Solution

```javascript
/**
 * @param {string} s
 * @return {string}
 */
var removeDuplicates = function(s) {
    let stack = []
    for(let char of s){
        if (char === stack[stack.length - 1]){
            stack.pop();
        } else {
            stack.push(char);
        }
    }

    return stack.join('');
    
};
```

##  6. Backspace String Compare

Given two strings `s` and `t`, return `true` *if they are equal when both are typed into empty text editors*. `'#'` means a backspace character.

Note that after backspacing an empty text, the text will continue empty.

 

**Example 1:**

```
Input: s = "ab#c", t = "ad#c"
Output: true
Explanation: Both s and t become "ac".
```

**Example 2:**

```
Input: s = "ab##", t = "c#d#"
Output: true
Explanation: Both s and t become "".
```

**Example 3:**

```
Input: s = "a#c", t = "b"
Output: false
Explanation: s becomes "c" while t becomes "b".
```

 

**Constraints:**

- `1 <= s.length, t.length <= 200`
- `s` and `t` only contain lowercase letters and `'#'` characters.

### My Solution

```javascript
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var backspaceCompare = function(s, t) {
    let stackS = [];
    let stackT = [];
    let length = Math.max(s.length, t.length);

    for (let i = 0; i < length; i++){
        if(s[i]){
            if(s[i] === '#'){
                stackS.pop();
            } else {
                stackS.push(s[i]);
            }
        }

        if(t[i]){
            if(t[i] ==='#'){
                stackT.pop();
            } else {
                stackT.push(t[i]);
            }
        }
    }

    if(stackS.length != stackT.length){
        return false;
    }

    for(let j = 0; j < stackS.length; j++){
        if(stackS[j] != stackT[j]){
            return false;
        }
    }

    return true;
    
};
```

### Better Space Complexity Solution with Two Pointer

 ```javascript
 var backspaceCompare = function(s, t) {
     let i = s.length - 1;
     let j = t.length - 1;
     
     let skipS = 0, skipT = 0;
 
     while (i >= 0 || j >= 0) {
         // 处理 s 中的退格符
         while (i >= 0) {
             if (s[i] === '#') {
                 skipS++;
                 i--;
             } else if (skipS > 0) {
                 skipS--;
                 i--;
             } else {
                 break;
             }
         }
 
         // 处理 t 中的退格符
         while (j >= 0) {
             if (t[j] === '#') {
                 skipT++;
                 j--;
             } else if (skipT > 0) {
                 skipT--;
                 j--;
             } else {
                 break;
             }
         }
 
         // 比较有效字符
         if (i >= 0 && j >= 0) {
             if (s[i] !== t[j]) {
                 return false;
             }
         } else {
             if (i >= 0 || j >= 0) {
                 return false;
             }
         }
         i--;
         j--;
     }
     
     return true;
 };
 ```

## 7. Simplify Path

You are given an *absolute* path for a Unix-style file system, which always begins with a slash `'/'`. Your task is to transform this absolute path into its **simplified canonical path**.

The *rules* of a Unix-style file system are as follows:

- A single period `'.'` represents the current directory.
- A double period `'..'` represents the previous/parent directory.
- Multiple consecutive slashes such as `'//'` and `'///'` are treated as a single slash `'/'`.
- Any sequence of periods that does **not match** the rules above should be treated as a **valid directory or** **file** **name**. For example, `'...' `and `'....'` are valid directory or file names.

The simplified canonical path should follow these *rules*:

- The path must start with a single slash `'/'`.
- Directories within the path must be separated by exactly one slash `'/'`.
- The path must not end with a slash `'/'`, unless it is the root directory.
- The path must not have any single or double periods (`'.'` and `'..'`) used to denote current or parent directories.

Return the **simplified canonical path**.

**Example 1:**

​	**Input:** path = "/home/"

​	**Output:** "/home"

​	**Explanation:**

​	The trailing slash should be removed.

**Example 2:**

​	**Input:** path = "/home//foo/"

​	**Output:** "/home/foo"

​	**Explanation:**

​	Multiple consecutive slashes are replaced by a single one.

**Example 3:**

​	**Input:** path = "/home/user/Documents/../Pictures"

​	**Output:** "/home/user/Pictures"

​	**Explanation:**

​	A double period `".."` refers to the directory up a level (the parent directory).

**Example 4:**

​	**Input:** path = "/../"

​	**Output:** "/"

​	**Explanation:**

​	Going one level up from the root directory is not possible.

**Example 5:**

​	**Input:** path = "/.../a/../b/c/../d/./"

​	**Output:** "/.../b/d"

​	**Explanation:**

​	`"..."` is a valid name for a directory in this problem.

**Constraints:**

- `1 <= path.length <= 3000`
- `path` consists of English letters, digits, period `'.'`, slash `'/'` or `'_'`.
- `path` is a valid absolute Unix path.

### My Solution

```javascript
/**
 * @param {string} path
 * @return {string}
 */
var simplifyPath = function(path) {
    let array = path.split('/');
    let resultStack = [];
    
    for(let i = 0; i < array.length; i++){
        switch(array[i]){
            case '.':
            case '':
            case ' ':
                break;
            case '..':
                resultStack.pop();
                break;
            default:
                resultStack.push(array[i]);
        }
        
    }
    
    let result = '/' + resultStack.join('/');
    
    return result;
};
```

## 8. Make The String Great

Given a string `s` of lower and upper case English letters.

A good string is a string which doesn't have **two adjacent characters** `s[i]` and `s[i + 1]` where:

- `0 <= i <= s.length - 2`
- `s[i]` is a lower-case letter and `s[i + 1]` is the same letter but in upper-case or **vice-versa**.

To make the string good, you can choose **two adjacent** characters that make the string bad and remove them. You can keep doing this until the string becomes good.

Return *the string* after making it good. The answer is guaranteed to be unique under the given constraints.

**Notice** that an empty string is also good.

 

**Example 1:**

```
Input: s = "leEeetcode"
Output: "leetcode"
Explanation: In the first step, either you choose i = 1 or i = 2, both will result "leEeetcode" to be reduced to "leetcode".
```

**Example 2:**

```
Input: s = "abBAcC"
Output: ""
Explanation: We have many possible scenarios, and all lead to the same answer. For example:
"abBAcC" --> "aAcC" --> "cC" --> ""
"abBAcC" --> "abBA" --> "aA" --> ""
```

**Example 3:**

```
Input: s = "s"
Output: "s"
```

**Constraints:**

- `1 <= s.length <= 100`
- `s` contains only lower and upper case English letters.

### My Solution

```javascript
/**
 * @param {string} s
 * @return {string}
 */
var makeGood = function(s) {
    let stack = [];
    
    for (let char of s){
        if(stack.length === 0){
            stack.push(char);
            continue;
        }
        if((char.toUpperCase() === stack[stack.length - 1].toUpperCase()) && (char != stack[stack.length - 1])){
            stack.pop();            
        } else {
            stack.push(char);
        }
    }
    
    
    return stack.join('');
};
```

## 9.  Moving Average from Data Stream

Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.

Implement the `MovingAverage` class:

- `MovingAverage(int size)` Initializes the object with the size of the window `size`.
- `double next(int val)` Returns the moving average of the last `size` values of the stream.

 

**Example 1:**

```
Input
["MovingAverage", "next", "next", "next", "next"]
[[3], [1], [10], [3], [5]]
Output
[null, 1.0, 5.5, 4.66667, 6.0]

Explanation
MovingAverage movingAverage = new MovingAverage(3);
movingAverage.next(1); // return 1.0 = 1 / 1
movingAverage.next(10); // return 5.5 = (1 + 10) / 2
movingAverage.next(3); // return 4.66667 = (1 + 10 + 3) / 3
movingAverage.next(5); // return 6.0 = (10 + 3 + 5) / 3
```

 

**Constraints:**

- `1 <= size <= 1000`
- `-105 <= val <= 105`
- At most `104` calls will be made to `next`.

### My Solution

```javascript
/**
 * @param {number} size
 */
var MovingAverage = function(size) {
    this.queue = [];
    this.currentSum = 0;
    this.size = size;
    this.currentSize = 0;
};

/** 
 * @param {number} val
 * @return {number}
 */
MovingAverage.prototype.next = function(val) {
    if(this.currentSize < this.size){
        this.queue.push(val);
        this.currentSum += val;
        this.currentSize += 1;
    } else {
        this.currentSum -= this.queue.shift();
        this.queue.push(val);
        this.currentSum += val;
    }
    
    return this.currentSum / this.currentSize;
};

/** 
 * Your MovingAverage object will be instantiated and called as such:
 * var obj = new MovingAverage(size)
 * var param_1 = obj.next(val)
 */
```

## 10. Contiguous Array

Given a binary array `nums`, return *the maximum length of a contiguous subarray with an equal number of* `0` *and* `1`.

 

**Example 1:**

```
Input: nums = [0,1]
Output: 2
Explanation: [0, 1] is the longest contiguous subarray with an equal number of 0 and 1.
```

**Example 2:**

```
Input: nums = [0,1,0]
Output: 2
Explanation: [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.
```

 

**Constraints:**

- `1 <= nums.length <= 105`
- `nums[i]` is either `0` or `1`.

### My Solution

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var findMaxLength = function(nums) {
    let map = new Map();
    map.set(0, -1); // 处理从索引 0 开始的子数组
    let maxLength = 0;
    let runningSum = 0;

    for (let i = 0; i < nums.length; i++) {
        runningSum += nums[i] === 1 ? 1 : -1;

        if (map.has(runningSum)) {
            // 如果当前累加和已出现，计算子数组的长度
            maxLength = Math.max(maxLength, i - map.get(runningSum));
        } else {
            // 记录当前累加和首次出现的位置
            map.set(runningSum, i);
        }
    }
    
    return maxLength;
};
```



### 问题的核心

我们需要在一个二进制数组中，找到一个子数组，使得其中的 `0` 和 `1` 的数量相等。传统的暴力解法是通过双重循环检查每个子数组，并统计其中的 `0` 和 `1`，这种做法的时间复杂度为 O(n²)，在数组较大时会导致超时。

### 优化思路的核心：累加和法

1. **将问题转化为累加和问题**：
   - 我们可以将 `0` 视为 `-1`，将 `1` 仍然视为 `1`。这样，每当数组中某个子数组的累加和为 `0` 时，就意味着这个子数组中 `0` 和 `1` 的数量是相等的。
   - 比如数组 `[0, 1]` 可以转换为 `[-1, 1]`，其累加和为 `0`，所以 `0` 和 `1` 的数量相等。
2. **使用哈希表来记录累加和的位置**：
   - 我们可以使用一个哈希表来存储每个累加和第一次出现的位置。如果某个累加和再次出现，则表示从上次该累加和出现的位置到当前索引的子数组和为 `0`，即该子数组有相等数量的 `0` 和 `1`。
   - 例如：假设累加和在索引 `i` 处为 `5`，而在索引 `j` 处再次为 `5`，那么从索引 `i+1` 到 `j` 的子数组的累加和为 `0`，即等数量的 `0` 和 `1`。
3. **初始化哈希表**：
   - 我们将累加和 `0` 初始化为 `-1`，这是为了处理从数组开头就满足条件的情况。例如，当整个数组从头开始就有相等数量的 `0` 和 `1` 时（即累加和为 `0`），此时子数组从索引 `0` 开始。

### 具体实现步骤

1. **初始化变量**：
   - 使用 `runningSum` 记录当前的累加和。
   - 使用 `map` 作为哈希表来存储每个累加和首次出现的索引。
   - 初始化 `maxLength` 为 0，用来存储最大子数组的长度。
2. **遍历数组**：
   - 遍历数组中的每一个元素：
     - 如果当前元素为 `1`，将 `runningSum` 加 `1`。
     - 如果当前元素为 `0`，将 `runningSum` 减 `1`（因为我们将 `0` 视为 `-1`）。
   - 对每个元素，检查当前的 `runningSum`是否已经在哈希表中存在：
     - 如果存在，说明从上次出现该累加和的位置到当前位置的子数组的累加和为 `0`，即该子数组中的 `0` 和 `1` 数量相等。我们计算该子数组的长度，并更新 `maxLength`。
     - 如果不存在，将当前 `runningSum` 及其对应的索引存入哈希表，表示这个累加和首次出现。
3. **返回结果**：
   - 最后返回 `maxLength`，即找到的最长子数组的长度。

## 11. Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/12/ex_depth.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 2
```

**Example 2:**

```
Input: root = [2,null,3,null,4,null,5,null,6]
Output: 5
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 105]`.
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
 * @return {number}
 */
var minDepth = function(root) {
    
    if(root === null){
        return 0;
    }
    
    let left = minDepth(root.left);
    let right = minDepth(root.right);
    
    if(left === 0){
        return right + 1;
    }
    
    if (right === 0){
        return left + 1;
        
    }
    return Math.min(left, right) + 1
};
```

### Better Logic Solution

```javascript
var minDepth = function(root) {
    // 如果节点为空，返回 0
    if (root === null) {
        return 0;
    }

    // 如果左子树为空，递归计算右子树的深度
    if (root.left === null) {
        return minDepth(root.right) + 1;
    }

    // 如果右子树为空，递归计算左子树的深度
    if (root.right === null) {
        return minDepth(root.left) + 1;
    }

    // 如果左右子树都存在，取两者的最小深度并加 1
    return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
};

```



## 12. Maximum Depth of Binary Tree

Given the `root` of a binary tree, return *its maximum depth*.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

**Example 2:**

```
Input: root = [1,null,2]
Output: 2
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
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
 * @return {number}
 */
var maxDepth = function(root) {
    if(root === null){
        return 0;
    }

    let left = maxDepth(root.left);
    let right = maxDepth(root.right);

    return Math.max(left, right) + 1; 
};
```

## 13. Maximum Difference Between Node and Ancestor

Given the `root` of a binary tree, find the maximum value `v` for which there exist **different** nodes `a` and `b` where `v = |a.val - b.val|` and `a` is an ancestor of `b`.

A node `a` is an ancestor of `b` if either: any child of `a` is equal to `b` or any child of `a` is an ancestor of `b`.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/09/tmp-tree.jpg)

```
Input: root = [8,3,10,1,6,null,14,null,null,4,7,13]
Output: 7
Explanation: We have various ancestor-node differences, some of which are given below :
|8 - 3| = 5
|3 - 7| = 4
|8 - 1| = 7
|10 - 13| = 3
Among all possible differences, the maximum value of 7 is obtained by |8 - 1| = 7.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/11/09/tmp-tree-1.jpg)

```
Input: root = [1,null,2,null,0,3]
Output: 3
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[2, 5000]`.
- `0 <= Node.val <= 105`

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
 * @return {number}
 */
var maxAncestorDiff = function(root) {
    return findMaxAndMin(root, root.val, root.val);

};

function findMaxAndMin(root, min, max){
    if(root === null){
        return max - min;
    }
    max = Math.max(root.val, max);
    min = Math.min(root.val, min);
    let leftResult = findMaxAndMin(root.left, min, max);
    let rightResult = findMaxAndMin(root.right,min, max);

    return Math.max(leftResult, rightResult);


}
```

## 14. Path Sum

Given the `root` of a binary tree and an integer `targetSum`, return `true` if the tree has a **root-to-leaf** path such that adding up all the values along the path equals `targetSum`.

A **leaf** is a node with no children.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsum1.jpg)

```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
Output: true
Explanation: The root-to-leaf path with the target sum is shown.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg)

```
Input: root = [1,2,3], targetSum = 5
Output: false
Explanation: There are two root-to-leaf paths in the tree:
(1 --> 2): The sum is 3.
(1 --> 3): The sum is 4.
There is no root-to-leaf path with sum = 5.
```

**Example 3:**

```
Input: root = [], targetSum = 0
Output: false
Explanation: Since the tree is empty, there are no root-to-leaf paths.
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5000]`.
- `-1000 <= Node.val <= 1000`
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
 * @return {boolean}
 */
var hasPathSum = function(root, targetSum) {

    if(root === null){
        return false;
    }

    if(root.left === null && root.right === null){
        if(targetSum === root.val){
            return true;
        } else {
            return false;
        }
    }

    let left = hasPathSum(root.left, targetSum - root.val);
    let right = hasPathSum(root.right, targetSum - root.val);

    return left || right
    
};
```

## 15. Diameter of Binary Tree

Given the `root` of a binary tree, return *the length of the **diameter** of the tree*.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

```
Input: root = [1,2,3,4,5]
Output: 3
Explanation: 3 is the length of the path [4,2,1,3] or [5,2,1,3].
```

**Example 2:**

```
Input: root = [1,2]
Output: 1
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
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
 * @return {number}
 */
var diameterOfBinaryTree = function(root) {
    let diameter = 0;
    
    function findLongestPath(node){
        if(node === null){
            return 0;
        }

        let leftPath = findLongestPath(node.left);
        let rightPath = findLongestPath(node.right);
        
        diameter = Math.max(diameter, leftPath + rightPath);

        return Math.max(leftPath, rightPath) + 1;
    }
    
    findLongestPath(root);
    
    return diameter;
    
};
```

## 16. Count Good Nodes in Binary Tree

Given a binary tree `root`, a node *X* in the tree is named **good** if in the path from root to *X* there are no nodes with a value *greater than* X.

Return the number of **good** nodes in the binary tree.

 

**Example 1:**

**![img](https://assets.leetcode.com/uploads/2020/04/02/test_sample_1.png)**

```
Input: root = [3,1,4,3,null,1,5]
Output: 4
Explanation: Nodes in blue are good.
Root Node (3) is always a good node.
Node 4 -> (3,4) is the maximum value in the path starting from the root.
Node 5 -> (3,4,5) is the maximum value in the path
Node 3 -> (3,1,3) is the maximum value in the path.
```

**Example 2:**

**![img](https://assets.leetcode.com/uploads/2020/04/02/test_sample_2.png)**

```
Input: root = [3,3,null,4,2]
Output: 3
Explanation: Node 2 -> (3, 3, 2) is not good, because "3" is higher than it.
```

**Example 3:**

```
Input: root = [1]
Output: 1
Explanation: Root is considered as good.
```

 

**Constraints:**

- The number of nodes in the binary tree is in the range `[1, 10^5]`.
- Each node's value is between `[-10^4, 10^4]`.

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
 * @return {number}
 */
var goodNodes = function(root) {
    let num = 0;

    function findGoodNode(root, max){
        if(root === null){
            return;
        }
        if(root.val >= max){
            num += 1;
            max = root.val;
        }

        findGoodNode(root.left, max);
        findGoodNode(root.right, max);
    }

    findGoodNode(root, root.val);

    return num;
    
};
```

## 17. Same Tree

Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/12/20/ex1.jpg)

```
Input: p = [1,2,3], q = [1,2,3]
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/12/20/ex2.jpg)

```
Input: p = [1,2], q = [1,null,2]
Output: false
```

**Example 3:**

![img](https://assets.leetcode.com/uploads/2020/12/20/ex3.jpg)

```
Input: p = [1,2,1], q = [1,1,2]
Output: false
```

**Constraints:**

- The number of nodes in both trees is in the range `[0, 100]`.
- `-104 <= Node.val <= 104`

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
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {boolean}
 */
var isSameTree = function(p, q) {

    if(p === null){
        if(q === null){
            return true;
        } else {
            return false;
        }
    } 

    if(q === null){
        if(p === null){
            return true;
        } else {
            return false;
        }
    }

    if(p.val != q.val){
        return false;
    } else {
        return isSameTree(p.left,q.left) && isSameTree(p.right, q.right);
    }
    
};
```



### Better Logic Solution

```javascript
var isSameTree = function(p, q) {
    // 如果 p 和 q 都是 null，则它们是相同的树
    if (p === null && q === null) {
        return true;
    }

    // 如果只有一个是 null 或者它们的值不同，则它们不是相同的树
    if (p === null || q === null || p.val !== q.val) {
        return false;
    }

    // 递归检查左子树和右子树
    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
};
```

## 18. Lowest Common Ancestor of a Binary Tree

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

**Example 3:**

```
Input: root = [1,2], p = 1, q = 2
Output: 1
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[2, 105]`.
- `-109 <= Node.val <= 109`
- All `Node.val` are **unique**.
- `p != q`
- `p` and `q` will exist in the tree.

### My Solution

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
var lowestCommonAncestor = function(root, p, q) {
    // 到leaf节点还没有找到p或q
    if(root === null){
        return null;
    }
	
    // 找到p, 返回
    if(root.val === p.val){
        return root;
    }
    
	// 找到q, 返回
    if(root.val === q.val){
        return root;
    }
	
    // 继续查找subtree
    let left = lowestCommonAncestor(root.left, p, q);
    let right = lowestCommonAncestor(root.right, p, q);
	
    // 如果左边找到了一个, 右边找到一个, 说明当前节点就是LCA (这是第一个能满足p和q在左右两侧的节点)
    if(left && right){
        return root;
    }
	
    // 如果只找到一个, 说明另一个在另一侧的子树上
    return left === null ? right : left;
    
};
```

