# 4. Median of Two Sorted Arrays 

##题目：
There are two sorted arrays nums1 and nums2 of size m and n respectively.
Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).
Example1:
```
nums1 = [1, 3]
nums2 = [2]
The median is 2.0
```
Example2:
```
nums1 = [1, 2]
nums2 = [3, 4]
The median is (2 + 3)/2 = 2.5
```
##解法一
###解析
用一个新数组存放nums1和nums2合并后的数组。有序数组的合并最后取出中位数

还有种思路，就是遍历两个数组，在里面找到第i个大元素，但是时间复杂度为O（m+n）
###代码（C++）
```
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        vector<int> numsAll;

        for (int i = 0, j = 0; i < nums1.size() || j < nums2.size();) {
            if (i < nums1.size()) {
                if (j < nums2.size()) {
                    if (nums1[i] < nums2[j]) {
                        numsAll.push_back(nums1[i++]);
                    } else {
                        numsAll.push_back(nums2[j++]);
                    }
                } else {
                    numsAll.push_back(nums1[i++]);
                }
            } else {
                numsAll.push_back(nums2[j++]);
            }
        }

        int middle = (nums1.size() + nums2.size()) >> 1;
        if ((nums1.size() + nums2.size()) % 2 == 1) {
            return (double)numsAll[middle];
        } else {
            return ((double)numsAll[middle] + (double)numsAll[middle-1]) / 2.0;
        }

    }
};

```
