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
            stack.pop()                
        } else {
            stack.push(char);
        }
    }
    
    
    return stack.join('');
};
```

