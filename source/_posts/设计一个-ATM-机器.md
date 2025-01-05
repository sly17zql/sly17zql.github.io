---
title: 【每日算法题】设计一个 ATM 机器
date: 2025-01-05 11:34:24
updated: 2025-01-05 11:34:24
tags:
  - 列表
description: 每天一道算法题，让生活变得更好
categories:
  - 学习
  - 代码
  - 算法
---

### 设计一个 ATM 机器

一个 ATM 机器，存有 5 种面值的钞票：20 ，50 ，100 ，200 和 500 美元。初始时，ATM 机是空的。用户可以用它存或者取任意数目的钱。

取款时，机器会优先取 较大 数额的钱。

比方说，你想取 $300 ，并且机器里有 2 张 $50 的钞票，1 张 $100 的钞票和1 张 $200 的钞票，那么机器会取出 $100 和 $200 的钞票。
但是，如果你想取 $600 ，机器里有 3 张 $200 的钞票和1 张 $500 的钞票，那么取款请求会被拒绝，因为机器会先取出 $500 的钞票，然后无法取出剩余的 $100 。注意，因为有 $500 钞票的存在，机器 不能 取 $200 的钞票。
请你实现 ATM 类：

ATM() 初始化 ATM 对象。
void deposit(int[] banknotesCount) 分别存入 $20 ，$50，$100，$200 和 $500 钞票的数目。
int[] withdraw(int amount) 返回一个长度为 5 的数组，分别表示 $20 ，$50，$100 ，$200 和 $500 钞票的数目，并且更新 ATM 机里取款后钞票的剩余数量。如果无法取出指定数额的钱，请返回 [-1] （这种情况下 不 取出任何钞票）。

### 解答

```python
class ATM:
    def __init__(self):
        self.ticket = [20, 50, 100, 200, 500]
        self.count = [0, 0, 0, 0, 0]

    def deposit(self, banknotesCount: List[int]) -> None:
        for i in range(0, len(self.count)):
            self.count[i] += banknotesCount[i]

    def withdraw(self, amount: int) -> List[int]:
        ret = [0, 0, 0, 0, 0]
        for i in range(len(self.count)-1, -1, -1):
            need = amount // self.ticket[i]
            if self.count[i] <= 0:
                continue
            can_need = min(need, self.count[i])
            amount -= can_need * self.ticket[i]
            self.count[i] -= can_need
            ret[i] = can_need
            if amount == 0:
                return ret
        self.deposit(ret)
        return [-1]
```

### 思路

最简单的贪心算法，维护一个列表来统计每个面额的数量。

当每个需求进来，从大到小遍历，从大的里取出最大可以得到的值，依次遍历。最后如果无法满足，则将已经取出的ret在重新添加回去。

### 复杂度

调用次数：m，面额数量：n

时间复杂度：O(m * n)

空间复杂度：O(m * n)
