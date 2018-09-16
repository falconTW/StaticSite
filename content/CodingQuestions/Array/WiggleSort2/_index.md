---
title: "WiggleSort II"
date: 2018-09-16T23:31:53+08:00
---

[Questions Link](https://leetcode.com/problems/wiggle-sort-ii/)

Given an unsorted array nums, reorder it such that nums[0] < nums[1] > nums[2] < nums[3]....

```
Example 1:

Input: nums = [1, 5, 1, 1, 6, 4]
Output: One possible answer is [1, 4, 1, 5, 1, 6].
Example 2:

Input: nums = [1, 3, 2, 2, 3, 1]
Output: One possible answer is [2, 3, 1, 3, 1, 2].
```

## Solution

Sol A:

We can simply sort this array and reorganize the array.

After we sort the array, the array will be partitioned as the smaller part mid and the larger part.

We pick the last element from the smaller part as the 1st element, then we pick another last element from the larger part as the 2nd element.

This 
