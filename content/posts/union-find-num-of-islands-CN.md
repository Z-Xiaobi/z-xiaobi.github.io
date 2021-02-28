---
title: "Union Find Num of Islands CN"
date: 2021-02-28T09:02:34+08:00
description: ""
tags: ["union find", "leetcode"]
series: ["data structure"]
categories: ["Algorithms"]
draft: true
---

# LeetCode 200. 并查集典型问题 2：岛屿数量

回顾并查集的基本描述：

> Given a set of N objects.
・***Union command***: connect two objects.
・***Find/connected query***: is there a path connecting the two objects?

**并查集** 的经典应用是解决 **连通性** 问题。比如 普林斯顿算法课I 上举的例子:
・Pixels in a digital photo.
・Computers in a network.
・Friends in a social network.
・Transistors in a computer chip.
・Elements in a mathematical set.
・Variable names in Fortran program.
・Metallic sites in a composite system.
或者是对应 Coursework 1 的 **Percolation** 问题
又或者是力扣第 200 题：岛屿数量

给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。
```
示例 1:


输入:
[
['1','1','1','1','0'],
['1','1','0','1','0'],
['1','1','0','0','0'],
['0','0','0','0','0']
]
输出: 1
```
```
示例 2:


输入:
[
['1','1','0','0','0'],
['1','1','0','0','0'],
['0','0','1','0','0'],
['0','0','0','1','1']
]
输出: 3

解释: 每座岛屿只能由水平和/或竖直方向上相邻的陆地连接而成。
```

题意分析：这道题给我们一个矩阵，表示一个二维平面。问我们，被海水包围的陆地一共有多少个。


思路分析：注意到所有的字符 '1' 表示的陆地在这个问题中是连成一片的，而 字符 '0' 表示的水域是连成一片的，这条信息提示我们有「连通性」这样的性质。因此很自然想到使用「并查集」解决这个问题。

设计算法流程：遍历二维矩阵，除了遍历到的单元格要读取出它是「陆地」还是「海水」，还需要看看它周围的区域是否与自己是同类的。

如果当前是「陆地」，尝试与周围的陆地合并一下；
如果当前是「水域」，则需要把所有的「水域」合并在一起。这是因为题目的意思是：所有的 水域为一个整体。因此需要设置了一个虚拟的结点，表示 所有的水域都和这个虚拟结点是连接的 。

作者：liweiwei1419
链接：https://leetcode-cn.com/leetbook/read/learning-algorithms-with-leetcode/57svnd/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

2 Princeton Coursework 1 - Percolation   https://coursera.cs.princeton.edu/algs4/assignments/percolation/specification.php
