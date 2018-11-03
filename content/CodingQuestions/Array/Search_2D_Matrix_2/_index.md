---
title: "Search_2D_Matrix_2"
date: 2018-10-16T21:25:00+08:00
---

[Question Link](https://leetcode.com/problems/search-a-2d-matrix-ii/description/)

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

Integers in each row are sorted in ascending from left to right.
Integers in each column are sorted in ascending from top to bottom.
```
Example:

Consider the following matrix:

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
Given target = 5, return true.

Given target = 20, return false.
```

# Solution1
The matrix is ascending from left to right, from top to bottom.

So we can start from the upper right element to shrink the search space.

```C++
 bool searchMatrix(vector<vector<int>>& matrix, int target) 
 {

	 int x = 0;
	 int y = matrix[0].size() - 1;

	 int row = matrix.size();
	 int col = matrix[0].size();

	 while (x < row && y > 0)
	 {
		 if (matrix[x][y] == target)
		 {
			 return true;
		 }
		 else if (matrix[x][y] > target)
		 {
			 y--;
		 }
		 else 
		 {
			 x++;
		 }
	 }


	 return false;

 }
 ```