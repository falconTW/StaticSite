---
title: "KthSmallestElementInBST"
date: 2018-10-06T13:31:05+08:00
---

[Questions Link](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/)

Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.

Note: 
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

```
Example 1:

Input: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
Output: 1
Example 2:

Input: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
Output: 3
```

Follow up:
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?


##Solution 1

Inorder traverse will do. Pass the k value to each recursive call and return when the 

```C++
int kthSmallest(TreeNode* root, int k)
	{
		int ans = 0;

		if (root == NULL)
		{
			return 0;
		}
		helper(root, k, ans);

		return ans;
	}

	void helper(TreeNode* root, int &k,int &ans)
	{
		if (root->left != NULL)
		{
			helper(root->left, k,ans);
		}

		if (k == 0)
		{
			ans = root->val;
		}

		k = k - 1;

		if (root->right != NULL)
		{
			helper(root->right, k, ans);
		}


	}
```
##Solution 2
In the followup part, if the BST is modified often, the cost of traversing will be expensive. 

Since the kth smallest element has k-1 nodes in its left-hand side,we may use Divide and Conquer to squeeze out the value.

```c++
int kthSmallest(TreeNode* root, int k)
	{
		int count = countNodes(root->left);

		if (count == k - 1)
		{
			return root->val;
		}
		else if (count < k - 1)
		{
			return kthSmallest(root->right,k-count-1 );
		}
		else
		{
			return kthSmallest(root->left, k);
		}
	}

	int countNodes(TreeNode* root)
	{
		if (root == NULL)
		{
			return 0;
		}

		return 1 + countNodes(root->left) + countNodes(root->right);
	}
```