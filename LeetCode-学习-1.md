---
title: LeetCode 学习 - 1
date: 2024-09-19 04:41:02
categories: 学习笔记
tags: 
- Interview Preparation
- LeetCode
---

This note includes multiple leetcode algorithm problems with my solution

<!-- more -->
<!-- toc -->



## 1. Reverse String

Write a function that reverses a string. The input string is given as an array of characters `s`.

You must do this by modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm) with `O(1)` extra memory.

 

**Example 1:**

```
Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```

**Example 2:**

```
Input: s = ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
```

**Constraints:**

- `1 <= s.length <= 105`
- `s[i]` is a [printable ascii character](https://en.wikipedia.org/wiki/ASCII#Printable_characters).

### My Solution

Use 2 pointer approach

```javascript
var reverseString = function(s) {
    let a = 0;
    let b = s.length - 1;
    
    while (a < b){
        let temp = s[a];
        s[a] = s[b];
        s[b] = temp;
        
        a ++;
        b --;
    }
        
};
```



## 2. Squares of a Sorted Array

Given an integer array `nums` sorted in **non-decreasing** order, return *an array of **the squares of each number** sorted in non-decreasing order*.

**Example 1:**

```
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].
```

**Example 2:**

```
Input: nums = [-7,-3,2,3,11]
Output: [4,9,9,49,121]
```

**Constraints:**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` is sorted in **non-decreasing** order.

 

**Follow up:** Squaring each element and sorting the new array is very trivial, could you find an `O(n)` solution using a different approach?

### My Solution

```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortedSquares = function(nums) {
    nums.forEach((value, index, array) => {array[index] = value * value;});
    return nums.sort((a, b) => {return a - b});
};
```

### Smarter Solution

**Intuition**

Since the array `A` is sorted, loosely speaking it has some negative elements with squares in decreasing order, then some non-negative elements with squares in increasing order.

For example, with `[-3, -2, -1, 4, 5, 6]`, we have the negative part `[-3, -2, -1]` with squares `[9, 4, 1]`, and the positive part `[4, 5, 6]` with squares `[16, 25, 36]`. Our strategy is to iterate over the negative part in reverse, and the positive part in the forward direction.

**Algorithm**

We can use two pointers to read the positive and negative parts of the array - one pointer `j` in the positive direction, and another `i` in the negative direction.

Now that we are reading two increasing arrays (the squares of the elements), we can merge these arrays together using a two-pointer technique.

```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortedSquares = function(nums) {
    let n = nums.length;
    let result = new Array(n);
    let left = 0;
    let right = n - 1;

    for (let i = n - 1; i >= 0; i--) {
        let square;
        if (Math.abs(nums[left]) < Math.abs(nums[right])) {
            square = nums[right];
            right--;
        } else {
            square = nums[left];
            left++;
        }
        result[i] = square * square;
    }
    return result;
};
```

## 3. Maximum Average Subarray I

You are given an integer array `nums` consisting of `n` elements, and an integer `k`.

Find a contiguous subarray whose **length is equal to** `k` that has the maximum average value and return *this value*. Any answer with a calculation error less than `10-5` will be accepted.

 

**Example 1:**

```
Input: nums = [1,12,-5,-6,50,3], k = 4
Output: 12.75000
Explanation: Maximum average is (12 - 5 - 6 + 50) / 4 = 51 / 4 = 12.75
```

**Example 2:**

```
Input: nums = [5], k = 1
Output: 5.00000
```

**Constraints:**

- `n == nums.length`
- `1 <= k <= n <= 105`
- `-104 <= nums[i] <= 104`

### My Solution

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var findMaxAverage = function(nums, k) {
    let i = 0;
    let j = i + k - 1;
    let maxAvg = -10000;
    let sum = 0;
    
    // calculate the initial sum
    for(let index = 0; index < k; index++){
        sum += nums[i+index];
    }
    
    while(j < nums.length){
        let tempAvg = sum / k;
        if (tempAvg > maxAvg){
            maxAvg = tempAvg;
        }
        i++;
        j++;
        
        // only need to add a new element and delete an old element. No need to calculate the whold sum of the array every loop
        sum += nums[j];
        sum -= nums[i-1];
        
    }
    
    return maxAvg;
        
};
```

### More Optimized Solution

The general logic is the same, but the algorithm is optimized in the smallest details.

``` javascript
var findMaxAverage = function(nums, k) {
    let maxAvg = -Infinity;
    let sum = 0;
    
    // Calculate the initial sum for the first window of size k
    for (let index = 0; index < k; index++) {
        sum += nums[index];
    }
    
    // Initialize maxAvg with the average of the first window
    maxAvg = sum / k;
    
    // Start sliding the window
    for (let i = k; i < nums.length; i++) {
        // Slide the window: add the next element and remove the first element of the previous window
        sum += nums[i] - nums[i - k];
        
        // Calculate the average of the current window
        let currentAvg = sum / k;
        
        // Update maxAvg if the current window's average is higher
        if (currentAvg > maxAvg) {
            maxAvg = currentAvg;
        }
    }
    
    return maxAvg;
};
```

## 4. Check if the Sentence Is Pangram

A **pangram** is a sentence where every letter of the English alphabet appears at least once.

Given a string `sentence` containing only lowercase English letters, return `true` *if* `sentence` *is a **pangram**, or* `false` *otherwise.*

 

**Example 1:**

```
Input: sentence = "thequickbrownfoxjumpsoverthelazydog"
Output: true
Explanation: sentence contains at least one of every letter of the English alphabet.
```

**Example 2:**

```
Input: sentence = "leetcode"
Output: false
```

 

**Constraints:**

- `1 <= sentence.length <= 1000`
- `sentence` consists of lowercase English letters.

### My Solution

```javascript
/**
 * @param {string} sentence
 * @return {boolean}
 */
var checkIfPangram = function(sentence) {
    if (sentence.length < 26){
        return false;
    }
    
    let set = new Set();
    
    for (let letter of sentence ){
        set.add(letter);
    }
    
    if(set.size < 26){
        return false;
    }
    
    return true;
};
```

### More Optimized Solution

Can form a Set with the constructor without the loop (it loop inside but with better optimized and tidy code) 

```javascript
/**
 * @param {string} sentence
 * @return {boolean}
 */
var checkIfPangram = function(sentence) {
    let checker = new Set(sentence)
    if(checker.size==26){
        return true
    }
    return false
};
```

## 5. Missing Number

Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return *the only number in the range that is missing from the array.*



**Example 1:**

```
Input: nums = [3,0,1]
Output: 2
Explanation: n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums.
```

**Example 2:**

```
Input: nums = [0,1]
Output: 2
Explanation: n = 2 since there are 2 numbers, so all numbers are in the range [0,2]. 2 is the missing number in the range since it does not appear in nums.
```

**Example 3:**

```
Input: nums = [9,6,4,2,3,5,7,0,1]
Output: 8
Explanation: n = 9 since there are 9 numbers, so all numbers are in the range [0,9]. 8 is the missing number in the range since it does not appear in nums.
```

 

**Constraints:**

- `n == nums.length`
- `1 <= n <= 104`
- `0 <= nums[i] <= n`
- All the numbers of `nums` are **unique**.

 

**Follow up:** Could you implement a solution using only `O(1)` extra space complexity and `O(n)` runtime complexity?

### My Solution

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var missingNumber = function(nums) {
    let set = new Set(nums);
    
    for (let i = 0; i < nums.length + 1; i++){
        if(!set.has(i)){
            return i;
        }
    }
    
    return -1;
    
};
```

### Smarter Solution

Calculate the sum based on equation (O(1)), minus the sum of the array

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var missingNumber = function(nums) {
    let sumOfNumber = (nums.length * (1 + nums.length)) / 2;
    let sum = 0;
    nums.forEach(value => {
        sum += value;
    })
    
    return sumOfNumber - sum;
};
```

## 6. Counting Elements

Given an integer array `arr`, count how many elements `x` there are, such that `x + 1` is also in `arr`. If there are duplicates in `arr`, count them separately.

**Example 1:**

```
Input: arr = [1,2,3]
Output: 2
Explanation: 1 and 2 are counted cause 2 and 3 are in arr.
```

**Example 2:**

```
Input: arr = [1,1,3,3,5,5,7,7]
Output: 0
Explanation: No numbers are counted, cause there is no 2, 4, 6, or 8 in arr.
```

 

**Constraints:**

- `1 <= arr.length <= 1000`
- `0 <= arr[i] <= 1000`

### My Solution

```javascript
/**
 * @param {number[]} arr
 * @return {number}
 */
var countElements = function(arr) {
    let set = new Set(arr);
    let result = 0;
    
    arr.forEach(value => {
        if (set.has(value + 1)){
            result ++;
        } 

    })
    
    return result;
};
```

## 7. Max Consecutive Ones III

Given a binary array `nums` and an integer `k`, return *the maximum number of consecutive* `1`*'s in the array if you can flip at most* `k` `0`'s.

 

**Example 1:**

```
Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
Output: 6
Explanation: [1,1,1,0,0,1,1,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
```

**Example 2:**

```
Input: nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
Output: 10
Explanation: [0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
```

 

**Constraints:**

- `1 <= nums.length <= 105`
- `nums[i]` is either `0` or `1`.
- `0 <= k <= nums.length`

> Correcting the understanding of the topic: **you can flip at most `k`**  means that you can flip an 0 to 1, which not means that swap 2 elements.

### My Solution

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var longestOnes = function(nums, k) {    
    let left = 0;
    let right = 0;
    
    let maxWindow= 0;
    let zeros = 0;

    while (right < nums.length){
        if (zeros < k){
            zeros = nums[right] === 0 ? zeros+1 : zeros;
            right ++;
        } else if (zeros === k){
            if (nums[right] === 1){
                right ++;
            } else {
                zeros = nums[left] === 0 ? zeros-1: zeros;
                left ++;
            } 
        }
        
        let windowLength = right - left;
        maxWindow = windowLength > maxWindow ? windowLength : maxWindow;
    }
    
    return maxWindow;
    
};
```

### 核心思想

题目要求找到一个长度最长的子数组，其中最多可以有 `k` 个 `0` 被翻转为 `1`。也就是说，需要找到一个满足条件的最长的子数组, 该数组最多可以包含`k`个0.

### 算法步骤

1. **定义滑动窗口**:

   - 用两个指针 `left` 和 `right` 来定义一个窗口，窗口内包含数组的一个子数组。
   - `right` 用于扩展窗口，`left` 用于收缩窗口，确保窗口内的 `0` 的数量不超过 `k`。

2. **扩展窗口**:

   - 开始时，`left` 和 `right` 都指向数组的第一个元素。
   - 随着 `right` 的增加，不断地向右扩展窗口，同时检查这个窗口内有多少个 `0`。
   - 如果窗口内的 `0` 数量小于 `k`，说明这个窗口是合法的（即最多可以翻转 `k` 个 `0`），因此可以继续扩大窗口。

3. **收缩窗口**:

   - 当窗口内的 `0` 数量等于 `k` 时，继续扩展窗口（`right` 向右移动）。
   - 如果在扩展过程中遇到一个新的 `0`，意味着再继续扩展窗口将导致 `0` 的数量超过 `k`，这个窗口就不再合法。
   - 为了保持合法性，需要收缩窗口，即移动 `left` 指针，将窗口内的 `0` 数量减少到不超过 `k`。

4. **更新最大窗口长度**:

   - 每次扩展或收缩窗口后，都要计算当前窗口的长度，并与记录的最大窗口长度进行比较，如果当前窗口长度更大，则更新最大窗口长度。

5. **循环直到结束**:

   - 一直重复上述步骤，直到 `right` 到达数组的末尾。此时，已经找到了能够满足条件的最长子数组。

> 1. **为什么先移动再检查？**
>
> 在滑动窗口的算法中，先移动指针（右边界 `right`）再检查 `0` 的数量是否合规，是一种**贪心策略**。这个策略有几个优势：
>
> - **简化实现**：通过先移动再检查，我们不需要考虑多种情况（例如下一个 `0` 是否会超过限制），只需要专注于当前窗口是否合法。如果不合法，就通过调整左边界来缩小窗口直到合法。
> - **动态调整**：在窗口扩展的过程中，你随时可以通过缩小窗口来调整，使其重新符合条件。这个策略确保每一步都在一个有效的范围内操作，而不必预先计算或预测未来的情况，这减少了复杂性。
> - **时间复杂度的优化**：预先检查可能需要额外的计算和判断，使得算法复杂度更高，甚至可能导致一些不必要的计算。而“先移动再检查”的策略确保了每次移动是必要且有效的，避免了重复和冗余的操作。
>
> 2. **为什么不选择预测下一步是否合规？**
>
> 虽然从逻辑上看，先预测下一步是否合规再决定是否移动可能显得更加“严谨”，但在实际操作中，这种方法往往会带来复杂的边界情况处理和多余的计算。
>
> - **复杂性增加**：如果你尝试在移动之前预先检查是否合规，你需要考虑多种可能的情况，比如下一个元素是否为 `0`，窗口是否需要缩小，左边界是否也需要调整等。每种情况都可能带来额外的条件判断，增加了代码的复杂性。
> - **操作冗余**：预先预测需要你在每次移动之前都进行一次完整的检查，可能会涉及到重复计算。而这些计算在实际移动后可能并不需要进行，从而浪费了资源。
> - **灵活性下降**：如果先预测再移动，你需要提前定义好所有可能的操作路径，这种方式在处理一些复杂或特殊情况下可能表现得不如先移动再调整的方法灵活。例如，当窗口中突然出现连续多个 `0` 时，先预测再移动的策略可能需要复杂的判断逻辑来确定如何操作，而先移动再调整的方法则可以自然而然地处理这些情况。
>
> 3. **为什么“先移动再检查”的策略足够好？**
>
> 先移动再检查实际上是一种**懒惰计算**的方式，即仅在需要时才做出调整，而不是提前计算。懒惰计算的好处是它避免了不必要的操作，保持了算法的简单和高效。即使在极端情况下（比如窗口内突然多了很多 `0`），你仍然可以通过调整左边界来保证窗口的合法性。
>
> 这种策略可以被视为一种“宽容”的方式，即允许窗口扩展，即使它可能会暂时超出限制，然后通过后续的调整来恢复窗口的合法性。这种方式不仅易于实现，还能保证在最坏情况下的效率。



## 8. Middle Node Of The Linked List

If there are two middle nodes, return **the second middle** node.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist1.jpg)

```
Input: head = [1,2,3,4,5]
Output: [3,4,5]
Explanation: The middle node of the list is node 3.
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist2.jpg)

