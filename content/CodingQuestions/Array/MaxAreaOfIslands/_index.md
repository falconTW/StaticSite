---
title: "MaxAreaOfIslands"
date: 2018-10-18T23:36:36+08:00
---

[Question Link](https://leetcode.com/problems/max-area-of-island/submissions/)

Given a non-empty 2D array grid of 0's and 1's, an island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Find the maximum area of an island in the given 2D array. (If there is no island, the maximum area is 0.)

```
Example 1:

[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
Given the above grid, return 6. Note the answer is not 11, because the island must be connected 4-directionally.
Example 2:

[[0,0,0,0,0,0,0,0]]
Given the above grid, return 0.
Note: The length of each dimension in the given grid does not exceed 50.
```

# Solution 1
DFS will work. This is a simpler problem of [Word Search](https://leetcode.com/problems/word-search/description/)

```C++
int maxAreaOfIsland(vector<vector<int>>& grid) 
    {
        int maxVal =0;
        
        for(int i=0;i<grid.size();++i)
            for(int j=0;j<grid[i].size();++j)
            {
                if(grid[i][j]==1)
                {
                    maxVal = max(maxVal,areaOfIsland(grid,i,j));
                }
            }
        
        return maxVal;
    }
    
    int areaOfIsland(vector<vector<int>> &grid, int i, int j)
    {        
        if(i>=0&&i<grid.size() && j>=0 &&j<grid[i].size() && grid[i][j]==1)
        {
            grid[i][j]=0;
            return 1+areaOfIsland(grid,i+1,j)+areaOfIsland(grid,i-1,j)
                +areaOfIsland(grid,i,j+1)+areaOfIsland(grid,i,j-1);
        }
        return 0;
    }
```