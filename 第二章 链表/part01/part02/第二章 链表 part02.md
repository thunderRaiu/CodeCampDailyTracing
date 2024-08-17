# 第二章 链表 part02

Date: August 17, 2024
Tags: 知识总结, 链表

## 链表位置互换

相当于从头到尾打通链表

通过画图的方法研究如何进行链表操作，头脑要清醒

![image.png](%E7%AC%AC%E4%BA%8C%E7%AB%A0%20%E9%93%BE%E8%A1%A8%20part02%20d726538066a946daab5037d1fe3c4722/image.png)

```cpp
/*
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:

    ListNode* swapPairs(ListNode* head) {
        ListNode* dummyHead= new ListNode(0);
        dummyHead->next = head;
        ListNode* cur = dummyHead;
        ListNode* tmp1;
        ListNode* tmp2;
        while(cur->next!=NULL&&cur->next->next!=NULL){
            tmp1=cur->next;
            tmp2=cur->next->next->next;
            cur->next=tmp1->next;
            cur->next->next=tmp1;
            cur->next->next->next=tmp2;

            cur=cur->next->next;
        }
        return dummyHead->next;
    }
};
```

## 删除倒数第n个元素

（这道题比较简单，但是也是要注意控制边界条件，第一次写的时候忘记了快指针前进的时候要卡结尾了，属于是失误）

而且也很好想快指针要比慢指针多走一个

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* cur = dummyHead;
        ListNode* cur1 = cur;
        while(n--&&cur1!=NULL){
            cur1=cur1->next;
        }
        while(cur1->next!=NULL){
            cur=cur->next;
            cur1=cur1->next;
        }
        ListNode* tmp = cur->next;
        cur->next=cur->next->next;
        delete tmp;
        return dummyHead->next;
    }
};
```

## 交叉链表

## 环形链表

上面两个思路都很好想，但是没有写代码，回头补齐！

用两天时间很快的就把链表的知识过了一遍，效率很高！总结：

[https://www.programmercarl.com/链表总结篇.html#链表经典题目](https://www.programmercarl.com/链表总结篇.html#链表经典题目)