```
Input: head = [1,2,3,4,5,6]
Output: [4,5,6]
Explanation: Since the list has two middle nodes with values 3 and 4, we return the second one.
```

 

**Constraints:**

- The number of nodes in the list is in the range `[1, 100]`.
- `1 <= Node.val <= 100`

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
var middleNode = function(head) {    
    let slow = head;
    let fast = head;
    
    while (fast && fast.next){
        slow = slow.next;
        fast = fast.next.next;
    }
    
    return slow;
    
};
```

## 9.  Remove Duplicates from Sorted List

Given the `head` of a sorted linked list, *delete all duplicates such that each element appears only once*. Return *the linked list **sorted** as well*.



**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/01/04/list1.jpg)

```
Input: head = [1,1,2]
Output: [1,2]
```

**Example 2:**

![img](https://assets.leetcode.com/uploads/2021/01/04/list2.jpg)

```
Input: head = [1,1,2,3,3]
Output: [1,2,3]
```

 

**Constraints:**

- The number of nodes in the list is in the range `[0, 300]`.
- `-100 <= Node.val <= 100`
- The list is guaranteed to be **sorted** in ascending order.

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
var deleteDuplicates = function(head) {
    let front = head ? head.next : head;
    let back = head;
    
    while(front && front.next){
        if(front.val===back.val){
            front = front.next;
            back.next = front;
        } else {
            front = front.next;
            back = back.next;
        }
    }
    
    if(front){
        if(front.val===back.val){
            back.next = null;
        }
    }
    
    return head
    
    
}
```

