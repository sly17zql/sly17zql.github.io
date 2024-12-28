---
title: 【每日算法题】切蛋糕的最小总开销 I
tags:
  - 优先级队列
  - hashmap
description: 每天一道算法题，让生活变得更好
categories:
  - 学习
  - 代码
  - 算法
abbrlink: 29880
date: 2024-12-25 12:36:56
updated: 2024-12-25 12:36:56
---

### 切蛋糕的最小总开销 I

有一个 m x n 大小的矩形蛋糕，需要切成 1 x 1 的小块。

给你整数 m ，n 和两个数组：

horizontalCut 的大小为 m - 1 ，其中 horizontalCut[i] 表示沿着水平线 i 切蛋糕的开销。
verticalCut 的大小为 n - 1 ，其中 verticalCut[j] 表示沿着垂直线 j 切蛋糕的开销。
一次操作中，你可以选择任意不是 1 x 1 大小的矩形蛋糕并执行以下操作之一：

沿着水平线 i 切开蛋糕，开销为 horizontalCut[i] 。
沿着垂直线 j 切开蛋糕，开销为 verticalCut[j] 。
每次操作后，这块蛋糕都被切成两个独立的小蛋糕。

每次操作的开销都为最开始对应切割线的开销，并且不会改变。

请你返回将蛋糕全部切成 1 x 1 的蛋糕块的 最小 总开销。

### 解答

```python
class Solution:
    def minimumCost(self, m: int, n: int, horizontalCut: List[int], verticalCut: List[int]) -> int:
        @cache
        def dfs(left, right, top, bottom) -> int:
            if right == left + 1 and bottom == top + 1:
                return 0
            cur = math.inf
            for l in range(left + 1, right):
                cur = min(cur, dfs(l, right, top, bottom) + horizontalCut[l-1] + dfs(left, l, top, bottom))
            for t in range(top + 1, bottom):
                cur = min(cur, dfs(left, right, t, bottom) + verticalCut[t-1] + dfs(left, right, top, t))
            return cur

        return dfs(0, m, 0, n)
```

### 思路

使用记忆搜索法，记录当前块结构的 上下左右 的边界，当上下左右之间划分出来只有1*1时，则返回0，需要再经过0次切割。

起始位置是，(0, m, 0, n) 构造递归函数。

### 复杂度

时间复杂度：O(N^2)

空间复杂度：O(N^2)
