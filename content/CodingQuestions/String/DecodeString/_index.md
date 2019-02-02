---
title: "DecodeString"
date: 2019-02-02T22:15:59+08:00
draft: false
---

Given an encoded string, return it's decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like 3a or 2[4].

Examples:

s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".


## Solution 1 
This is a traditional recursive problem. We can use stack or recursion to solve.

We push parameter for calculation to stack when left bracket appears and pop to calculate while right bracket is met.

```
  
   void decode(string &str,int i,int times) {

	int start = 0;
	string targetString = "";

	while (i < str.size()) {
		if (str[i] >= '0'&&str[i] <= '9') {
			string numStr="";
			while (str[i] >= '0'&&str[i] <= '9') {
				numStr += str[i];
				i++;
			}
			times = stoi(numStr);
			decode(str, i, times);
			i = start;
		}
		else if ((str[i] >= 'a'&&str[i] <= 'z') || (str[i] >= 'A'&&str[i] <= 'Z')) {
			while ((str[i] >= 'a'&&str[i] <= 'z') || (str[i] >= 'A'&&str[i] <= 'Z')) {
				targetString += str[i];
				i++;
			}
		}
		else if (str[i] == '[') {
			start = i - 1;
			++i;
		}
		else if (str[i] == ']') {
			str.replace(start, i-start + 1, "");
			string finalString = "";
			for (auto index = 0; index < times; ++index) {
				finalString += targetString;
			}
			str.insert(start, finalString);
			return;
		}
	}
}
    
    string decodeString(string s) {
        
        if (s.empty()) {
            return s;
        }

        int i=0;
        int start=0;
        int times=0;
        
        decode(s,i,times);
        return s;
    }
```

## Solution 2 

Re-write the stack solution to stack version.


```
 struct decodeParam {
	int Start = 0;
	int End = 0;
	int Times = 0;
	string TargetString = "";
};
    string decodeString(string str) {
        
	if (str.empty()) {
		return str;
	}

	stack<decodeParam*> parseStack;
	int i = 0;

	while (i < str.size()) {
		if (str[i] >= '0'&&str[i] <= '9') {
			decodeParam* param = new decodeParam();
            if (!parseStack.empty()) {
				if (parseStack.top()->Start == i) {
					while (str[i] >= '0'&&str[i] <= '9') {
						++i;
					}
					continue;
				}
			}
			param->Start = i;
			string numStr = "";
			while (str[i] >= '0'&&str[i] <= '9') {
				numStr += str[i];
				++i;
			}
			param->Times = stoi(numStr);
			parseStack.push(param);
		}
		else if ((str[i] >= 'a'&&str[i] <= 'z') || (str[i] >= 'A'&&str[i] <= 'Z')) {

			if (parseStack.empty()) {
				i++;
				continue;
			}

			string targetStr = "";
			while ((str[i] >= 'a'&&str[i] <= 'z') || (str[i] >= 'A'&&str[i] <= 'Z')) {
				targetStr += str[i];
				++i;
			}
			decodeParam* param = parseStack.top();
			param->TargetString = targetStr;
		}
		else if (str[i] == '[') {
			++i;
		}
		else if (str[i] == ']') {
			decodeParam* param = parseStack.top();
			param->End = i;
			str.replace(param->Start, param->End-param->Start+1, "");
			int times = param->Times;
			string finalString = "";
			for (auto index = 0; index < times; ++index) {
				finalString += param->TargetString;
			}
			str.insert(param->Start, finalString);
			parseStack.pop();
			if (!parseStack.empty()) {
				i = parseStack.top()->Start;
			}
			else {
				i = param->Start + finalString.size();
			}
		}
	}
        
        return str;
    }
    ```