## 10. Running Sum of 1d Array

Given an array `nums`. We define a running sum of an array as `runningSum[i] = sum(nums[0]…nums[i])`.

Return the running sum of `nums`.

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: [1,3,6,10]
Explanation: Running sum is obtained as follows: [1, 1+2, 1+2+3, 1+2+3+4].
```

**Example 2:**

```
Input: nums = [1,1,1,1,1]
Output: [1,2,3,4,5]
Explanation: Running sum is obtained as follows: [1, 1+1, 1+1+1, 1+1+1+1, 1+1+1+1+1].
```

**Example 3:**

```
Input: nums = [3,1,2,10,1]
Output: [3,4,6,16,17]
```

 

**Constraints:**

- `1 <= nums.length <= 1000`
- `-10^6 <= nums[i] <= 10^6`

### My Solution

```javascript
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var runningSum = function(nums) {
    let sum = 0;
    let result = [];
    
    nums.forEach((value) => {
        sum += value;
        result.push(sum);
    })
    
    return result;
};
```

## 11. Minimum Value to Get Positive Step by Step Sum

Given an array of integers `nums`, you start with an initial **positive** value *startValue.*

In each iteration, you calculate the step by step sum of *startValue* plus elements in `nums` (from left to right).

Return the minimum **positive** value of *startValue* such that the step by step sum is never less than 1.

**Example 1:**

```
Input: nums = [-3,2,-3,4,2]
Output: 5
Explanation: If you choose startValue = 4, in the third iteration your step by step sum is less than 1.
step by step sum
startValue = 4 | startValue = 5 | nums
  (4 -3 ) = 1  | (5 -3 ) = 2    |  -3
  (1 +2 ) = 3  | (2 +2 ) = 4    |   2
  (3 -3 ) = 0  | (4 -3 ) = 1    |  -3
  (0 +4 ) = 4  | (1 +4 ) = 5    |   4
  (4 +2 ) = 6  | (5 +2 ) = 7    |   2
