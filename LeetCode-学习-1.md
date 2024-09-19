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



