---
title: 【每日算法题】统计满足 K 约束的子字符串数量 I
date: 2024-11-12 10:14:32
tags:
  - 滑动窗口
description: 每天一道算法题，让生活变得更好
categories:
  - 学习
  - 代码
  - 算法
---

### 统计满足 K 约束的子字符串数量 I

给你一个 二进制 字符串 s 和一个整数 k。

如果一个 二进制字符串 满足以下任一条件，则认为该字符串满足 k 约束：

- 字符串中 0 的数量最多为 k。
- 字符串中 1 的数量最多为 k。

返回一个整数，表示 s 的所有满足 k 约束 的 子字符串 的数量。

### 解答 

```python
class Solution:
    def countKConstraintSubstrings(self, s: str, k: int) -> int:
        left = 0
        right = 0
        ans = 0
        pre_1 = 1 if s[0] == '1' else 0
        pre_0 = 1 if s[0] == '0' else 0
        ans += 1
        while right < len(s) - 1:
            cur = s[right+1]
            pre_1 += 1 if cur == "1" else 0
            pre_0 += 1 if cur == "0" else 0
            if pre_1 > k and pre_1 > k:
                while pre_0 > k and pre_1 > k:
                    pre_0 -= 1 if s[left] == '0' else 0
                    pre_1 -= 1 if s[left] == "1" else 0
                    left += 1
            right += 1
            ans += (right - left) + 1
        return ans
```

### 思路

1和0的统计值任意一个小于等于K 就是合法字符串。可以以右边界为基准进行计算，并实时维护前置的 0 和 1 的统计值。加上当前值如果是合法的，那么加入 右边界 减去 左边界 的长度+1的数量。

如果加入当前值，已经不合法，那从前开始剔除数值，直到再次符合时，下次就直接在当前维护的left开始进行统计。

### 复杂度

时间复杂度：O(N)

空间复杂度：O(1)