```

**Example 2:**

```
Input: nums = [1,2]
Output: 1
Explanation: Minimum start value should be positive. 
```

**Example 3:**

```
Input: nums = [1,-2,-3]
Output: 5
```

**Constraints:**

- `1 <= nums.length <= 100`
- `-100 <= nums[i] <= 100`

### My Solution

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var minStartValue = function(nums) {
    let sum = 0;
    let min_sum = 0;
    
    nums.forEach((value) => {
        sum += value;
        if(sum < min_sum){
            min_sum = sum;
        }
    })
    
    return 1 - min_sum;
};
```

## 12. K Radius Subarray Averages

You are given a **0-indexed** array `nums` of `n` integers, and an integer `k`.

The **k-radius average** for a subarray of `nums` **centered** at some index `i` with the **radius** `k` is the average of **all** elements in `nums` between the indices `i - k` and `i + k` (**inclusive**). If there are less than `k` elements before **or** after the index `i`, then the **k-radius average** is `-1`.

Build and return *an array* `avgs` *of length* `n` *where* `avgs[i]` *is the **k-radius average** for the subarray centered at index* `i`.

The **average** of `x` elements is the sum of the `x` elements divided by `x`, using **integer division**. The integer division truncates toward zero, which means losing its fractional part.

- For example, the average of four elements `2`, `3`, `1`, and `5` is `(2 + 3 + 1 + 5) / 4 = 11 / 4 = 2.75`, which truncates to `2`.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2021/11/07/eg1.png)

