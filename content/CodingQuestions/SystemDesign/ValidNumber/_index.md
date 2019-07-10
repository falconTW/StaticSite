---
title: "ValidNumber"
date: 2019-05-23T23:12:19+08:00
draft: true
---

Validate if a given string can be interpreted as a decimal number.

```
Some examples:
"0" => true
" 0.1 " => true
"abc" => false
"1 a" => false
"2e10" => true
" -90e3   " => true
" 1e" => false
"e3" => false
" 6e-1" => true
" 99e2.5 " => false
"53.5e93" => true
" --6 " => false
"-+3" => false
"95a54e53" => false
```

Note: It is intended for the problem statement to be ambiguous. You should gather all requirements up front before implementing one. However, here is a list of characters that can be in a valid decimal number:

Numbers 0-9
Exponent - "e"
Positive/negative sign - "+"/"-"
Decimal point - "."
Of course, the context of these characters also matters in the input.

Update (2015-02-10):
The signature of the C++ function had been updated. If you still see your function signature accepts a const char * argument, please click the reload button to reset your code definition.


## Solution 1 

Draw a DFA diagram and mark each vaild state. Then make a transition table for the operation.

[DFA](https://tinyurl.com/yxaryx8r)

```c++
class Solution {
public:
	bool isNumber(string  s) {
		enum InputType {
			INVALID, SPACE, SIGN, DOT, E, DIGIT, LEN
		};
		int trans[][LEN] = {
		{ -1,  0,  1,  2, -1,  3 },
		{ -1, -1, -1,  2, -1,  3 },
		{ -1, -1, -1, -1, -1,  4 },
		{ -1,  5, -1,  4,  6,  3 },
		{ -1,  5, -1, -1,  6,  4 },
		{ -1,  5, -1, -1, -1, -1 },
		{ -1, -1,  7, -1, -1,  8 },
		{ -1, -1, -1, -1, -1,  8 },
		{ -1,  5, -1, -1, -1,  8 }
		};
	int state = 0;
	for (int i = 0; i < s.size(); ++i) {
		InputType input;
		if (isspace(s[i])) {
			input = SPACE;
		}
		else if (s[i] == '+' || s[i] == '-') {
			input = SIGN;
		}
		else if (s[i] == '.') {
			input = DOT;
		}
		else if (s[i] == 'e' || s[i] == 'E') {
			input = E;
		}
		else if (isdigit(s[i])) {
			input = DIGIT;
		}
		else {
			input = INVALID;
		}
		state = trans[state][input];
		if (state == -1) {
			return false;
		}
	}
	
		return state == 3 || state == 4 || state == 5 || state == 8;
	}
};
```