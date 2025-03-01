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

## 19. Binary Tree Right Side View

Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return *the values of the nodes you can see ordered from top to bottom*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/14/tree.jpg)

```
Input: root = [1,2,3,null,5,null,4]
Output: [1,3,4]
```

**Example 2:**

```
Input: root = [1,null,3]
Output: [1,3]
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
 * @return {number[]}
 */
var rightSideView = function(root) {
    let queue = [root];
    let result = [];
    if(root === null){
        return [];
    }

    while(queue.length){
        result.push(queue[queue.length - 1].val);

        let nextlevelNodes = [];

        queue.forEach((value)=>{
            if(value.left){
                nextlevelNodes.push(value.left);
            }

            if(value.right){
                nextlevelNodes.push(value.right);
            }
        });

        queue = nextlevelNodes;
    }

    return result;
    
};
```

## 20. Find Largest Value in Each Tree Row

Given the `root` of a binary tree, return *an array of the largest value in each row* of the tree **(0-indexed)**.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/08/21/largest_e1.jpg)

```
Input: root = [1,3,2,5,3,null,9]
Output: [1,3,9]
```

**Example 2:**

```
Input: root = [1,2,3]
Output: [1,3]
```

 

**Constraints:**

- The number of nodes in the tree will be in the range `[0, 104]`.
- `-231 <= Node.val <= 231 - 1`

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
 * @return {number[]}
 */
var largestValues = function(root) {
    if(root === null){
        return [];
    }
    let queue = [root];
    let result = [];

    while(queue.length){
        let maxThisRow = -Infinity;
        let nextQueue = [];

        queue.forEach(value => {
            if(value.val > maxThisRow){
                maxThisRow = value.val;
            }
            if(value.left){
                nextQueue.push(value.left);
            }
            if(value.right){
                nextQueue.push(value.right);
            }
        })
        result.push(maxThisRow);
        queue = nextQueue;
    }

    return result;
    
};
```

## 21. Deepest Leaves Sum

Given the `root` of a binary tree, return *the sum of values of its deepest leaves*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/07/31/1483_ex1.png)

```
Input: root = [1,2,3,4,5,null,6,7,null,null,null,null,8]
Output: 15
```

**Example 2:**

```
Input: root = [6,7,8,2,7,1,3,9,null,1,4,null,null,null,5]
Output: 19
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `1 <= Node.val <= 100`

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
var deepestLeavesSum = function(root) {
    let queue = [root];
    let leafLevel = {};
    let currentDepth = 0;
    while(queue.length){
        let nextQueue = [];
        let leafNodesValue = [];
        queue.forEach((value) => {
            if(value.left){
                nextQueue.push(value.left);
            }
            
            if(value.right){
                nextQueue.push(value.right);
            }
            
            // identif leaf node
            if(value.left === null && value.right === null){
                leafNodesValue.push(value.val);
            }
        })
        
        leafLevel[currentDepth]  = leafNodesValue;
        currentDepth ++;
        queue = nextQueue; 
    }
    
    return leafLevel[currentDepth - 1].reduce((acc, current)=>{return acc + current}, 0);
};
```

### Better Solution

```javascript
var deepestLeavesSum = function(root) {
    let queue = [root];
    let sum = 0;
    
    while (queue.length) {
        sum = 0;  // 重置当前层级的和
        const nextQueue = [];
        
        queue.forEach(node => {
            sum += node.val;  // 累加当前节点的值
            
            if (node.left) nextQueue.push(node.left);
            if (node.right) nextQueue.push(node.right);
        });
        
        queue = nextQueue;  // 移动到下一层
    }
    
    return sum;  // 返回最深层叶子节点的和
};
```

## 22. Binary Tree Zigzag Level Order Traversal

Given the `root` of a binary tree, return *the zigzag level order traversal of its nodes' values*. (i.e., from left to right, then right to left for the next level and alternate between).

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[20,9],[15,7]]
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
 * @return {number[][]}
 */
var zigzagLevelOrder = function(root) {
    if(root === null){
        return [];
    }
    let queue = [root];
    let result = [];
    
    let flag = true;
    
    while(queue.length){
        let nextQueue = [];
        let nextQueueValue = [];
        
        queue.forEach(node => {
            if(node.left){
                nextQueue.push(node.left);
            }
            if(node.right){
                nextQueue.push(node.right);
            }
            
            switch(flag){
                case true: 
                    nextQueueValue.push(node.val);
                    break;
                case false:
                    nextQueueValue.unshift(node.val);
                    break;
            }
        });
        
      
        
        flag = !flag;
        queue = nextQueue;
        result.push(nextQueueValue);
    }
        
    return result;
    
};
```