```
Input: nums = [7,4,3,9,1,8,5,2,6], k = 3
Output: [-1,-1,-1,5,4,4,-1,-1,-1]
Explanation:
- avg[0], avg[1], and avg[2] are -1 because there are less than k elements before each index.
- The sum of the subarray centered at index 3 with radius 3 is: 7 + 4 + 3 + 9 + 1 + 8 + 5 = 37.
  Using integer division, avg[3] = 37 / 7 = 5.
- For the subarray centered at index 4, avg[4] = (4 + 3 + 9 + 1 + 8 + 5 + 2) / 7 = 4.
- For the subarray centered at index 5, avg[5] = (3 + 9 + 1 + 8 + 5 + 2 + 6) / 7 = 4.
- avg[6], avg[7], and avg[8] are -1 because there are less than k elements after each index.
```

**Example 2:**

```
Input: nums = [100000], k = 0
Output: [100000]
Explanation:
- The sum of the subarray centered at index 0 with radius 0 is: 100000.
  avg[0] = 100000 / 1 = 100000.
```

**Example 3:**

```
Input: nums = [8], k = 100000
Output: [-1]
Explanation: 
- avg[0] is -1 because there are less than k elements before and after index 0.
```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 105`
- `0 <= nums[i], k <= 105`



### My Solution

```javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var getAverages = function(nums, k) {
    let sum = 0;
    let sums = [];
    let result = [];
    
    nums.forEach((value) => {
        sum += value;
        sums.push(sum);
    })
    nums.forEach((value, index, array) => {
        if (index < k || (index + k) > (array.length -1)){
            result.push(-1);
        } else {
            let count = 2 * k + 1;
            let sumSubArr = sums[index + k] - ((index - k - 1) < 0 ? 0 : sums[index - k - 1]);
            result.push(Math.trunc(sumSubArr / count));
        }
    })
    
    return result;
};
```

## 13. Subarray Product Less than K

Given an array of integers `nums` and an integer `k`, return *the number of contiguous subarrays where the product of all the elements in the subarray is strictly less than* `k`.

**Example 1:**

```
Input: nums = [10,5,2,6], k = 100
Output: 8
Explanation: The 8 subarrays that have product less than 100 are:
[10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6]
Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.
```

**Example 2:**

```
Input: nums = [1,2,3], k = 0
Output: 0
```

**Constraints:**

- `1 <= nums.length <= 3 * 104`
- `1 <= nums[i] <= 1000`
- `0 <= k <= 106`

### My Solution

```javascript
 /**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var numSubarrayProductLessThanK = function(nums, k) {
    let front = 0;
    let back = 0;
    let currentValue = 1;
    let count = 0;
    if (k <= 1){
        return 0;
    }
    while (front < nums.length){
        currentValue *= nums[front];
        while(currentValue >= k){
            currentValue = currentValue / nums[back];
            back ++;
        }

        count += front - back + 1;
        front ++;
    }


    return count;
    
};
```

> 关键点:  在这题条件中, 如果我们已经知道一个sub array满足 乘积小于k 的条件, 那么这个sub array的所有 sub array 也一定满足条件 ( 因为规定数组中的整数 >= 1)

## 14. Intersection Of Multiple Arrays

Given a 2D integer array `nums` where `nums[i]` is a non-empty array of **distinct** positive integers, return *the list of integers that are present in **each array** of* `nums` *sorted in **ascending order***.

**Example 1:**

```
Input: nums = [[3,1,2,4,5],[1,2,3,4],[3,4,5,6]]
Output: [3,4]
Explanation: 
The only integers present in each of nums[0] = [3,1,2,4,5], nums[1] = [1,2,3,4], and nums[2] = [3,4,5,6] are 3 and 4, so we return [3,4].
```

**Example 2:**

```
Input: nums = [[1,2,3],[4,5,6]]
Output: []
Explanation: 
There does not exist any integer present both in nums[0] and nums[1], so we return an empty list [].
```

 

**Constraints:**

- `1 <= nums.length <= 1000`
- `1 <= sum(nums[i].length) <= 1000`
- `1 <= nums[i][j] <= 1000`
- All the values of `nums[i]` are **unique**.

### My Solution

```javascript
/**
 * @param {number[][]} nums
 * @return {number[]}
 */
