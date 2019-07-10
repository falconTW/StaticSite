---
title: "WildCardMatching"
date: 2019-06-15T15:50:10+08:00
draft: false
---

Given an input string (s) and a pattern (p), implement wildcard pattern matching with support for '?' and '*'.

'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
The matching should cover the entire input string (not partial).

Note:

s could be empty and contains only lowercase letters a-z.
p could be empty and contains only lowercase letters a-z, and characters like ? or *.
Example 1:

```
Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
Example 2:

Input:
s = "aa"
p = "*"
Output: true
Explanation: '*' matches any sequence.
Example 3:

Input:
s = "cb"
p = "?a"
Output: false
Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.
Example 4:

Input:
s = "adceb"
p = "*a*b"
Output: true
Explanation: The first '*' matches the empty sequence, while the second '*' matches the substring "dce".
Example 5:

Input:
s = "acdcb"
p = "a*c?b"
Output: false
```

## Question 1

```
我們使用一個二維 dp 數組，其中 dp[i][j] 表示 s中前i個字符組成的子串和p中前j個字符組成的子串是否能匹配。
大小初始化為 (m+1) x (n+1)，加1的原因是要包含 dp[0][0] 的情況，因為若s和p都為空的話，也應該返回 true
所以也要初始化 dp[0][0] 為 true。還需要提前處理的一種情況是，當s為空，p為連續的星號時的情況。由於星號是可以代表空串的，所以只要s為空
那麼連續的星號的位置都應該為 true，所以我們現將連續星號的位置都賦為 true。

然後就是推導一般的狀態轉移方程了，如何更新 dp[i][j]，首先處理比較 tricky 的情況，若p中第j個字符是星號，由於星號可以匹配空串，所以如果p中的前 j-1 個字符跟s中前i個字符匹配成功了（ dp[i][j-1] 為true）的話，那麼 dp[i][j] 也能為 true。或者若p中的前j個字符跟s中的前i-1個字符匹配成功了（ dp[i-1][j] 為true ）的話，那麼 dp[i][j] 也能為 true（因為星號可以匹配任意字符串，再多加一個任意字符也沒問題）。若p中的第j個字符不是星號，對於一般情況，我們假設已經知道了s中前 i-1 個字符和p中前 j-1 個字符的匹配情況（即 dp[i-1][j-1] ），那麼現在只需要匹配s中的第i個字符跟p中的第j個字符，若二者相等（ s[i-1] == p[j-1] ），或者p中的第j個字符是問號（ p[j-1] == '?' ）

class Solution {
public:
	bool isMatch(string s, string p) {
		int m = s.size();
		int n = p.size();

		//Let dp denotes whether s[i] matches p[j] or not.
		vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));

		dp[0][0] = true;

		for (int i = 1; i <= n; ++i) {
			if (p[n - 1] == '*') {
				dp[0][i] = dp[0][i - 1];
			}
		}

		for (int i = 1; i <= m; ++i) {
			for (int j = 1; j <= n; ++j) {
				if (p[j - 1] == '*') {
					dp[i][j] = dp[i][j - 1] || dp[i - 1][j];
				}
				else {
					dp[i][j] = (s[i - 1] == p[j - 1] || p[j - 1] == '?') && dp[i - 1][j - 1];
				}
			}
		}
		return dp[m][n];
	}
};
```