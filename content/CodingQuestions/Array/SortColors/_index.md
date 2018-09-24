---
title: "SortColors"
date: 2018-09-22T22:06:32+08:00
---

[Questions Link](https://leetcode.com/problems/sort-colors/description/)


Given an array with n objects colored red, white or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Note: You are not suppose to use the library's sort function for this problem.

```
Example:

Input: [2,0,2,1,1,0]

Output: [0,0,1,1,2,2]

Follow up:
```

A rather straight forward solution is a two-pass algorithm using counting sort.
First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
Could you come up with a one-pass algorithm using only constant space?


## Solution 1

The two pass method is simple. We may just try to record the times each number appears.

And restore the numbers back to the result.

```C++
class Solution {
public:
    void sortColors(int A[], int n) {
        int count[3] = {0}, idx = 0;
        for (int i = 0; i < n; ++i) ++count[A[i]];
        for (int i = 0; i < 3; ++i) {
            for (int j = 0; j < count[i]; ++j) {
                A[idx++] = i;
            }
        }
    }
};
```

## Solution 2

Since the order is not strictly ascending,so we don't really need to sort the whole array,

what we need to come to in mind is the method in Quick sort which helps to partition the array 

according to the chosen pivot.

```C++
void sortColors(vector<int>& nums) 
    {
        int left =0;
        int right =nums.size()-1;
        int pivot=0;
        
        while(left<=right)
        {
            if(nums[left]==2)
            {
                swap(nums[left],nums[right--]);
               
            }
            else if(nums[left]==0)
            {
                swap(nums[left++],nums[pivot++]);
            }
            else
            {
                left++;
            }
            
        }
        
    }
```