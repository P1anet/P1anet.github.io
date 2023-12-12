---
layout: post
title: "MOOC浙大《数据结构》/ PTA-C语言 / 11-散列4 Hashing - Hard Version (30分)"
subtitle: "MOOC"
date: 2020-05-15
author: "Kenshin"
header-img: "img/post-bg/post-bg-default.png"
tags:
  - Others
---

# 题目描述

Given a hash table of size **_N_**, we can define a hash function **_H(x)=x%N_**. Suppose that the linear probing is used to solve collisions, we can easily obtain the status of the hash table with a given sequence of input numbers.

However, now you are asked to solve the reversed problem: reconstruct the input sequence from the given status of the hash table. Whenever there are multiple choices, the smallest number is always taken.

### Input Specification:

Each input file contains one test case. For each test case, the first line contains a positive integer **_N_** (≤1000), which is the size of the hash table. The next line contains N integers, separated by a space. A negative integer represents an empty cell in the hash table. It is guaranteed that all the non-negative integers are distinct in the table.

### Output Specification:

For each test case, print a line that contains the input sequence, with the numbers separated by a space. Notice that there must be no extra space at the end of each line.

### Sample Input:

> **11
> 33 1 13 12 34 38 27 22 32 -1 21**

### Sample Output:

> **1 13 12 21 33 34 38 27 22 32**

# 思路

看了陈越老师的思路是用拓扑排序做，奈何刚开始学，还没复习过什么都不熟悉，想着用自己的方法做一做，提交 ac 了而且貌似效率还不错的样子，所以试着写下自己的第一篇 Blog。

思路是 degree 保存其当前地址减去哈希地址为排在其前面输入哈希表的数的数目，并复制存入 cmpdegree 以便比较，当其前驱值确定并输出（先放入 a[n]数组）后，其 degree 减 1。当 degree 为-1 时表示此值已经确定了在输入序列的位置不用再进行比较。当最后 degree 都为-1 即表示所有哈希表中的值都已确定的输入次序，此时顺序输出 a[k]即可，其中为-1 的值代表原本哈希表中的空位。

# 代码

```c
#include <stdio.h>
int main() {
    int n;
    scanf("%d", &n);
    int i, j, num, min, k = 0, cmp;
    int data[n], degree[n], cmpdegree[n], a[n];
    for (i = 0; i < n; i++) {
    //读入输入数据并计算优先级
        scanf("%d", &num);
        data[i] = num;
        degree[i] = (i - num % n + n) % n;
        if (num < 0)
            degree[i] = -1;
        cmpdegree[i] = degree[i];
    }
    while (k < n) {
        min = -1;//最终仍为-1则表示表中除了空位都已经被输出了
        for (i = 0; i < n; i++)
            if (degree[i] == 0)//0表示可以输出了，同时过滤了不能被输出的和已经输出过的数据
                if (min == -1 || data[i] < min)//寻找其中的最小值
                    min = data[i];
        for (i = 0; i < n; i++) //寻找此最小值所在的位置，其实可以在上个循环中新声明一个变量记录i
            if (data[i] == min)
                break;
        for (j = 0; j < n; j++) {
            cmp = (j - i + n) % n;
            if (degree[j] >= 0 && cmp <= cmpdegree[j])//处理优先级
                degree[j]--;
        }
        a[k] = min;//将结果放入数组中最后一起输出
        k++;//k可以同时充当一个标记的作用
    }
    for (k = 0; k < n; k++) {
        if (a[k] == -1)
            break;//读到-1表示后面对应的都是哈希表中的空位
        if (k != 0)
            printf(" ");
        printf("%d", a[k]);
    }
    return 0;
}
```
