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
