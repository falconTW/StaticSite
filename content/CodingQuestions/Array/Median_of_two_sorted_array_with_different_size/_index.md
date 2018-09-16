---
title: "Median of 2 sorted array"
date: 2018-08-31T00:03:02+08:00
---

[Questions Link](https://leetcode.com/problems/median-of-two-sorted-arrays/description/)

```
Example 1:

nums1 = [1, 3]
nums2 = [2]

The median is 2.0
Example 2:

nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5

There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

You may assume nums1 and nums2 cannot be both empty.
```


## Solution

This is indeed an kth smalleset element questions. The kth value turns to be median in this case.

Since we need to implement this within logarithmic time. We need to take advantage of **Binary Search**.

The point is that **it's not what is left but what we delete** matters. 

Suppose there is two array called A and B.

A: 1,3,5,6,10. 

B: 2,6,9,13

and we wanna find the **7th** element.

7/2 =3, So we take out the 3th element from A and B.

A[2] = 5, B[2] =9

Here we can see that B[2]>A[2] which means the 7th elements won't be in the top 3 elements of A.

Then we can remove the top 3 elemets of A and the problem is reduced to finding K = 7-K/2 = 4;

A: 6,10 

B: 2,6,9,13

K/2 = 2 => A[1] = 10, B[2] = 9;

A[1] > B[1]; Remove the top elements of B 

K= 4-2=2

K/2 = 1

A: 6,10 B: 9,13 A[0] = 6, B[0]=9

B[0]>A[0] So we remove A[0] 

A: 10 B: 9,13 K = 2-1 = 1

Now we can return the smallest one as the answer.

Note that there will possily be **Corner cases**

1.one array is empty, return the kth number of the nonempty array. 
</br>
2.even number of total elements,we need to find two middle numbers and calculate the median. 
</br>
3.k may be larger than the length of the smaller array.


```C++

	double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) 
	{

		int total = nums1.size() + nums2.size();
		if (total % 2 != 0)
		{
			return findKth(nums1, nums2, total / 2 + 1);
		}
		else
		{
			return (findKth(nums1, nums2, total / 2) + findKth(nums1, nums2, total / 2 + 1)) *0.5;
		}
	}

	int findKth(vector<int> nums1, vector<int> nums2, int k) 
	{

		int m = nums1.size();
		int n = nums2.size();

		if (m > n)
			return findKth(nums2, nums1, k);
		if (m == 0)
			return nums2[k - 1];
		if (k == 1)
			return min(nums1[0], nums2[0]);
		int i = min(m, k / 2), j = min(n, k / 2);

		if (nums1[i - 1] > nums2[j - 1]) 
		{
			return findKth(nums1, vector<int>(nums2.begin() + j, nums2.end()), k - j);
		}
		else 
		{
			return findKth(vector<int>(nums1.begin() + i, nums1.end()), nums2, k - i);
		}
		return 0;
	}
	
```