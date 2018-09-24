---
title: "ValidateBST"
date: 2018-09-22T18:26:45+08:00
---

[Questions Link](https://leetcode.com/problems/validate-binary-search-tree/)

```
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
```

```
Example 1:

Input:
    2
   / \
  1   3
Output: true
```


To validate a Binary Search Tree, we can either utilize its property to validate 

or we can have inorder traverse to see if the previous element is smaller than current one,

if not, that implies this BST is invalid.


## Solution 1

We validate each node of the tree to see if it meets with the property.

```C++
class Solution {
public:
    bool isValidBST(TreeNode* root) 
    {
        return checkValidBST(root,LONG_MIN,LONG_MAX);
    }
    
    bool checkValidBST(TreeNode* root,long min,long max)
    {
        if(root==NULL)
        {
            return true;
        }
        
        if(root->val <= min || root->val >=max)
        {
            return false;
        }
        
        return checkValidBST(root->left,min,root->val) && checkValidBST(root->right,root->val,max);
    }
};
```

## Solution 2

We traverse the whole tree with inorder and see if the prev element is always smaller than current one.

```c++
class Solution {
public:
    bool isValidBST(TreeNode* root) 
    {
        long prev = LONG_MIN;
        
        return inorderTraverse(root,prev);
    }
    
    bool inorderTraverse(TreeNode* root, long& prev)
    {
        if(root==NULL)
        {
            return true;
        }
        
        if(!inorderTraverse(root->left,prev))
        {
            return false;
        }
        
        if(prev >=root->val)
        {
            return false;
        }
        
        prev = root->val;
        
        if(!inorderTraverse(root->right,prev))
        {
            return false;
        }
        
        return true;
        
    }
};
```

