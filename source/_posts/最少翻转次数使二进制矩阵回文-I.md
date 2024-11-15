---
title: 【每日算法题】最少翻转次数使二进制矩阵回文 I
date: 2024-11-15 10:13:27
updated: 2024-11-15 10:13:27
tags:
  - 双指针
description: 每天一道算法题，让生活变得更好
categories:
  - 学习
  - 代码
  - 算法
---

### 最少翻转次数使二进制矩阵回文 I

给你一个 m x n 的二进制矩阵 grid 。

如果矩阵中一行或者一列从前往后与从后往前读是一样的，那么我们称这一行或者这一列是 回文 的。

你可以将 grid 中任意格子的值 翻转 ，也就是将格子里的值从 0 变成 1 ，或者从 1 变成 0 。

请你返回 最少 翻转次数，使得矩阵 要么 所有行是 回文的 ，要么所有列是 回文的 。

### 解答

```python
class Solution:
    def minFlips(self, grid: List[List[int]]) -> int:
        row_ans = 0
        for row in range(len(grid)):
            left = 0
            right = len(grid[0])-1
            while left < right:
                if grid[row][right] != grid[row][left]:
                    row_ans += 1
                left += 1
                right -= 1

        col_ans = 0
        for col in range(len(grid[0])):
            top = 0
            button = len(grid) - 1
            while top < button:
                if grid[top][col] != grid[button][col]:
                    col_ans += 1
                top += 1
                button -= 1

        return min(row_ans, col_ans)
```

### 思路

据题知，只要 行 或 列 其中一个满足回文既可以，使用双指针，行首和行末，逐行判断是否需要翻转；使用双指针，列头和列尾，逐列判断是否需要翻转。

### 复杂度

n 是 行数，m 是 列数

时间复杂度：O(max(n, m)) 

空间复杂度：O(1)
