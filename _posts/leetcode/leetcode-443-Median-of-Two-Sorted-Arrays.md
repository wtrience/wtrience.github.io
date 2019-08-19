# 443. String Compression


##题目：
Given an array of characters, compress it [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm).

The length after compression must always be smaller than or equal to the original array.

Every element of the array should be a **character** (not int) of length 1.

After you are done **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)**, return the new length of the array.

**Follow up:**
Could you solve it using only O(1) extra space?

Example1:
```
Input:
["a","a","b","b","c","c","c"]

Output:
Return 6, and the first 6 characters of the input array should be: ["a","2","b","2","c","3"]

Explanation:
"aa" is replaced by "a2". "bb" is replaced by "b2". "ccc" is replaced by "c3".
```
Example2:
```
Input:
["a"]

Output:
Return 1, and the first 1 characters of the input array should be: ["a"]

Explanation:
Nothing is replaced.
```
Example3:
```
Input:
["a","b","b","b","b","b","b","b","b","b","b","b","b"]

Output:
Return 4, and the first 4 characters of the input array should be: ["a","b","1","2"].

Explanation:
Since the character "a" does not repeat, it is not compressed. "bbbbbbbbbbbb" is replaced by "b12".
Notice each digit has it's own entry in the array.
```
##解法一
###解析
            还是用一个新的数组，先把答案存起来，最后交换chars。
###代码（C++）
```
class Solution {
public:
    int compress(vector<char>& chars) {
        if(chars.size() == 0) return 0;
        vector<char> v;
        v.push_back(chars[0]);
        int sum = 1;
        for(int i = 1;i < chars.size(); i ++){
            if(chars[i-1] == chars[i]&&i != chars.size()-1) sum++;
            else {
                if(chars[i-1] == chars[i]&&i == chars.size()-1) {
                    f(v, sum+1);
                    break;
                }
                if(sum != 1) f(v, sum);
                v.push_back(chars[i]);
                sum = 1;
            }
        }
        swap(v, chars);
        return chars.size();
    }
    void f(vector<char>& v, int n){
        string s = to_string(n);
        for(int i = 0; i < s.length(); i ++)
            v.push_back(s[i]);
    }
};
```
##解法二
###解析
实现O(1)的空间复杂度，需要建一个索引cur，直接修改原串，记录压缩后的字符串。遍历字符串，逐个找字符连续出现的范围。先将该字符拷贝到原串的cur上，如果该字符出现一次，不再操作，如果多次，则将该字符的次数追加到cur之后，修改cur。

例如：["a","b","b","c","c","c"]

开始cur = 0，“a”出现一次，无需压缩。chars[cur++] = chars[0]即可。接着"b"出现2次，则有chars[cur++] = chars[1], chars[cur++] = chars[2]。原串中chars[2]变为"2"，cur = 3，类似地，有chars[cur++] = chars[3], chars[cur++] = chars[4]。chars[4]变为"3"，此时cur = 5，即为压缩后的字符串长度。原串压缩为["a","b","2","c","3"]。

这种压缩方式，字符连续出现频率高，才能有较好的压缩效果。

###代码（C++）
```
class Solution {
public:
    int compress(vector<char>& chars) {
        int n = chars.size();
        int cur = 0; // 记录当前字符的索引，最后为压缩字符串的长度
        for(int i = 0; i < n; ) {
            int j = i;
            while( j < n - 1 && chars[j] == chars[j+1]) {// 查找字符连续相同的个数
                j++;
            }
            chars[cur++] = chars[i];// 将当前字符写入原字符串中
            if(i != j) {
                string times = to_string(j - i + 1);// 字符连续相同的个数
                int tLen = times.length();
                for(int k = 0; k < tLen; k++) {//把字符连续相同个数写入字符串，用来压缩
                    chars[cur++] = times[k];
                }
            }
            i = j + 1;//接着处理下一个字符
        }
        return cur;
    }
};  
```

###代码（python）
```
class Solution(object):
    def compress(self, chars):
        """
        :type chars: List[str]
        :rtype: int
        """
        n = len(chars) 
        cur = 0 # 当前字符的索引，用以压缩原字符串
        i = 0
        while i < n:
            j = i
            while j < n - 1 and chars[j] == chars[j+1]:# 找字符连续出现的次数
                j += 1
            chars[cur] = chars[i] # 记录当前处理的字符
            cur += 1
            if i != j:
                times = str(j-i+1) # 将字符的次数写入原串中
                tLen = len(times)
                for k in range(tLen):
                    chars[cur+k] = times[k]
                cur += tLen
            i = j + 1 # 处理下一个字符
        return cur
```

