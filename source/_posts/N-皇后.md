---
title: 【每日算法题】N 皇后
date: 2024-12-01 09:46:45
updated: 2024-12-01 09:46:45
tags:
  - 回溯
description: 每天一道算法题，让生活变得更好
categories:
  - 学习
  - 代码
  - 算法
---

### N 皇后

n 皇后问题 研究的是如何将 n 个皇后放置在 n × n 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 n ，返回 n 皇后问题 不同的解决方案的数量。

### 解答

```python
class Solution:
    def totalNQueens(self, n: int) -> int:
        def inner(row, position):
            "position: {col: row}"
            if row >= n:
                return 1
            cur_ans = 0
            for col in range(n):
                flag = True
                if col in position:
                    continue
                for cur_col, cur_row in position.items():
                    if abs((row - cur_row) / (col - cur_col)) == 1:
                        flag = False
                        break
                if flag:
                    cur_ans += inner(row + 1, position | {col: row})
            return cur_ans

        ret = inner(0, {})
        return ret
        
```

### 思路

皇后可以直线，45°对角线移动，因为可以直线移动，所以要保证每行只有一个。

回溯入口就是从1行到n行，并使用hashmap记录，保证没有重复的列，已经需要check，保证之前所处的位置没有和现位置处于45°对角线上。

### 复杂度

时间复杂度：O(n!)

空间复杂度：O(n)
