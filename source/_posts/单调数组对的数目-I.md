---
title: 【每日算法题】单调数组对的数目 I
date: 2024-11-28 13:06:13
updated: 2024-11-28 13:06:13
tags:
  - 遍历
description: 每天一道算法题，让生活变得更好
categories:
  - 学习
  - 代码
  - 算法
---

### 单调数组对的数目 I

给你一个长度为 n 的 正 整数数组 nums 。

如果两个 非负 整数数组 (arr1, arr2) 满足以下条件，我们称它们是 单调 数组对：

两个数组的长度都是 n 。
arr1 是单调 非递减 的，换句话说 arr1[0] <= arr1[1] <= ... <= arr1[n - 1] 。
arr2 是单调 非递增 的，换句话说 arr2[0] >= arr2[1] >= ... >= arr2[n - 1] 。
对于所有的 0 <= i <= n - 1 都有 arr1[i] + arr2[i] == nums[i] 。
请你返回所有 单调 数组对的数目。

由于答案可能很大，请你将它对 10^9 + 7 取余 后返回。

### 解答

```python
class Solution:
    def countOfPairs(self, nums: List[int]) -> int:
        @cache
        def inner(index, pre1, pre2) -> int:
            if index >= len(nums):
                return 1
            total = nums[index]
            cur = 0
            for first in range(pre1, total+1):
                second = total - first
                if second > pre2:
                    continue
                cur += inner(index+1, first, second)
            return cur

        ans = 0
        for num in range(0, nums[0]+1):
            ans += inner(1, num, nums[0] - num)
        return ans % (10**9 + 7)
```

### 思路

任务的目的是组成一个非递减和一个非递增的两个数组，他们每个index元素之和是原数组index的值。

使用分治思想，递归实现，添加记忆法。

从 第 0 位的index开始，第一个非递减数组0位 list1[0] 可以从 0 - nums[0] 都可以取，那么对应的非递增数组，nums[0] - (nums[0] - list1[0])

对i+1位的判断则是，非递减数组的index位，需要 取 大于等于 pre1 的数值才可以保证非递减成立，同样的，非递增数组的index位，需要取 小于等于 pre2 的数值才可以保证非递增成立。

直到index 大于 len(nums)-1 时，是递归退出的点。

### 复杂度

时间复杂度：O()

空间复杂度：O()