## 23. Range Sum of BST

Given the `root` node of a binary search tree and two integers `low` and `high`, return *the sum of values of all nodes with a value in the **inclusive** range* `[low, high]`.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/11/05/bst1.jpg)

```
Input: root = [10,5,15,3,7,null,18], low = 7, high = 15
Output: 32
Explanation: Nodes 7, 10, and 15 are in the range [7, 15]. 7 + 10 + 15 = 32.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/11/05/bst2.jpg)

```
Input: root = [10,5,15,3,7,13,18,1,null,6], low = 6, high = 10
Output: 23
Explanation: Nodes 6, 7, and 10 are in the range [6, 10]. 6 + 7 + 10 = 23.
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 2 * 104]`.
- `1 <= Node.val <= 105`
- `1 <= low <= high <= 105`
- All `Node.val` are **unique**.

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
 * @param {number} low
 * @param {number} high
 * @return {number}
 */
var rangeSumBST = function(root, low, high) {
    if(root === null){
        return 0;
    }
    let anwser = 0;

    if(root.val >= low && root.val <= high){
        anwser += root.val;
    }

    if(root.val > low){
        anwser += rangeSumBST(root.left, low, high);
    }

    if(root.val < high){
        anwser += rangeSumBST(root.right, low, high);
    }

    return anwser;
};
```

## 24. Minimum Absolute Difference in BST

Given the `root` of a Binary Search Tree (BST), return *the minimum absolute difference between the values of any two different nodes in the tree*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/02/05/bst1.jpg)

```
Input: root = [4,2,6,1,3]
Output: 1
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/02/05/bst2.jpg)

```
Input: root = [1,0,48,null,null,12,49]
Output: 1
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[2, 104]`.
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
var getMinimumDifference = function(root) {
    let prev = null;
    let minDiff = Infinity;

    function dfs(root){
        if(root === null){
            return;
        }

        dfs(root.left);
        if(prev != null){
            minDiff = Math.min(minDiff, Math.abs(prev-root.val));
        }
        prev = root.val;
        dfs(root.right);
    }

    dfs(root);
    return minDiff;
    
};
```

## 25. Validate Binary Search Tree

Given the `root` of a binary tree, *determine if it is a valid binary search tree (BST)*.

A **valid BST** is defined as follows:

- The left subtree of a node contains only nodes with keys less than the node's key.

- The right subtree of a node contains only nodes with keys **greater than** the node's key.

- Both the left and right subtrees must also be binary search trees.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)

```
Input: root = [2,1,3]
Output: true
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg)

```
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-231 <= Node.val <= 231 - 1`

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
var isValidBST = function(root) {
    let prev = null;
    let indicator = true;
    function dfs(node){
        if(node === null){
            return;
        }

        dfs(node.left);
        if(prev != null){
            if(prev >= node.val){
                indicator = false;
            }
        }
        prev = node.val;
        dfs(node.right);

        return;
    }

    dfs(root);

    return indicator;
    
};
```

## 26. Insert into a Binary Search Tree

You are given the `root` node of a binary search tree (BST) and a `value` to insert into the tree. Return *the root node of the BST after the insertion*. It is **guaranteed** that the new value does not exist in the original BST.

**Notice** that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return **any of them**.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/05/insertbst.jpg)

```
Input: root = [4,2,7,1,3], val = 5
Output: [4,2,7,1,3,5]
Explanation: Another accepted tree is:
```

**Example 2:**

```
Input: root = [40,20,60,10,30,50,70], val = 25
Output: [40,20,60,10,30,50,70,null,null,25]
```

