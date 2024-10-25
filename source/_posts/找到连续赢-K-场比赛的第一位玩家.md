---
title: 找到连续赢 K 场比赛的第一位玩家
abbrlink: 26085
date: 2024-10-24 15:00:35
updated: 2024-10-24 15:00:35
tags: 
  - 每日算法题
description: 每天一道算法题，让你变得更好
categories: 
  - 学习 
  - 代码
---

#### 找到连续赢 K 场比赛的第一位玩家

有 `n` 位玩家在进行比赛，玩家编号依次为 `0` 到 `n - 1` 。

给你一个长度为 `n` 的整数数组 `skills` 和一个 **正** 整数 `k` ，其中 `skills[i]` 是第 `i` 位玩家的技能等级。`skills` 中所有整数 **互不相同** 。

所有玩家从编号 `0` 到 `n - 1` 排成一列。

比赛进行方式如下：

- 队列中最前面两名玩家进行一场比赛，技能等级 **更高** 的玩家胜出。
- 比赛后，获胜者保持在队列的开头，而失败者排到队列的末尾。

这个比赛的赢家是 **第一位连续** 赢下 `k` 场比赛的玩家。

请你返回这个比赛的赢家编号。

#### 解答

```python
from typing import List


class Solution:
    def findWinningPlayer(self, skills: List[int], k: int) -> int:
        max_skill = skills[0]
        max_skill_index = 0
        for index, n in enumerate(skills[1:], start=1):
            if n > max_skill:
                max_skill_index = index
                max_skill = n

        length = len(skills)
        if k >= length - 1:
            return max_skill_index

        player_index = 0
        step = 0
        for i in range(1, length):
            if skills[player_index] > skills[i]:
                step += 1
            else:
                player_index = i
                step = 1
            if step == k:
                return player_index
        return max_skill_index
```

#### 思路

需要连赢K场，分两种情况判断：

1、如果要大于等于 length，那直接返回最大值 max_skill_index 的下标

2、如果是小于length，本质逻辑是遍历这个数组，找到当前的 player_index，并记录下当前可以连赢的场次，如果满足K的数量时，直接返回。否则，当遍历完一遍以后，还没有找到结果，那么此时player_index一定是数组的最大值 max_skill_index ，想要完成K，那么一定只能是数组的最大值 max_skill_index

#### 复杂度

时间复杂度: O(N)

空间复杂度: O(1)
