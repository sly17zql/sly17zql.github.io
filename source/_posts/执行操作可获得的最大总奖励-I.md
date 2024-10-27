---
title: 执行操作可获得的最大总奖励 I
abbrlink: 64865
date: 2024-10-25 09:06:02
updated: 2024-10-25 09:06:02
tags: 
  - DP
description: 每天一道算法题，让生活变得更好
categories: 
  - 学习 
  - 代码
  - 算法
---

#### 执行操作可获得的最大总奖励 I

给你一个整数数组 `rewardValues`，长度为 `n`，代表奖励的值。

最初，你的总奖励 `x` 为 0，所有下标都是 **未标记** 的。你可以执行以下操作 **任意次** ：

- 从区间 `[0, n - 1]` 中选择一个 **未标记** 的下标 `i`。
- 如果 `rewardValues[i]` **大于** 你当前的总奖励 `x`，则将 `rewardValues[i]` 加到 `x` 上（即 `x = x + rewardValues[i]`），并 **标记** 下标 `i`。

以整数形式返回执行最优操作能够获得的 **最大** 总奖励。

#### 解答

```python
class Solution:
    def maxTotalReward(self, rewardValues: List[int]) -> int:
        length = len(rewardValues)
        rewardValues.sort()
        ret = {}

        for i in range(length):
            cur_num = rewardValues[i]
            ret[cur_num] = True
            num = cur_num - 1
            while num > 0:
                if num in ret:
                    ret[cur_num + num] = True
                num -= 1

        return max(ret.keys())
```

#### 思路

需要执行操作获得最大的奖励，假设当前想要获得X的奖励，那么我之前最多只能获得X-1的奖励，那么这是一个顺序有关的算法。

1、首先我们对**rewardValues**进行排序。

2、构造一个有序的集合，开始遍历我们可以获得的奖励列表**rewardValues**。

3、每个需要获取的奖励就是 rewardValues[index]，小于当前值的奖励都是可以获取到的，那么我们需要取更新整个**ret**。

4、重复上述动作，获取有序集合最大值就是最终答案。

#### 复杂度

N 是 rewardValues 的 长度，M是 rewardValues 的最大值

时间复杂度：O(NM)

空间复杂度：O(M)
