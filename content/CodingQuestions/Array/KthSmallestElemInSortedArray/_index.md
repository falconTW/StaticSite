---
title: "KthSmallestElemInSortedArray"
date: 2019-05-08T21:08:36+08:00
draft: true
---

```
Given a n x n matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

Example:

matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.
Note: 
You may assume k is always valid, 1 ≤ k ≤ n2.
```

# Solution 1
The main issue of this problem is that the last element of a row isn't necessary smaller than 

the first element of next row, thus directly using Binay search won't work here.

The intuitive solution is trying to push all the element into a ```MaxHeap```, 

and pop when the whole tree size is larger than K.

The answer will be the top element eventually.


```C++
	int kthSmallest(vector<vector<int>>& matrix, int k) {
		priority_queue<int> queue;

		for (int i = 0; i < matrix.size(); ++i) {
			for (int j = 0; j < matrix[0].size(); ++j) {
				queue.push(matrix[i][j]);
				if (queue.size() > k) {
					queue.pop();
				}
			}
		}
		

		return queue.top();
	}
```

## Solution 2

Using binary search to shirnk search space.

In this case , we are using value instead of index to perform binary search, noted that when ```num``` equals to k,

it doesn't necessary represent the actual result we want. We need to clamp the value.

So the answer will be the ```left``` bound as while loop ends.

```C++
class Solution {
public:
	int kthSmallest(vector<vector<int>>& matrix, int k) {
		int n = matrix.size(), left = matrix[0][0], right = matrix[n - 1][n - 1];
		while (left <= right)
		{
			long mid = (left + right) / 2, num = 0;
			for (int i = 0; i < n; i++)
			{
				auto it = upper_bound(matrix[i].begin(), matrix[i].end(), mid);
				num += it - matrix[i].begin();
			}
			if (num < k) {
				left = mid + 1;
			}
			else if (num == k) {
				right = mid - 1;
			}
			else {
				right = mid - 1;
			}
		}
		return left;
	}
};
```


