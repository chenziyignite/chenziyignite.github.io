---
layout: post
title: Day9---Convert BST to Greater Tree
description: Java solution
image: assets/images/feb-challenge.png
---

# Day9---Convert BST to Greater Tree(medium)

## 题目

Given the **root** of a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.

As a reminder, a *binary search tree* is a tree that satisfies these constraints:

- The left subtree of a node contains only nodes with keys <u>less than</u> the node's key.
- The right subtree of a node contains only nodes with keys <u>greater than</u> the node's key.
- Both the left and right subtrees must also be binary search trees.

题意概括：

* 给定参数是一颗二叉搜索树（BST）的root(class TreeNode)，需要我们把它转化成Greater Tree。
* Greater Tree定义为在BST的基础上，每个结点的值(val)都要再加上所有值大于该结点的结点值之和。

## 分析

<center><img src="..\assets\images\day9.png" style="zoom:67%;" /></center>

<center><i>从此以后，只对比较有价值的题进行更新</i></center>

与前几天出现过的二叉树的题相比，这道题对于递归设计的要求更高一些，个人认为需要强化的概念有：

* 递归设计的时候，**<u>永远不要去想子结构</u>**。只要把最上层结构的问题解决了，任何子结构的问题也就解决了。
* 设计递归函数的时候，可以把在函数体内的递归调用想象具有任何你想让它有的功能，都是成立的。前提是你要在最上层merge的时候实现同样的功能才可以。
* 一定要具备**顺序**意识：应该先递归调用左子树还是右子树？在什么位置merge？
* 做题相关：submit之后有两次以上比较复杂的test case出错了，基本上就要重新设计递归函数了，小修小补一般就是拆了东墙补西墙。

回头来看这道题，有两种看待它的方式：

1. 如果你一开始就对二叉树理解得比较透彻，你会发现它就是一个后序遍历的问题。后序遍历的结点顺序是递增顺序，所以把之前遍历过的结点求和就好了，这个“和”需要是一个全局范围的变量。后序遍历的处理顺序是：右子树-根-左子树。
2. 如果理解不到这么透彻，也可能纯从递归思想入手。最上层结构中只需要考虑根结点，显然大于根结点的结点都在右子树上，并且根+右子树大于所有左子树的结点，所以顺序是：先递归调用右子树，再调整根，再递归调用左子树。我们知道这个递归函数只能给上一层返回一个root结点，所以为了留住这个和必须设置全局变量sum。右子树递归调用结束后，我们认为它可以调整好所有结点的值，并把右子树结点求和存在sum中，这时我们调整根结点和sum的值（实现我们所期望的功能），再对左子树递归调用即可。

## 代码

Java Solution(0ms, 39MB)：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int sum = 0;
    public TreeNode convertBST(TreeNode root) {
        if (root == null){
            return null;
        }
        root.right = convertBST(root.right);
        sum += root.val;
        root.val = sum;
        root.left = convertBST(root.left);
        return root;
    }
}
```

## 优化

尚无。