var intersection = function(nums) {
    let map = new Map();
    let keys = [];

    nums.forEach((value, index, array) => {
            value.forEach((subValue) => {
                if (map.has(subValue)){
                    let currentValue = map.get(subValue);
                    map.set(subValue, currentValue + 1);
                }else {
                    map.set(subValue, 1);
                }
            })
    })
    map.forEach((value, key) => {
        if (value === nums.length) {
            keys.push(key);
        }
    });

    return keys.sort((a, b) => {return a - b});
}
```

## 15. Check if All Characters Have Equal Number of Occurrences

Given a string `s`, return `true` *if* `s` *is a **good** string, or* `false` *otherwise*.

A string `s` is **good** if **all** the characters that appear in `s` have the **same** number of occurrences (i.e., the same frequency).

**Example 1:**

```
Input: s = "abacbc"
Output: true
Explanation: The characters that appear in s are 'a', 'b', and 'c'. All characters occur 2 times in s.
```

**Example 2:**

```
Input: s = "aaabb"
Output: false
Explanation: The characters that appear in s are 'a' and 'b'.
'a' occurs 3 times while 'b' occurs 2 times, which is not the same number of times.
```

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consists of lowercase English letters.

### My Solution

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var areOccurrencesEqual = function(s) {
    let map = new Map();
    for(let char of s){
        if(map.has(char)){
            let curr = map.get(char);
            map.set(char, curr + 1);
        } else {
            map.set(char, 1);
        }
    }

    return areAllValuesEqual(map);
};

function areAllValuesEqual(map) {
    const values = Array.from(map.values()); 

    if (values.length === 0) {
        return true;
    }

    const firstValue = values[0];

    return values.every(value => value === firstValue); 
}

```

## 16. Subarray Sum Equals K

Given an array of integers `nums` and an integer `k`, return *the total number of subarrays whose sum equals to* `k`.

A subarray is a contiguous **non-empty** sequence of elements within an array.

 

**Example 1:**

```
Input: nums = [1,1,1], k = 2
Output: 2
```

**Example 2:**

```
Input: nums = [1,2,3], k = 3
Output: 2
```

**Constraints:**

- `1 <= nums.length <= 2 * 104`
- `-1000 <= nums[i] <= 1000`
- `-107 <= k <= 107`
