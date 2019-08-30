---
layout:     post
title:      "leetcode 1 two sum"
subtitle:   " \"Hello World, Hello Blog\""
date:       2019-02-29 12:00:00
author:     "tian"
header-img: "img/post-bg-2015.jpg"
tags:
    - Leetcode
---

[leetcode1sum](https://wtrience.github.io/2019/01/29/leetcode-1-Two-Sum/)
# 1.Two Sum
## 题目：
Given an array of integers, return indices of the two numbers such that they add up to a specific target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
Example:
```
Given nums = [2, 7, 11, 15], target = 9,
Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```
## 解析
这题表面上看需要遍历数组中两两之和，并与目标进行比较，需要O(n²)的时间复杂度。但我们只需要利用哈希表就可以将时间复杂度降到O(n)。 
首先定义一个哈希表（即c++的map），把值作为key，把下标作为value，遍历数组，根据目标值减去数组值算出要找的值，访问map，若存在该值则将答案记录下来，若没有找到，则将该值-下标对加入到map中。
## 代码（C++）
```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> result;
        map<int, int> hash;
        for (int i = 0; i < nums.size(); i++) {
            int NumberToFind = target - nums[i];
            //计算要找的值
            if (hash.find(NumberToFind) != hash.end()) {
            //从表中找到了要找的值
                result.push_back(hash[NumberToFind]);
                result.push_back(i);
            }
            //将值-下标对添加到map中
                hash[nums[i]] = i;
            }
        }
        return result;
    }
};
```
