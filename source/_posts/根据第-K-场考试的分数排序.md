---
title: 【每日算法题】根据第 K 场考试的分数排序
tags:
  - 排序
description: 每天一道算法题，让生活变得更好
categories:
  - 学习
  - 代码
  - 算法
abbrlink: 9561
date: 2024-12-21 12:50:58
updated: 2024-12-21 12:50:58
---

### 根据第 K 场考试的分数排序

班里有 m 位学生，共计划组织 n 场考试。给你一个下标从 0 开始、大小为 m x n 的整数矩阵 score ，其中每一行对应一位学生，而 score[i][j] 表示第 i 位学生在第 j 场考试取得的分数。矩阵 score 包含的整数 互不相同 。

另给你一个整数 k 。请你按第 k 场考试分数从高到低完成对这些学生（矩阵中的行）的排序。

返回排序后的矩阵。

### 解答

```python
class Solution:
    def sortTheStudents(self, score: List[List[int]], k: int) -> List[List[int]]:
        score.sort(key=lambda x: -x[k])
        return score
```

### 思路

自定义排序

### 复杂度

时间复杂度：O(nlogn)

空间复杂度：O(1)
