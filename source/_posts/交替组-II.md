---
title: 【每日算法题】交替组 II
date: 2024-11-27 21:17:13
tags:
  - 滑动窗口
description: 每天一道算法题，让生活变得更好
categories:
  - 学习
  - 代码
  - 算法
---

### 交替组 II

给你一个整数数组 colors 和一个整数 k ，colors表示一个由红色和蓝色瓷砖组成的环，第 i 块瓷砖的颜色为 colors[i] ：

colors[i] == 0 表示第 i 块瓷砖的颜色是 红色 。
colors[i] == 1 表示第 i 块瓷砖的颜色是 蓝色 。
环中连续 k 块瓷砖的颜色如果是 交替 颜色（也就是说除了第一块和最后一块瓷砖以外，中间瓷砖的颜色与它 左边 和 右边 的颜色都不同），那么它被称为一个 交替 组。

请你返回 交替 组的数目。

注意 ，由于 colors 表示一个 环 ，第一块 瓷砖和 最后一块 瓷砖是相邻的。

### 解答

```python
class Solution:
    def numberOfAlternatingGroups(self, colors: List[int], k: int) -> int:
        ans = 0
        colors += colors[:k-1]

        start = 0
        pre = colors[start]
        for i in range(1, len(colors)):
            cur = colors[i]
            if cur == pre:
                start = i
                end = i
                continue
            end = i
            pre = colors[i]
            if end - start + 1 == k:
                ans += 1
                start = start + 1
        return ans
```

### 思路

据题知，因为 k<length，所以可以在color末尾添加k-1的长度，colors += colors[:k-1]，作为需要遍历的整个长度。

记录start和end两个index，维护的是合法的一个块，如果当前合法块的长度等于k，那么表示能够成为答案，否则，继续往后遍历。

### 复杂度

k 为 k的值，n为colors的长度

时间复杂度：O(K+N)

空间复杂度：O(1)