**Example 3:**

```
Input: root = [4,2,7,1,3,null,null,null,null,null,null], val = 5
Output: [4,2,7,1,3,5]
```

 

**Constraints:**

- The number of nodes in the tree will be in the range `[0, 104]`.
- `-108 <= Node.val <= 108`
- All the values `Node.val` are **unique**.
- `-108 <= val <= 108`
- It's **guaranteed** that `val` does not exist in the original BST.

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
 * @param {number} val
 * @return {TreeNode}
 */
var insertIntoBST = function(root, val) {
    
    if(root === null){
        return new TreeNode(val, null, null);
    }
    
    function insert(root, val){
        if(root.left === null && root.right === null){
            if(val < root.val){
                root.left = new TreeNode(val, null, null);
                return;
            } else {
                root.right = new TreeNode(val, null, null);
                return;
            }
        }
        
        if(root.left === null && val < root.val){
            root.left = new TreeNode(val, null, null);
                return;
        }
        
        if(root.right === null && val > root.val){
            root.right = new TreeNode(val, null, null);
                return;
        }
    
        if(val < root.val){
            insert(root.left, val);
        } else {
            insert(root.right, val);
        }
    }
    
    insert(root, val);
    
    return root;    
};
```

### Better Solution

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val === undefined ? 0 : val);
 *     this.left = (left === undefined ? null : left);
 *     this.right = (right === undefined ? null : right);
 * }
/**
 * @param {TreeNode} root
 * @param {number} val
 * @return {TreeNode}
 */
var insertIntoBST = function(root, val) {
    // 如果 root 是空树，返回一个新的节点作为根节点
    if (root === null) {
        return new TreeNode(val, null, null);
    }
    
    // 插入的过程，找到正确位置
    if (val < root.val) {
        // 递归插入到左子树
        root.left = insertIntoBST(root.left, val);
    } else {
        // 递归插入到右子树
        root.right = insertIntoBST(root.right, val);
    }

    return root;
};

```

## 27. Closest Binary Search Tree Value

Given the `root` of a binary search tree and a `target` value, return *the value in the BST that is closest to the* `target`. If there are multiple answers, print the smallest.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/12/closest1-1-tree.jpg)

```
Input: root = [4,2,5,1,3], target = 3.714286
Output: 4
```

**Example 2:**

```
Input: root = [1], target = 4.428571
Output: 1
```

 

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `0 <= Node.val <= 109`
- `-109 <= target <= 109`

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
 * @param {number} target
 * @return {number}
 */
var closestValue = function(root, target) {
    let closest = root.val;
    
    function findMinDiff(root){
        if(root === null){
            return;
        }
        let currSmallestDiff = Math.abs(target - closest);
        let currDiff = Math.abs(target - root.val);
        
        if(currSmallestDiff === currDiff){
            closest = Math.min(closest, root.val);
        }
        
        if(currSmallestDiff > currDiff){
            closest = root.val;
        }
        
         if (target === root.val) {
            closest = root.val;
            return;
        }
        
        if(target > root.val){
            findMinDiff(root.right);       
        } else {
            findMinDiff(root.left);
        }
    }
    
    findMinDiff(root);
    return closest;
    
};
```

##  28. Number of Provinces

There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the `ith` city and the `jth` city are directly connected, and `isConnected[i][j] = 0` otherwise.

Return *the total number of **provinces***.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg)

```
Input: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
Output: 2
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg)

```
Input: isConnected = [[1,0,0],[0,1,0],[0,0,1]]
Output: 3
```

 

**Constraints:**

- `1 <= n <= 200`
- `n == isConnected.length`
- `n == isConnected[i].length`
- `isConnected[i][j]` is `1` or `0`.
- `isConnected[i][i] == 1`
- `isConnected[i][j] == isConnected[j][i]`

### My Solution

