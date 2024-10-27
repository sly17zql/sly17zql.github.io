---
title: 构成整天的下标对数目 II
abbrlink: 26602
date: 2024-10-23 16:13:33
updated: 2024-10-23 16:13:33
tags: 
  - HASHMAP
description: 每天一道算法题，让生活变得更好
categories: 
  - 学习 
  - 代码
  - 算法
---

#### 题目

给你一个整数数组 hours，表示以 小时 为单位的时间，返回一个整数，表示满足 i < j 且 hours[i] + hours[j] 构成 整天 的下标对 i, j 的数目。

整天 定义为时间持续时间是 24 小时的 整数倍 。

例如，1 天是 24 小时，2 天是 48 小时，3 天是 72 小时，以此类推。

#### 解答

```python
class Solution:
    def countCompleteDayPairs(self, hours: List[int]) -> int:
        hours_map = defaultdict(int)
        for i in range(len(hours)):
            hours[i] = hours[i] % 24
            hours_map[hours[i]] += 1
        ans1 = 0
        ans2 = 0
        for k, v in hours_map.items():
            if k == 0 or k == 12:
                ans1 += v * (v - 1) // 2
                continue
            if 24 - k in hours_map:
                ans2 += hours_map[24 - k] * v
        return ans1 + ans2 // 2
```

### 

#### 思路

取余以后使用`hashmap`，对0和12做特殊处理，因为这两个值自身可以作为配对。

### 

#### 复杂度

时间复杂度: O(N)

空间复杂度: O(1)
