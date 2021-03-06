---
title: "NumberOfIslands"
date: 2018-10-25T01:35:33+08:00
---

[Questions Link](https://leetcode.com/problems/number-of-islands/)

Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

```
Example 1:

Input:
11110
11010
11000
00000

Output: 1
Example 2:

Input:
11000
11000
00100
00011

Output: 3
```

BFS will do. Just like the word search

```c++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) 
    {
        int islandCount=0;
        
        for(int i =0;i<grid.size();++i)
            for(int j=0;j<grid[i].size();++j)
            {
                if(grid[i][j]=='1')
                {
                    bfsIsland(grid,i,j);
                    ++islandCount;
                }
            }
        
        return islandCount;
    }
    
    void bfsIsland(vector<vector<char>> &grid, int i, int j)
    {
        if(i<0||j<0||i>grid.size()-1||j>grid[i].size()-1)
        {
            return;
        }
        
        if(grid[i][j]=='0'||grid[i][j]=='#')
        {
            return;
        }
        
        if(grid[i][j]=='1')
        {
            grid[i][j]='#';
        }
        
        bfsIsland(grid,i+1,j);
        bfsIsland(grid,i-1,j);
        bfsIsland(grid,i,j+1);
        bfsIsland(grid,i,j-1); 
    }
};
```