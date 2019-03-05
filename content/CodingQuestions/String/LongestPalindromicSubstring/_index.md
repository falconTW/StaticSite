---
title: "LongestPalindromicSubstring"
date: 2018-10-06T21:29:32+08:00
---

[Question Link](https://leetcode.com/problems/longest-palindromic-substring/description/)

Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

```
Example 1:

Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
Example 2:

Input: "cbbd"
Output: "bb"
```


## Solution1

This is a classic dynamic programming problem.

We must figure out what **Subproblem** and **Optimal structure**.

The optimal solution can derived from every Optimal solution of the subproblems. 

Here we can easily figure out that the Longest palindromic string is related to all the short palindromic string,

Let P[i,j] =  is section [i,j] is a palindrome.

For example : 
S= a b c c b
Index = 0 1 2 3 4

We can see that 

P[0,0] =1 //each char is a palindrome
P[0,1] =S[0] == S[1] , P[1,1] =1
P[0,2] = S[0] == S[2] && P[1,1], P[1,2] = S[1] == S[2] , P[2,2] = 1
P[0,3] = S[0] == S[3] && P[1,2], P[1,3] = S[1] == S[3] && P[2,2] , P[2,3] =S[2] ==S[3], P[3,3]=1

Here we can conclude these examples to rules.

P[i,j] = 1 if i ==j
= S[i] ==S[j] if j = i+1
= S[i] == S[j] && P[i+1][j-1] if j>i+1

```C++
 string longestPalindrome(string s) 
{
    if (s.size() <= 1)
	 {
		 return s;
	 }

	 int start = 0;
	 int end = 0;
	 int currentMaxLen = 0;

	 vector<vector<bool>> dp(s.size(), vector<bool>(s.size(), false));

	 for (int i = 0; i < s.size(); ++i)
	 {
		 for (int j = 0; j < i; ++j)
		 {
			 dp[j][i] = s[i] == s[j] && (i - j <= 2 || dp[j + 1][i - 1]);
			 if (dp[j][i])
			 {
				 if (currentMaxLen<i - j + 1)
				 {
					 start = j;
					 end = i;
					 currentMaxLen = i - j + 1;
				 }
			 }
		 }
		 dp[i][i] = true;
	 }

	 return s.substr(start, end-start+1); 
     // Should not replace end-start+1 with currentMaxLen since in case "ac",currentMaxLen will be zero but still we need one char as answer
 }
```