```javascript
/**
 * @param {number[][]} isConnected
 * @return {number}
 */
var findCircleNum = function(isConnected) {
    // build the graph
    let graph = new Map();
    for(let i = 0; i < isConnected.length; i++){
        graph.set(i, []);
        for(let j = 0; j < isConnected.length; j++){
            if(isConnected[i][j] === 1){
                graph.get(i).push(j);
            }
        }
    }

    // make the seen set to prevent infinite search
    let seen = new Set();
	
    // deep first search with traversal every node in the province of the input node
    function dfs(node){
        for(let neighbor of graph.get(node)){
            if (!seen.has(neighbor)) {
                seen.add(neighbor);
                dfs(neighbor);
            }
        }
    }

    let ans = 0;

    for(let i = 0; i < isConnected.length; i++){
        if(!seen.has(i)){
            ans ++;
            // every time trigger a search means that a new province
            dfs(i);
        }
    }

    return ans;
    
};
```

## 29. Number of Islands

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return *the number of islands*.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

**Example 2:**

```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

 

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`.

### My Solution

```javascript
/**
 * @param {character[][]} grid
 * @return {number}
 */
var numIslands = function(grid) {
    let direction = [
        [-1, 0], 
        [0, 1], 
        [0, -1], 
        [1, 0]
    ];

    let seen = [];
    
    for (let i = 0; i < grid.length; i++) {
        seen.push(new Array(grid[0].length).fill(false));
    }

    function validIslandNeighbor(row, col){
        return row >= 0 && col >= 0 && row < grid.length && col < grid[0].length && grid[row][col] === '1';
    }

    function dfs(row, col){
        for(const[dx, dy] of direction){
            let nextRow = row + dx;
            let nextCol = col + dy;
            if(validIslandNeighbor(nextRow, nextCol) && !seen[nextRow][nextCol]){
                seen[nextRow][nextCol] = true;
                dfs(nextRow, nextCol);
            }  
        }
    }

    let ans = 0;

    for(let i = 0;  i < grid.length; i++){
        for(let j = 0; j < grid[0].length; j++){
            if(grid[i][j] === '1' && !seen[i][j]){
                ans ++;
                seen[i][j] = true;
                dfs(i, j);
            }
        }
    }

    return ans
    
};
```

## 30. Reorder Routes to Make All Paths Lead to the City Zero

There are `n` cities numbered from `0` to `n - 1` and `n - 1` roads such that there is only one way to travel between two different cities (this network form a tree). Last year, The ministry of transport decided to orient the roads in one direction because they are too narrow.

Roads are represented by `connections` where `connections[i] = [ai, bi]` represents a road from city `ai` to city `bi`.

This year, there will be a big event in the capital (city `0`), and many people want to travel to this city.

Your task consists of reorienting some roads such that each city can visit the city `0`. Return the **minimum** number of edges changed.

It's **guaranteed** that each city can reach city `0` after reorder.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/05/13/sample_1_1819.png)

