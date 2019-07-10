---
title: "MinimumSizeSubSum"
date: 2019-07-07T22:44:16+08:00
draft: false
---

Given an array of n positive integers and a positive integer s, find the minimal length of a contiguous subarray of which the sum ≥ s. If there isn't one, return 0 instead.

Example: 

Input: s = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: the subarray [4,3] has the minimal length under the problem constraint.
Follow up:
If you have figured out the O(n) solution, try coding another solution of which the time complexity is O(n log n). 


# Solution 1 O(n)

```C++
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
     int start = 0;
		int res = INT_MAX;
		int sum = 0;
		for (int i = 0; i < nums.size(); ++i) {
			sum += nums[i];
			while (sum >= s && start<=i) {
				res = min(res, i - start + 1);
				sum -= nums[start];
				start++;
			}
		}

		return res == INT_MAX ? 0 : res;
    }
};
```
