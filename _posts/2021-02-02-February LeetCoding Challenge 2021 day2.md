---
layout: post
title: Day2---Trim a BST
description: C++ solution
image: assets/images/feb-challenge.png
---

# Day2---Trim a Binary Search Tree(medium)

## 题目

Given the **root** of a binary search tree and the lowest and highest boundaries as **low** and **high**, trim the tree so that all its elements lies in [low, high]. Trimming the tree should <u>not</u> change the relative structure of the elements that will remain in the tree (i.e., any node's descendant should remain a descendant). It can be proven that there is a <u>unique answer</u>.

Return *the root of the trimmed binary search tree*. Note that the root may change depending on the given bounds.

题意概括：

* 给定参数有root(struct TreeNode), low(int), high(int)，low和high是BST结点值的边界。
* 我们要做的是通过对BST剪枝，使得其中所有结点的值都在[low, high]区间内。
* 要保留原BST结点间的逻辑关系，父子兄弟结点的关系不能改变。

## 分析

<center><img src="..\assets\images\day2.png" alt="image-20210203095236504" style="zoom:80%;" /></center>

首先，树相关的问题很自然想到用递归来做。因为要调整一整颗树，所以用递归的思路处理这个问题是合适的。其次，需要明确BST的性质，即满足left child < root < right child。再次，需要清楚编程相关的一些要求，题目所给的二叉树是通过结构体指针来实现的，root结点唯一代表一棵树。

递归求解，我们只需要考虑二叉树的母结构，也就是左子树-根-右子树。如果根的value小于low，那就说明整个左子树所有结点的值都小于low，所以只需要保留右子树，而右子树已经是一个完整的子结构了，所以我们直接对右子树进行递归调用，返回新右子树的root。同理，如果根的value大于high，则递归调用左子树。第三种情况，如果要保留根，那么我们最后要返回的也就是这个根，那么我们对这棵树的调整其实就体现在根的left child和right child两根指针上。所以分别对左右子树进行递归调用，并赋给根的两根指针即可。

## 代码

初版C++代码如下：

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if (root == NULL){
            return NULL;
        }
        if (root->val < low){
            return trimBST(root->right, low, high); //右子树修剪后的root
        }
        else if (root->val > high){
            return trimBST(root->left, low, high); //左子树修剪后的root
        }
        else{ //保留原root
            root->left = trimBST(root->left, low, high);
            root->right = trimBST(root->right, low, high);
            return root;
        }
    }
};
```

## 优化

尚无。