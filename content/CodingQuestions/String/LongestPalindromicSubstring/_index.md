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


##Solution

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

