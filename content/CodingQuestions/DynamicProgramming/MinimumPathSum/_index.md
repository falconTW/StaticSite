---
title: "MinimumPathSum"
date: 2019-02-08T23:37:36+08:00
draft: false
---

[QuestionLink](https://leetcode.com/problems/minimum-path-sum/)

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

```
Example:

Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.
```

## Solution 1

This is a simple dynamic programming problem.

We create a 2D array to store the current maximum path sum and return the most right-bottom value.

```C++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size();
	int n = grid[0].size();

	vector<vector<int>> dp(m, vector<int>(n,0));

	dp[0][0] = grid[0][0];

	for (int i = 0; i < m; ++i) {
		for (int j = 0; j < n; ++j) {
			if (i == 0 && j == 0) {
				dp[i][j] = grid[i][j];
				continue;
			}
			else if (i == 0) {
				dp[i][j] = grid[i][j] + dp[i][j - 1];
			}
			else if (j == 0) {
				dp[i][j] = grid[i][j] + dp[i - 1][j];
			}
			else {
				dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
			}
		}
	}

	return dp[m - 1][n - 1];
    }
};
```