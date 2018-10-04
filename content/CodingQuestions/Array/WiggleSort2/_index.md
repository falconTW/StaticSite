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

To make the array wiggle like, [Stefan Pochmann](https://discuss.leetcode.com/topic/32929/o-n-o-1-after-median-virtual-indexing) 's post 

introduced a way called virtual indexing to reorder the map position and the index itself.

Consider 

```
A(i) = nums[(2*i+1)/(n|1)]
```

This helps us to recorder the element and mapping with wiggle order.

```
  A(0) – > nums[1] 
  A(1) – > nums[3] 
  A(2) – > nums[5] 
  A(3) – > nums[7] 
  A(4) – > nums[9] 
  A(5) – > nums[0] 
  A(6) – > nums[2] 
  A(7) – > nums[4] 
  A(8) – > nums[6] 
  A(9) – > nums[8]
```

Our target is to make the 1,3,5,7,... elements always bigger than the 0,2,4,6,8 elements.

Then let's focus on the A(i) array, in the new A(i) array, we need to place the ones bigger than the median starting from the front 

and place the ones smaller than the median from the last position. In this way, we can ensure the odd-indexed elemnt is always larger

than the even-indexed element. To acheive this, we can refer to the [Sort color](https://falcontw.me/codingquestions/array/sortcolors/); problem



```C++
class Solution {
public:
    int newIndex(int index, int n)
    {
        return (1 + 2 * index) % (n | 1);
    }

    void wiggleSort(vector<int> &nums)
    {
        int size = nums.size();
        int front = 0;
        int mid = (size+1) / 2;

        int pos = 0;

        nth_element(nums.begin(), nums.begin() + mid, nums.end()); // Quick select to partition the array base on given pivot 

        int left = 0, i = 0, right = size - 1;
        int median = nums[mid];
        
        while (i <= right) // Sort color
        {
            if (nums[newIndex(i,size)] > median) 
            {
                swap(nums[newIndex(left++,size)], nums[newIndex(i++,size)]);
            }
            else if (nums[newIndex(i,size)] < median) 
            {
                swap(nums[newIndex(right--,size)], nums[newIndex(i,size)]);
            }
            else 
            {
                i++;
            }
        }
    }
};
```