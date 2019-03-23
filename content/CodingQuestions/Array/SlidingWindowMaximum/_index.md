---
title: "SlidingWindowMaximum"
date: 2019-03-23T15:22:59+08:00
draft: false
---

```
Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.

Example:

Input: nums = [1,3,-1,-3,5,3,6,7], and k = 3
Output: [3,3,5,5,6,7] 
Explanation: 

Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
Note: 
You may assume k is always valid, 1 ≤ k ≤ input array's size for non-empty array.

Follow up:
Could you solve it in linear time?
```

## Solution 1 

Using Deque to solve this question can restrict the time complexity in linear.

The peak element is the start of a descending sequence, so that's what we should take a look at.

We store the indexes of the descending sequence to the deque, when the index of the frontiest element equals to i-k which 

means it's on the verge of the window. 


When the current element is larger than the frontiset element, we should clear 

the whole deque, otherise we push the index of the current element to the deque.

The frontiest element will be pushsed into the result vector when i>=k-1;


```C++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
      
		if (nums.empty()) {
			return vector<int>{};
		}

		if (nums.size() == 1) {
			return vector<int>{nums[0]};
		}
		
		deque<int> dq;
		vector<int> res;

		for (int i = 0; i < nums.size(); ++i) {
			if (!dq.empty() && dq.front() == i - k) {
				dq.pop_front();
			}

			while (!dq.empty() && nums[i] > nums[dq.back()]) {
				dq.pop_back();
			}

			dq.push_back(i);

			if (i >= k-1) {
				res.push_back(nums[dq.front()]);
			}
		}
		return res;
    }
};
```