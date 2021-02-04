---
layout: post
title: Day1---number of 1 bits
description: C++ solution
image: assets/images/feb-challenge.png
---

# Day1---Number of 1 bits(easy)

## 题目

Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the [Hamming weight](http://en.wikipedia.org/wiki/Hamming_weight)).

**Note:**

- Note that in some languages such as Java, there is no unsigned integer type. In this case, the input will be given as a signed integer type. It should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
- In Java, the compiler represents the signed integers using [2's complement notation](https://en.wikipedia.org/wiki/Two's_complement). Therefore, in **Example 3** above, the input represents the signed integer. `-3`.

**Follow up**: If this function is called many times, how would you optimize it?

题意概括：

* 题目给定一个unsigned int，然后要求这个数（二进制）有几位1。
* 需要注意的是Java没有unsigned类型，而且Java的编译器用补码表示有符号数，在实现的时候需要注意转换。

## 分析

第一种思路是直接把输入的无符号数转化成二进制的码流，遍历计数。

第二种思路是递归，把原问题分解成判断LSB是否为1-bit+子问题的形式，然后递归求解子问题。

第三种思路是就是把递归改成循环。

## 代码

初版C++代码如下：

```c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        if (n == 0 || n == 1){
            return n;
        }
        return ((n % 2) + hammingWeight(n >> 1));
    }
};
```

运行结果4ms，5.9MB。

## 优化

*首先很重要的一点，用右移1位来代替除以2会快很多*。

#### 方案1

现在时间是4ms，猜测时间可能花费在那些非按位的操作上了，所以优化目标应该是尽量使用按位操作。随着n的移位，我们其实关注的只是LSB，所以可以用mask和按位与来获得LSB的信息，这样通过n&mask来代替取模的操作。优化代码如下：

```c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int bits = 0;
        unsigned int mask = 1;
        for (int i = 0; i < 32; i++) {
            if ((n & mask) != 0) {
                bits++;
            }
            mask <<= 1; // mask左移等同于n右移
        }
        return bits;
    }
};
```

运行结果是0ms，5.9MB。

#### 方案2（高端操作）

思路依然是对每一个1-bit进行计数，但是可以不用移位来进行，我们可以用n&(n-1)巧妙地解决这个问题。下面我们来分析一下n&(n-1)这个操作：

首先，对于n最后的一个1-bit（后面都是0-bit），n-1对应位置上一定是0-bit，所以n&(n-1)可以把最后一个1-bit以及后面的所有位清0。假如我们每次都能清掉最后的1-bit，最终我们就可以把n清零，同时还可以对所有的1-bit进行计数。

代码如下：

```c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int sum = 0;
        while (n != 0) {
            sum++;
            n &= (n - 1);
        }
        return sum;
    }
};
```

运行结果是0ms，5.7MB。