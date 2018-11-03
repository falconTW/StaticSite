---
title: "Search_2D_Matrix_1"
date: 2018-10-13T23:51:24+08:00
---

[Question Link](https://leetcode.com/problems/search-a-2d-matrix/)

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.
```
Example 1:

Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
Output: true
Example 2:

Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
Output: false
```

# Solution 1
Binary search will do the job.
```C++
bool searchMatrix(vector<vector<int>>& matrix, int target)
{
	int m = matrix.size();
	int n = matrix[0].size();

	if (target<matrix[0][0] || target > matrix.back().back())
	{
		return false;
	}

	int start = 0;
	int end = m * n - 1;

	while (start <= end)
	{
		int mid = (start + end) / 2;
		int value = matrix[mid / n][mid%n];

		if (value == target)
		{
			return true;
		}
		else if (value < target)
		{
			start = mid + 1;
		}
		else
		{
			end = mid - 1;
		}
	}

	return false;
}
```