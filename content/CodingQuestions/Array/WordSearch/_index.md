---
title: "WordSearch"
date: 2018-09-28T02:19:52+08:00
---

[Questions Link](https://leetcode.com/problems/word-search/description/)

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

Example:

```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

## Solution 1
We need to start from everypoint and search in four directions to check if there is word matched, in this way,

we apply DFS and backtracking to this problem, and initialize a 'visited' array to keep recording where this entry is visited 

for traversing the whole board.


```C++
bool exist(vector<vector<char>>& board, string word)
{
    if (board.empty() || board[0].empty())
    {
        return false;
    }

    vector<vector<bool>> visited(board.size(), vector<bool>(board[0].size(), false));

    for (int i = 0; i<board.size(); ++i)
    {
        for (int j = 0; j<board[0].size(); ++j)
        {
            if (searchWord(board,word,0,i, j, visited))
            {
                return true;
            }
        }
    }
    
    return false;
}

bool searchWord(vector<vector<char>> &board,string word,int idx,int i, int j, vector<vector<bool>> &visited)
{
    if (idx == word.size())
    {
        return true;
    }

    if (i < 0 || j < 0 || i > board.size() - 1 || j > board[0].size() - 1 || visited[i][j] || board[i][j] != word[idx])
    {
        return false;
    }
    
    visited[i][j] = true;
    bool res = searchWord(board, word, idx + 1, i - 1, j, visited)
        || searchWord(board, word, idx + 1, i + 1, j, visited)
        || searchWord(board, word, idx + 1, i, j - 1, visited)
        || searchWord(board, word, idx + 1, i, j + 1, visited);
    visited[i][j] = false;
    
    return res;
}
```

