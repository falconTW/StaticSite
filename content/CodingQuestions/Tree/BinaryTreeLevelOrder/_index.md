---
title: "BinaryTreeLevelOrder"
date: 2018-09-24T10:13:43+08:00
---

[Questions Link](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)

Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree [3,9,20,null,null,15,7],

```
    3
   / \
  9  20
    /  \
   15   7
return its level order traversal as:
[
  [3],
  [9,20],
  [15,7]
]
```


## Solution 1  

BFS is the one of the most common ways to solve this questions.

```C++
vector<vector<int>> levelOrder(TreeNode* root) 
{
    vector<vector<int>> totalResult;
    if (root == NULL)
    {
        return totalResult;
    }

    queue<TreeNode*> q;
    q.push(root);

    while (!q.empty())
    {
        int size = q.size();
        vector<int> res;
        for (int i = 0; i < size; ++i)
        {
            TreeNode* node = q.front();
            res.push_back(node->val);
            q.pop();
            if (node->left)
            {
                q.push(node->left);
            }
            if (node->right)
            {
                q.push(node->right);
            }
        }
        totalResult.push_back(res);
    }

    return totalResult;
}
```
