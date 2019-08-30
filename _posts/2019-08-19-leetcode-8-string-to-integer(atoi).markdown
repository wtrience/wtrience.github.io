---
layout:     post
title:      "leetcode 8 string to integer"
subtitle:   " \"coding everyday\""
date:       2019-08-19 12:00:00
author:     "Tian"
header-img: "img/post-bg-2015.jpg"
tags:
    - Leetcode
---

> “Yeah It's on. ”

# 8. String to Integer (atoi)

## 题目：

Implement atoi which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.
Example:
Example 1:
```
Input: "42"
Output: 42
```
Example 2:
```
Input: "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign.
Then take as many numerical digits as possible, which gets 42.
```
Example 3:

```
Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.
```
Example 4:
```
Input: "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical 
digit or a +/- sign. Therefore no valid conversion could be performed.
```

Example 5:
```
Input: "-91283472332"
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.
Thefore INT_MIN (−231) is returned.
```
## 解析
字符串转为整数是很常用的一个函数，由于输入的是字符串，所以需要考虑的情况有很多种。这题只需要考虑数字和符号的情况：

1. 若字符串开头是空格，则跳过所有空格，到第一个非空格字符，如果没有，则返回0.
2. 若第一个非空格字符是符号 +/-，则标记 sign 的真假，这道题还有个局限性，那就是在 c++ 里面，+-1 和-+1 都是认可的，都是 -1，而在此题里，则会返回0.
3. 若下一个字符不是数字，则返回0. 完全不考虑小数点和自然数的情况，不过这样也好，起码省事了不少。
4. 如果下一个字符是数字，则转为整形存下来，若接下来再有非数字出现，则返回目前的结果。
5. 还需要考虑边界问题，如果超过了整型数的范围，则用边界值替代当前值。
## 代码（C++）
```
class Solution {
public:
int myAtoi(string str) {
if(str.empty()) 
return 0;
int sign =0,base=0,i=0,n=str.size();
while(i<n&&str[i]==' ')
++i;
if(i<n&&(str[i]=='+'||str[i]=='-')){
sign=(str[i++]=='+')?1:-1;
}
while(i<n&&str[i]>='0'&str[i]<='9'){
if(base>INT_MAX/10||(base==INT_MAX/10&&str[i]-0>7)){
return (sign==1)?INT_MAX:INT_MIN;
}
base =base*10+(str[i++]-'0');
}
return base*sign;
}
};
```
