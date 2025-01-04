---
title: 【每日算法题】我的日程安排表 II
tags:
  - 线段树
description: 每天一道算法题，让生活变得更好
categories:
  - 学习
  - 代码
  - 算法
abbrlink: 34773
date: 2025-01-03 19:21:58
udapted: 2025-01-03 19:21:58
---

### 我的日程安排表 II

实现一个程序来存放你的日程安排。如果要添加的时间内不会导致三重预订时，则可以存储这个新的日程安排。

当三个日程安排有一些时间上的交叉时（例如三个日程安排都在同一时间内），就会产生 三重预订。

事件能够用一对整数 startTime 和 endTime 表示，在一个半开区间的时间 [startTime, endTime) 上预定。实数 x 的范围为  startTime <= x < endTime。

实现 MyCalendarTwo 类：

MyCalendarTwo() 初始化日历对象。
boolean book(int startTime, int endTime) 如果可以将日程安排成功添加到日历中而不会导致三重预订，返回 true。否则，返回 false 并且不要将该日程安排添加到日历中。

### 解答

```python
class MyCalendarTwo:
    def __init__(self):
        self.calendar = {}

    def update(self, start, end, left, right, val, index):
        if start > right or end < left:
            return
        if start <= left and end >= right:
            cur_node = self.calendar.get(index, [0, 0])
            cur_node[0] += val
            cur_node[1] += val
            self.calendar[index] = cur_node
            return
        mid = (left + right) // 2
        self.update(start, end, left, mid, val, index * 2)
        self.update(start, end, mid + 1, right, val, (index * 2) + 1)
        cur_node = self.calendar.get(index, [0, 0])
        cur_node[0] = cur_node[1] + max(
            self.calendar.get(index * 2, [0, 0])[0],
            self.calendar.get((index * 2) + 1, [0, 0])[0],
        )
        self.calendar[index] = cur_node

    def book(self, startTime: int, endTime: int) -> bool:
        self.update(
            start=startTime, end=endTime - 1, left=0, right=10**9, val=1, index=1
        )
        if self.calendar[1][0] > 2:
            self.update(
                start=startTime, end=endTime - 1, left=0, right=10**9, val=-1, index=1
            )
            return False
        return True
```

### 思路

使用线段树实现，使用动态线段树，如果发现+1后，最高使用次数超过2时，则返回False，并且-1，恢复数据。

### 复杂度

时间复杂度：O(n*logm)

空间复杂度：O(n*logm)