```
Input: n = 6, connections = [[0,1],[1,3],[2,3],[4,0],[4,5]]
Output: 3
Explanation: Change the direction of edges show in red such that each node can reach the node 0 (capital).
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/05/13/sample_2_1819.png)

```
Input: n = 5, connections = [[1,0],[1,2],[3,2],[3,4]]
Output: 2
Explanation: Change the direction of edges show in red such that each node can reach the node 0 (capital).
```

**Example 3:**

```
Input: n = 3, connections = [[1,0],[2,0]]
Output: 0
```

 

**Constraints:**

- `2 <= n <= 5 * 104`
- `connections.length == n - 1`
- `connections[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`

### My Solution

```javascript
/**
 * @param {number} n
 * @param {number[][]} connections
 * @return {number}
 */
var minReorder = function(n, connections) {

    // make the graph
    let map = new Map();
    let road = new Set();
    let seen = new Set();

    for(let i = 0; i < connections.length; i++){
        if(!map.has(connections[i][0])){
            map.set(connections[i][0], []);
        }
        if(!map.has(connections[i][1])){
            map.set(connections[i][1], []);
        }
        map.get(connections[i][0]).push(connections[i][1]);
        map.get(connections[i][1]).push(connections[i][0]);
        road.add(JSON.stringify(connections[i]));
    }

    let ans = 0;
    function dfs(node){
        for(const neighbor of map.get(node)){
            if(!seen.has(neighbor)){
                if(road.has(JSON.stringify([node, neighbor]))){
                    ans += 1;
                }
                seen.add(neighbor);
                dfs(neighbor);
            }
        }
        
    }

    seen.add(0);
    dfs(0);

    return ans;
    
};
```

## 31. Keys and Rooms

There are `n` rooms labeled from `0` to `n - 1` and all the rooms are locked except for room `0`. Your goal is to visit all the rooms. However, you cannot enter a locked room without having its key.

When you visit a room, you may find a set of **distinct keys** in it. Each key has a number on it, denoting which room it unlocks, and you can take all of them with you to unlock the other rooms.

Given an array `rooms` where `rooms[i]` is the set of keys that you can obtain if you visited room `i`, return `true` *if you can visit **all** the rooms, or* `false` *otherwise*.

 

**Example 1:**

```
Input: rooms = [[1],[2],[3],[]]
Output: true
Explanation: 
We visit room 0 and pick up key 1.
We then visit room 1 and pick up key 2.
We then visit room 2 and pick up key 3.
We then visit room 3.
Since we were able to visit every room, we return true.
```

**Example 2:**

```
Input: rooms = [[1,3],[3,0,1],[2],[0]]
Output: false
Explanation: We can not enter room number 2 since the only key that unlocks it is in that room.
```

 

**Constraints:**

- `n == rooms.length`
- `2 <= n <= 1000`
- `0 <= rooms[i].length <= 1000`
- `1 <= sum(rooms[i].length) <= 3000`
- `0 <= rooms[i][j] < n`
- All the values of `rooms[i]` are **unique**.

### My Solution

```javascript
/**
 * @param {number[][]} rooms
 * @return {boolean}
 */
var canVisitAllRooms = function(rooms) {

    let seen = new Set();

    function dfs(openRoom){
        for(const roomCanBeOpen of rooms[openRoom]){
            if(!seen.has(roomCanBeOpen)){
                seen.add(roomCanBeOpen);
                dfs(roomCanBeOpen);
            }
        }
    }

    seen.add(0);
    dfs(0);

    if(seen.size === rooms.length){
        return true;
    } else {
        return false;
    }
};
```

## 32. Minimum Number of Vertices to Reach All Nodes

Given a **directed acyclic graph**, with `n` vertices numbered from `0` to `n-1`, and an array `edges` where `edges[i] = [fromi, toi]` represents a directed edge from node `fromi` to node `toi`.

Find *the smallest set of vertices from which all nodes in the graph are reachable*. It's guaranteed that a unique solution exists.

Notice that you can return the vertices in any order.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/07/07/untitled22.png)

```
Input: n = 6, edges = [[0,1],[0,2],[2,5],[3,4],[4,2]]
Output: [0,3]
Explanation: It's not possible to reach all the nodes from a single vertex. From 0 we can reach [0,1,2,5]. From 3 we can reach [3,4,2,5]. So we output [0,3].
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2020/07/07/untitled.png)

```
Input: n = 5, edges = [[0,1],[2,1],[3,1],[1,4],[2,4]]
Output: [0,2,3]
Explanation: Notice that vertices 0, 3 and 2 are not reachable from any other node, so we must include them. Also any of these vertices can reach nodes 1 and 4.
```

 

**Constraints:**

- `2 <= n <= 10^5`
- `1 <= edges.length <= min(10^5, n * (n - 1) / 2)`
- `edges[i].length == 2`
- `0 <= fromi, toi < n`
- All pairs `(fromi, toi)` are distinct.

### My Solution

The answer is the number of nodes with 0 indegree.

```javascript
/**
 * @param {number} n
 * @param {number[][]} edges
 * @return {number[]}
 */
var findSmallestSetOfVertices = function(n, edges) {
    // find the vertices wich indegree is 0;
    let zeroIndegreeNode = new Array(n).fill(0);
    edges.forEach((([from, target]) =>{
        zeroIndegreeNode[target] += 1;
    }))

    let ans = [];
    zeroIndegreeNode.forEach((value, index) => {
        if(value === 0){
            ans.push(index);
        }
    })
    
    return ans;
};
```

## 33. Find if Path Exists in Graph

