#3. Longest Substring Without Repeating Characters
##题目：
Given a string, find the length of the longest substring without repeating characters.
Example:
```
Given "abcabcbb", the answer is "abc", which the length is 3.
Given "bbbbb", the answer is "b", with the length of 1.
Given "pwwkew", the answer is "wke", with the length of 3. 
Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```
##解法一
###解析
用一个map记录各个字符的下标，首先将所有的下表初始化为-1，然后向后遍历的过程中记录当前字符的下标，并将当前字符的下标与上一个相同字符的下标做差值，即可得到它们之间无重复字符的子串。然后用Max记录更新最大子串数，并更新map存储的下标值，即从这个重复值重新开始数字符串的数（最大无重复字符子串必定在两个重复字符之间，或者是整个字符串的长度）
这种方法好像被叫做滑动窗口法
"滑动窗口" 
    比方说 abcabccc 当你右边扫描到abca的时候你得把第一个a删掉得到bca，
    然后"窗口"继续向右滑动，每当加到一个新char的时候，左边检查有无重复的char，
    然后如果没有重复的就正常添加，
    有重复的话就左边扔掉一部分（从最左到重复char这段扔掉），在这个过程中记录最大窗口长度
###代码（C++）
```
class Solution {  
public:  
    int lengthOfLongestSubstring(string s) {  
        map<char,int> book;  
        int i,Max=0,pre=-1;  
        for(i=0;i<s.length();i++)
        book[s[i]]=-1;  
        for(i=0;i<s.length();i++)  
        {  
            pre=max(pre,book[s[i]]);//更新map中各个字符的下标  
            Max=max(Max,i-pre); //保存暂时的最大无重复子串长度  
            book[s[i]]=i; //计算差值后继续更新  
        }  
        return Max;  
    }  
}; 

```
##解法二
###解析
建立一个256位大小的整型数组来代替哈希表，这样做的原因是ASCII表共能表示256个字符，所以可以记录所有字符，然后我们需要定义两个变量res和left，其中res用来记录最长无重复子串的长度，left指向该无重复子串左边的起始位置，然后我们遍历整个字符串，对于每一个遍历到的字符，如果哈希表中该字符串对应的值为0，说明没有遇到过该字符，则此时计算最长无重复子串，i - left +１，其中ｉ是最长无重复子串最右边的位置，left是最左边的位置，还有一种情况也需要计算最长无重复子串，就是当哈希表中的值小于left，这是由于此时出现过重复的字符，left的位置更新了，如果又遇到了新的字符，就要重新计算最长无重复子串。最后每次都要在哈希表中将当前字符对应的值赋值为i+1。
###代码（C++）
```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int m[256] = {0}, res = 0, left = 0;
        for (int i = 0; i < s.size(); ++i) {
            if (m[s[i]] == 0 || m[s[i]] < left) {
                res = max(res, i - left + 1);
            } else {
                left = m[s[i]];
            }
            m[s[i]] = i + 1;
        }
        return res;
    }
};
```
