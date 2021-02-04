---
layout: post
title: Day3---Linked List Cycle
description: C++ solution
image: assets/images/feb-challenge.png
---

# Day3---Linked List Cycle(easy)

## 题目

Given **head**, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the **next** pointer. Internally, **pos** is used to denote the index of the node that tail's **next** pointer is connected to. <u>Note that <strong>pos</strong> is not passed as a parameter</u>.

Return **true** *if there is a cycle in the linked list*. Otherwise, return **false**.

**Constraints:**

- The number of the nodes in the list is in the range [0, 10e4].
- -10e5 <= Node.val <= 10e5
- **pos** is -1 or a valid index in the linked-list.

**Follow up:** Can you solve it using **O(1)** (i.e. constant) memory?

题意概括：

* 给定参数只有链表的head(struct ListNode)，任务是判断该链表中有没有环路。
* 需要明确的是pos并不是参数，而是内部的指向tail->next结点的指针。如果pos的值为-1，那么该链表就无环，否则一定是有环的。

## 分析

<center><img src="..\assets\images\day3.png" alt="image-20210204105007931" style="zoom: 80%;" /></center>

这道题的关键在于，我们用一根指针往后遍历，当指针指回之前的结点的时候，应该如何告诉程序当前这个结点我们曾经访问过，也就是要区分访问过的结点和没有访问过的结点。我能想到的区分方法只有**标记**：

首先，可以设置一个类似于visited数组的结构。但是难点在于如何设计这个数组的索引，当我们的指针走到一个结点的时候，我们用这个结点的什么值来索引这个数组呢？如果用结点的value，题目没有保证不同结点的value不会重复，而且value的值也可能很大而且很稀疏......想来想去，发现很难找到和结点形成一一映射的特征。

在链表之外标记行不通的话，能不能干脆直接在链表上进行标记呢？我们能修改的只有结点的value，因为题目给定value的值在[-10e5, 10e5]之内，如果每次访问当前结点的时候把它的value设置在这个区间之外，其实就可以达到标记的效果了。所以我想的一个方案是每走到一个结点，就把它的value减掉2*10e5+1，保证访问过结点的值都小于这个区间的下界。这样一来，这道题就解决了。

## 代码

初版C++代码如下：

```c++
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
    bool hasCycle(ListNode *head) {
        ListNode *seek = head;
        while(seek != NULL){
            if (seek->val < -100000){
                return true;
            }
            seek->val = seek->val - (2 * 100000 + 1);
            seek = seek->next;
        }
        return false;
    }
};
```

最终运行结果是16ms，7.4MB，运行时间很长，但内存用量小于94.32%的选手，同时也完成了题目要求的用O(1)内存解题的挑战。

<center><img src="..\assets\images\day-3-res.png" alt="image-20210204105007931" style="zoom: 80%;" /></center>

## 优化

尚无。

