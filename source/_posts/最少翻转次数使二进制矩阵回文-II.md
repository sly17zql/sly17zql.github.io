---
title: 【每日算法题】最少翻转次数使二进制矩阵回文 II
tags:
  - 双指针
description: 每天一道算法题，让生活变得更好
categories:
  - 学习
  - 代码
  - 算法
abbrlink: 37132
date: 2024-11-17 08:53:00
updated: 2024-11-17 08:53:00
---

### 最少翻转次数使二进制矩阵回文 II

给你一个 m x n 的二进制矩阵 grid 。

如果矩阵中一行或者一列从前往后与从后往前读是一样的，那么我们称这一行或者这一列是 回文 的。

你可以将 grid 中任意格子的值 翻转 ，也就是将格子里的值从 0 变成 1 ，或者从 1 变成 0 。

请你返回 最少 翻转次数，使得矩阵中 所有 行和列都是 回文的 ，且矩阵中 1 的数目可以被 4 整除 。

### 解答

```python
class Solution:
    def minFlips(self, grid: List[List[int]]) -> int:
        m = len(grid)
        n = len(grid[0])

        def func1(x_mid, y_mid):
            cur_ans = 0
            for x in range(x_mid):
                for y in range(y_mid):
                    one = (x, y)
                    two = (x, n - 1 - y)
                    three = (m - 1 - x, y)
                    four = (m - 1 - x, n - 1 - y)
                    total = sum(
                        [grid[item[0]][item[1]] for item in [one, two, three, four]]
                    )
                    match total:
                        case 1:
                            cur_ans += 1
                        case 2:
                            cur_ans += 2
                        case 3:
                            cur_ans += 1
                        case _:
                            continue
            return cur_ans

        def func2(x_mid, y_mid):
            one_count = 0
            two_count = 0
            for x in range(x_mid):
                one = (x, y_mid)
                two = (m - 1 - x, y_mid)
                total = sum([grid[item[0]][item[1]] for item in [one, two]])
                match total:
                    case 1:
                        one_count += 1
                    case 2:
                        two_count += 1
                if two_count == 2:
                    two_count = 0
            return one_count if one_count >= 1 else two_count * 2

        def func3(x_mid, y_mid):
            one_count = 0
            two_count = 0
            for y in range(y_mid):
                one = (x_mid, y)
                two = (x_mid, n - 1 - y)
                total = sum([grid[item[0]][item[1]] for item in [one, two]])
                match total:
                    case 1:
                        one_count += 1
                    case 2:
                        two_count += 1
                if two_count == 2:
                    two_count = 0
            return one_count if one_count >= 1 else two_count * 2

        def func4():
            cur_ans = 0
            mid = (m // 2, n // 2)
            mid_flag = False
            if grid[mid[0]][mid[1]] == 1:
                mid_flag = True
            step = 1
            one_count = 0
            two_count = 0
            while True:
                x_top = mid[0] - step
                x_bottom = mid[0] + step
                y_top = mid[1] - step
                y_bottom = mid[1] + step
                if x_top < 0 and y_top < 0:
                    break
                if y_top >= 0:
                    total = sum([grid[mid[0]][y_top], grid[mid[0]][y_bottom]])
                    match total:
                        case 1:
                            one_count += 1
                        case 2:
                            two_count += 1
                    if two_count == 2:
                        two_count = 0
                if x_top >= 0:
                    total = sum([grid[x_top][mid[1]], grid[x_bottom][mid[1]]])
                    match total:
                        case 1:
                            one_count += 1
                        case 2:
                            two_count += 1
                    if two_count == 2:
                        two_count = 0
                step += 1
            return (
                cur_ans
                + (1 if mid_flag else 0)
                + (one_count if one_count >= 1 else two_count * 2)
            )

        ans = 0
        if m % 2 == 0 and n % 2 == 0:
            ans += func1(m // 2, n // 2)
        elif m % 2 == 0 and n % 2 != 0:
            ans += func1(m // 2, n // 2)
            ans += func2(m // 2, n // 2)
        elif m % 2 != 0 and n % 2 == 0:
            ans += func1(m // 2, n // 2)
            ans += func3(m // 2, n // 2)
        else:
            ans += func1(m // 2, n // 2)
            ans += func4()
        print(ans)
        return ans
```

### 思路

据题知，有两大条件：
- 1的数量是4的倍数
- 行、列的回文

要分类讨论：偶数*偶数，奇数*偶数，偶数*奇数，奇数*奇数

1、那么就是需要所有的都要是所有的元素，对称的4个元素，都是1或者0。

2、单独成行或成列的，需要对称，但是因为要是4的倍数，所以需要做一下判断，首先不需要考虑一对都是0的
如果都是2个2，那么2对可以组成一个4；
如果1个1，1个2，需要翻转一次；
如果2个1，那么需要翻转2次； ，

3、对奇数*奇数的情况，会出现一个中心点，需要保证中心点必然为0，因为它是单独的一个点，无法成为1

### 复杂度

时间复杂度：O(N*M)

空间复杂度：O(1)
