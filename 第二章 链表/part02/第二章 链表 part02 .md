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

一开始没有想起把链表反过来，所以用了冗长的代码让长链表的cur往前走，用了教程的方法后简洁了很多。记笔记

```cpp
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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* curA=headA;
        ListNode* curB=headB;
        int lenA=0, lenB=0;
        while(curA!=NULL){
            curA=curA->next;
            lenA++;
        }
        while(curB!=NULL){
            curB=curB->next;
            lenB++;
        }
        curA=headA;
        curB=headB;
        /////////////////
        int N=lenA-lenB;
        if(N>=0){
            while(N--){
                curA=curA->next;
            }
        }else{
            N=-N;
            while(N--){
                curB=curB->next;
            }
        }
        /////////////////
        //  if (lenB > lenA) {
        //     swap (lenA, lenB);
        //     swap (curA, curB);
        // }
        // // 求长度差
        // int gap = lenA - lenB;
        // // 让curA和curB在同一起点上（末尾位置对齐）
        // while (gap--) {
        //     curA = curA->next;
        // }
        ///////////////////

        while(curA!=NULL&&curB!=NULL){
            if(curA==curB){
                return curA;
            }
            curA=curA->next;
            curB=curB->next;
        }
        return NULL;
    }
};
```

## 环形链表

![image.png](%E7%AC%AC%E4%BA%8C%E7%AB%A0%20%E9%93%BE%E8%A1%A8%20part02%20d726538066a946daab5037d1fe3c4722/image%201.png)

![image.png](%E7%AC%AC%E4%BA%8C%E7%AB%A0%20%E9%93%BE%E8%A1%A8%20part02%20d726538066a946daab5037d1fe3c4722/image%202.png)

```cpp
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
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;

        while(fast!=NULL&&fast->next!=NULL){
            slow=slow->next;
            fast=fast->next->next;

            if(slow==fast){
                ListNode* cur1 = fast;
                ListNode* cur2 = head;

                while(cur1!=cur2){
                    cur1 = cur1 -> next;
                    cur2 = cur2 -> next;
                }
                return cur1;   
            }
        }
        return NULL;
    }
};
```

用两天时间很快的就把链表的知识过了一遍，效率很高！总结：

[https://www.programmercarl.com/链表总结篇.html#链表经典题目](https://www.programmercarl.com/链表总结篇.html#链表经典题目)