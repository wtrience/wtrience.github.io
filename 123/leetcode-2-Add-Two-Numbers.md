#2. Add Two Numbers
##题目：
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.
Example:
```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```
##解析
用LinkList代表整数中每一个数字，模拟加法进位的方式进行求和计算。例如：2 -> 4 -> 3
和5 -> 6 -> 4，先计算个位数 2和5的和为7， 十位数 4和6的和为10，逢十进一，则该位的数字
为0，产生一位进位1到百位参与百位的数字求和。那么百位的结果：3 + 4 + 1（这个1就是刚刚十位
相加产生的进位），百位计算结果为8，所以最终返回结果 7 -> 0 -> 8
##代码（C++）
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* pRoot = NULL;
        
        do {
            if (l1 == NULL) {
                pRoot = l2;
                break;
            } 
            
            if (l2 == NULL) {
                pRoot = l1;
                break;
            }
            
            int sum = l1->val + l2->val;
            int digit = sum % 10;
            int carry = sum / 10;
            pRoot = new ListNode(digit);
            ListNode* pTail = pRoot;
            l1 = l1->next;
            l2 = l2->next;

            while (l1 != NULL || l2 != NULL) {
                int sum = ((l1 != NULL) ? l1->val : 0) + ((l2 != NULL) ? l2->val : 0) + carry;
                int digit = sum % 10;
                carry = sum / 10;
                
                ListNode* pNew = new ListNode(digit);
                pTail->next = pNew;
                pTail = pNew;
                
                l1 = l1 != NULL ? l1->next : NULL;
                l2 = l2 != NULL ? l2->next : NULL;
            }
            
            if (carry == 1) {
                ListNode* pNew = new ListNode(carry);
                pTail->next = pNew;
            }
        } while (false);
        
        return pRoot;
    }
};
```