There is a **bi-directional** graph with `n` vertices, where each vertex is labeled from `0` to `n - 1` (**inclusive**). The edges in the graph are represented as a 2D integer array `edges`, where each `edges[i] = [ui, vi]` denotes a bi-directional edge between vertex `ui` and vertex `vi`. Every vertex pair is connected by **at most one** edge, and no vertex has an edge to itself.

You want to determine if there is a **valid path** that exists from vertex `source` to vertex `destination`.

Given `edges` and the integers `n`, `source`, and `destination`, return `true` *if there is a **valid path** from* `source` *to* `destination`*, or* `false` *otherwise**.*

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex1.png)

```
Input: n = 3, edges = [[0,1],[1,2],[2,0]], source = 0, destination = 2
Output: true
Explanation: There are two paths from vertex 0 to vertex 2:
- 0 → 1 → 2
- 0 → 2
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex2.png)

```
Input: n = 6, edges = [[0,1],[0,2],[3,5],[5,4],[4,3]], source = 0, destination = 5
Output: false
Explanation: There is no path from vertex 0 to vertex 5.
```

 

**Constraints:**

- `1 <= n <= 2 * 105`
- `0 <= edges.length <= 2 * 105`
- `edges[i].length == 2`
- `0 <= ui, vi <= n - 1`
- `ui != vi`
- `0 <= source, destination <= n - 1`
- There are no duplicate edges.
- There are no self edges.

### My Solution

```javascript
/**
 * @param {number} n
 * @param {number[][]} edges
 * @param {number} source
 * @param {number} destination
 * @return {boolean}
 */
var validPath = function(n, edges, source, destination) {
    
    if(n < 2){
        return true;
    }
    
    if(source === destination){
        return true;
    }
    
    // make the graph
    let graph = new Map();
    
    edges.forEach(([a, b]) =>{
        if(!graph.has(a)){
            graph.set(a, []);
        }
        
        if(!graph.has(b)){
            graph.set(b, []);
        }
        
        graph.get(a).push(b);
        graph.get(b).push(a);
    })
    
    let seen = new Set();
    let result= false;
    
    function dfs(node){
        for(const neighbor of graph.get(node)){
            if(!seen.has(neighbor)){
               if(neighbor === destination){
                   result = true;
                   return;
               }
                seen.add(neighbor);
                dfs(neighbor);
            }
        }
    }
    
    seen.add(source);
    dfs(source);
    
    return result;
    
};
```

## 34. Number of Connected Components in an Undirected Graph

You have a graph of `n` nodes. You are given an integer `n` and an array `edges` where `edges[i] = [ai, bi]` indicates that there is an edge between `ai` and `bi` in the graph.

Return *the number of connected components in the graph*.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/03/14/conn1-graph.jpg)

```
Input: n = 5, edges = [[0,1],[1,2],[3,4]]
Output: 2
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/03/14/conn2-graph.jpg)

```
Input: n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]
Output: 1
```

 

**Constraints:**

- `1 <= n <= 2000`
- `1 <= edges.length <= 5000`
- `edges[i].length == 2`
- `0 <= ai <= bi < n`
- `ai != bi`
- There are no repeated edges.

### My Solution

```javascript
/**
 * @param {number} n
 * @param {number[][]} edges
 * @return {number}
 */
var countComponents = function(n, edges) {
    
    // make the graph
    let graph = new Map();
    let seen = new Set();
    
    for(let i = 0; i < n; i++){
        graph.set(i, []);
    }
    
    edges.forEach(([a, b], index) =>{
        graph.get(a).push(b);
        graph.get(b).push(a);
    })
    
    function dfs(node){
        for(const neighbor of graph.get(node)){
            if(!seen.has(neighbor)){
                seen.add(neighbor);
                dfs(neighbor);
            }
        }
    }
    
    let ans = 0;
    
    for(let i = 0; i < n; i++){
        if(!seen.has(i)){
            ans++;
            seen.add(i);
            dfs(i);
        }
    }
    
    return ans;
    
};
```

## 35. Max Area of Island

