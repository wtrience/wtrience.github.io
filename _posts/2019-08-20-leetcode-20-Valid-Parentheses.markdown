---
layout:     post
title:      "leetcode 20 valid parentheses"
subtitle:   " \"coding everyday\""
date:       2019-08-19 12:00:00
author:     "Tian"
header-img: "img/post-bg-2015.jpg"
tags:
    - Leetcode
---
> “Thinking”

#Valid Parentheses
##题目：Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Note that an empty string is also considered valid.


Example:
```
Input: "()[]{}"
Output: true
```
##解析
##代码（C++）
```
class Solution {
public:
bool isValid(string s) {
stack<char> paren;
for(char& c:s){
switch(c){
case'(':
case'{':
case'[':paren.push(c);break;
case')':if(paren.empty()||paren.top()!='(')
return false;else paren.pop();break;
case'}':if(paren.empty()||paren.top()!='{')
return false;else paren.pop();break;
case']':if(paren.empty()||paren.top()!='[')
return false;else paren.pop();break;
default:;
}
}
return paren.empty();
}
};
```
