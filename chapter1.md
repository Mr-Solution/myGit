

## 2. Add Two Numbers

**Tags: 指针，链表**

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Example：
>Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)    
>Output: 7 -> 0 -> 8    
>Explanation: 342 + 465 = 807.

两个数字逆序存放于链表中，实现两个数相加，并将其和也逆序存放于链表中。除了数字0之外，其他数字没有前导零(即999不会被表示为0999)。

``` C++
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
        ListNode *sumHead = new ListNode(0);
        ListNode *p = l1, *q = l2, *curr = sumHead;
        int carry = 0;
        while (p != NULL || q != NULL) {
            int x = (p != NULL) ? p->val : 0;
            int y = (q != NULL) ? q->val : 0;
            int sum = carry + x + y;
            carry = sum / 10;
            curr->next = new ListNode(sum % 10);
            curr = curr->next;
            if (p != NULL) 
                p = p->next;
            if (q != NULL) 
                q = q->next;
        }
        if (carry > 0) {
            curr->next = new ListNode(carry);
        }
        return sumHead->next;
    }
};
```


## 7.Reverse Integer

**Tags: 数值表示范围，溢出**

Given a 32-bit signed integer, reverse digits of an integer.
Example：
> Input: 123    
> Output: 321
> 
> Input: -123    
> Output: -321
> 
> Input: 120    
> Output: 21

Note:
Assume we are dealing with an environment which could only hold integers within the 32-bit signed integer range. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

翻转问题需要注意的是溢出问题，有符号整型的最高位是符号位，假设一个32位的有符号数，它表示的范围是-2147483648 ~ 2147483647，2147483647翻转之后显然是会溢出的，在 limits.h 中，有 INT_MAX 的宏定义，``` #define INT_MAX 214783647 ```，但是如果直接比较 res 同 INT_MAX 的大小显然是行不通的，再加上算法中采用的是 res*10 来使 res 左移，所以，临界判断采用 ``` abs(res) > INT_MAX / 10 ```，采用 abs 可以排除正负号影响。

``` C++
class Solution {
public:
    int reverse(int x) {
        int res = 0;
        while (x != 0) {
            if (abs(res) > INT_MAX / 10) return 0;
            res = res * 10 + x % 10;
            x /= 10;
        }
        return res;
    }
};
```

## 9. Palindrome Number

**回文、取整取余**  
Determine whether an integer is a palindrome. Do this without extra space.  
``` C++
class Solution {
public:
    bool isPalindrome(int x) {
        if(x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }     
        int revertedNumber = 0;
        while(x > revertedNumber) {
            revertedNumber = revertedNumber * 10 + x % 10;
            x /= 10;
        }
        return x == revertedNumber || x == revertedNumber/10;
    }
};
```
答案的做法通过判断 x > revertedNumber， 将整个过程折半，不仅使时间消耗减半，同时由于 x 并没有减为 0 ，故不需要中间变量存储 x 的初始值，减少了不必要的空间消耗。  
_Note_: C99 开始支持布尔型，声明方式为 _Bool