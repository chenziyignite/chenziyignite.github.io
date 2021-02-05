---
layout: post
title: Day4---Longest Harmonious Subsequence
description: Java solution
image: assets/images/feb-challenge.png
---

# Day4---Longest Harmonious Subsequence

## 题目

We define a harmonious array as an array where the difference between its maximum value and its minimum value is **exactly** 1.

Given an integer array **nums**, return *the length of its longest harmonious subsequence among all its possible subsequences*.

A **subsequence** of array is a sequence that can be derived from the array by deleting some or no elements without changing the order of the remaining elements.

题意概括：

* 给定参数是数组nums，要求出**最长**的harmonious subsequence。
* Harmonious subsequence是由原序列中的元素（不一定连续，但是先后顺序要相同）构成的子序列，其中元素最大值与最小值的差要等于1。

## 分析

首先对harmonious subsequence的性质进行分析，因为这是个整数序列，所以这个子序列只能包含两个相差为1的整数。<u>如果我们能求解出这一对(n-1, n)，那么我们便求解出了这个子序列</u>。

求解这个问题，最直观的思路就是统计每个数出现的次数，然后找到相邻的两个出现次数总和最大的整数。如果用数组来统计还是会出现稀疏的问题，而采用HashMap是比较合适的。到这里此题就得到解决了。

## 代码

初版Java代码如下：

```java
class Solution {
    public int findLHS(int[] nums) {
        int ret = 0;
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i: nums){ // construct HashMap
            if(map.get(i) == null){
                map.put(i, 1);
            }
            else{
                map.put(i, map.get(i) + 1);
            }
        }
        for (int i: map.keySet()){
            if (map.get(i + 1) != null){
                ret = Math.max(ret, map.get(i) + map.get(i + 1));
            }
        }
        return ret;
    }
}
```

## 优化

尚无。

