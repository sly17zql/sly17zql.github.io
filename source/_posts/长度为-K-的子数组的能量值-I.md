---
title: 【每日算法题】长度为 K 的子数组的能量值 I
tags:
  - 双指针
description: 每天一道算法题，让生活变得更好
categories:
  - 学习
  - 代码
  - 算法
abbrlink: 5501
date: 2024-11-06 10:14:52
updated: 2024-11-06 10:14:52
---

### 长度为 K 的子数组的能量值 I

给你一个长度为 n 的整数数组 nums 和一个正整数 k 。

一个数组的 能量值 定义为：

如果 所有 元素都是依次 连续 且 上升 的，那么能量值为 最大 的元素。 否则为 -1 。
你需要求出 nums 中所有长度为 k 的 子数组 的能量值。

请你返回一个长度为 n - k + 1 的整数数组 results ，其中 results[i] 是子数组 nums[i..(i + k - 1)] 的能量值。

### 解题

```python
class Solution:
    def resultsArray(self, nums: List[int], k: int) -> List[int]:
        if k == 1:
            return nums

        ans = []

        def init():
            cur_length = 1
            cur_pre = nums[0]
            for index in range(1, k):
                if nums[index] == cur_pre + 1:
                    cur_pre = nums[index]
                    cur_length += 1
                else:
                    cur_pre = nums[index]
                    cur_length = 1
            if cur_length != k:
                init_ans = -1
            else:
                init_ans = nums[k-1]
            ans.append(init_ans)
            return cur_length, cur_pre

        length, pre = init()
        end = k
        while end < len(nums):
            if length == k:
                length -= 1
            if nums[end] == pre + 1:
                length += 1
            else:
                length = 1
            if length == k:
                ans.append(nums[end])
            else:
                ans.append(-1)
            pre = nums[end]
            end += 1
        return ans
```

### 思路

如题意，判断定长数组是否连续递增。

1、先初始化第一个数组，记录当前连续递增长度，为后续添加元素做准备。

2、当append一个新元素时，判断与前置最后一个是否连续递增，并重新计算length

3、如果当前length == k，那么说明是K长度的连续递增数组，记录值

### 复杂度

时间复杂度：O(N)

空间复杂度：O(K)
