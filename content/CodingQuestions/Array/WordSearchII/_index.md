---
title: "WordSearchII"
date: 2019-05-04T20:09:58+08:00
draft: false
---

```
Given a 2D board and a list of words from the dictionary, find all words in the board.

Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

 

Example:

Input: 
board = [
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]
words = ["oath","pea","eat","rain"]

Output: ["eat","oath"]
 

Note:

All inputs are consist of lowercase letters a-z.
The values of words are distinct.
```


## Solution 1

This question is similar to Word search, however, this question want further gather all the words in the board.

To get better time complexity, iterative ways are not accepted, we need to transform the input into prefix tree first then do word search.

```C++
class Solution {
public:
	struct TrieNode {
		TrieNode *child[26];
		string str;
		bool isEndOfWord;
		TrieNode() :str("") {
			for (auto &c : child) {
				c = NULL;
			}
		}
	};

	void InsertTrieNode(TrieNode* root, string str) {
		TrieNode* p = root;
		for (auto &c : str) {
			if (p->child[c - 'a'] == NULL) {
				p->child[c - 'a'] = new TrieNode();
			}
			p = p->child[c - 'a'];
		}
		p->str = str;
	}

	vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
		vector<string> res;
		if (words.empty() || board.empty() || board[0].empty()) {
			return res;
		}
		vector<vector<bool>> visited(board.size(), vector<bool>(board[0].size(), false));

		TrieNode* root = new TrieNode();
		for (auto &s : words) {
			InsertTrieNode(root, s);
		}

		for (int i = 0; i < board.size(); ++i) {
			for (int j = 0; j < board[0].size(); ++j) {
				if (root->child[board[i][j] - 'a'] != NULL) {
					search(board, visited, root, res, i, j);
				}
			}
		}
		return res;
	}

	void search(vector<vector<char>>& board, vector<vector<bool>>& visited, TrieNode *p, vector<string> &res, int i, int j) {
		if (!p->str.empty()) {
			res.push_back(p->str);
			p->str.clear();
			return;
		}

		if (i<0 || j<0 || i>board.size()-1 || j>board[0].size()-1) {
			return;
		}

		if (visited[i][j] || p->child[board[i][j] - 'a'] == NULL){
			return;
		}

		visited[i][j] = true;
		search(board, visited, p->child[board[i][j] - 'a'], res, i + 1, j);
		search(board, visited, p->child[board[i][j] - 'a'], res, i , j+1);
		search(board, visited, p->child[board[i][j] - 'a'], res, i -1, j);
		search(board, visited, p->child[board[i][j] - 'a'], res, i , j-1);
		visited[i][j] = false;
		return;
	}

};
```