You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected **4-directionally** (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The **area** of an island is the number of cells with a value `1` in the island.

Return *the maximum **area** of an island in* `grid`. If there is no island, return `0`.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/05/01/maxarea1-grid.jpg)

```
Input: grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
Output: 6
Explanation: The answer is not 11, because the island must be connected 4-directionally.
```

**Example 2:**

```
Input: grid = [[0,0,0,0,0,0,0,0]]
Output: 0
```

 

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 50`
- `grid[i][j]` is either `0` or `1`.

### My Solution

```javascript
/**
 * @param {number[][]} grid
 * @return {number}
 */
var maxAreaOfIsland = function(grid) {
    
    let m = grid.length;
    let n = grid[0].length;
    
    let direction = [
        [1, 0],
        [0, 1], 
        [-1, 0], 
        [0, -1]
    ];
    
    function valid(row, col){
        return row >= 0 && row < m && col >= 0 && col < n && grid[row][col] === 1;
    }
    
    // make the seen array
    
    let seen = [];
    
    for(let i = 0; i < m; i++){
        seen.push(new Array(n).fill(false));
    }
    
    let area = 0;
    let maxArea = 0;
    
    function dfs(row, col){
        for(const [dx, dy] of direction){
            let newRow = row + dx;
            let newCol = col + dy;
            
            if(valid(newRow, newCol) && !seen[newRow][newCol]){
                seen[newRow][newCol] = true;
                area ++;
                dfs(newRow, newCol);
            }
        }
        
    }
    
    
    for(let i = 0; i < m; i++){
        for(let j = 0; j < n; j++){
            area = 0;
            if(grid[i][j] === 1 && seen[i][j] === false){
                area ++;
                seen[i][j] = true;
                dfs(i, j);
                maxArea = Math.max(maxArea, area);
            }
            
        }
    }
    
    return maxArea;
    
};
```

## 36. Reachable Nodes With Restrictions

There is an undirected tree with `n` nodes labeled from `0` to `n - 1` and `n - 1` edges.

You are given a 2D integer array `edges` of length `n - 1` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the tree. You are also given an integer array `restricted` which represents **restricted** nodes.

Return *the **maximum** number of nodes you can reach from node* `0` *without visiting a restricted node.*

Note that node `0` will **not** be a restricted node.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2022/06/15/ex1drawio.png)

```
Input: n = 7, edges = [[0,1],[1,2],[3,1],[4,0],[0,5],[5,6]], restricted = [4,5]
Output: 4
Explanation: The diagram above shows the tree.
We have that [0,1,2,3] are the only nodes that can be reached from node 0 without visiting a restricted node.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2022/06/15/ex2drawio.png)

```
Input: n = 7, edges = [[0,1],[0,2],[0,5],[0,4],[3,2],[6,5]], restricted = [4,2,1]
Output: 3
Explanation: The diagram above shows the tree.
We have that [0,5,6] are the only nodes that can be reached from node 0 without visiting a restricted node.
```

 

**Constraints:**

- `2 <= n <= 105`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `0 <= ai, bi < n`
- `ai != bi`
- `edges` represents a valid tree.
- `1 <= restricted.length < n`
- `1 <= restricted[i] < n`
- All the values of `restricted` are **unique**.

### My Solution

```javascript
/**
 * @param {number} n
 * @param {number[][]} edges
 * @param {number[]} restricted
 * @return {number}
 */
var reachableNodes = function(n, edges, restricted) {
    let graph = new Map();
    
    // make the graph without restricted
    
    let restrictSet = new Set(restricted);
    
    edges.forEach(([a, b]) => {
        if(!graph.has(a)){
            graph.set(a, []);
        }

        if(!graph.has(b)){
            graph.set(b, []);
        }

        graph.get(a).push(b);
        graph.get(b).push(a);
    })
    
    // dfs
    let seen = restrictSet;
    let ans = 0;
    
    function dfs(node){
        for(const neighbor of graph.get(node)){
            if(!seen.has(neighbor)){
                ans ++;
                seen.add(neighbor)
                dfs(neighbor);
            }   
        }
    }
    
    seen.add(0);
    ans++;
    dfs(0);
    
    return ans;
};
```

