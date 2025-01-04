---
title: 【每日算法题】我的日程安排表 I
date: 2025-01-02 19:14:49
updated: 2025-01-02 19:14:49
tags:
  - 二分排序
description: 每天一道算法题，让生活变得更好
categories:
  - 学习
  - 代码
  - 算法
---

### 我的日程安排表 I

实现一个 MyCalendar 类来存放你的日程安排。如果要添加的日程安排不会造成 重复预订 ，则可以存储这个新的日程安排。

当两个日程安排有一些时间上的交叉时（例如两个日程安排都在同一时间内），就会产生 重复预订 。

日程可以用一对整数 startTime 和 endTime 表示，这里的时间是半开区间，即 [startTime, endTime), 实数 x 的范围为，  startTime <= x < endTime 。

实现 MyCalendar 类：

MyCalendar() 初始化日历对象。
boolean book(int startTime, int endTime) 如果可以将日程安排成功添加到日历中而不会导致重复预订，返回 true 。否则，返回 false 并且不要将该日程安排添加到日历中。

### 解答

```python
class MyCalendar:
    def __init__(self):
        from sortedcontainers import SortedList

        self.calendar = SortedList()

    def book(self, startTime: int, endTime: int) -> bool:
        if len(self.calendar) == 0:
            self.calendar.add([startTime, endTime])
            return True

        n = self._find_right(startTime, 0)
        start1, end1 = self.calendar[n]
        if start1 == startTime:
            return False
        elif startTime < start1:
            if start1 < endTime:
                return False
            if n > 0:
                next_node = self.calendar[n - 1]
                start2, end2 = next_node
                if start2 < startTime and end2 > startTime:
                    return False
                if end2 == startTime:
                    self.calendar.remove(next_node)
                    self.calendar.add([start2, endTime])
                    return True
            self.calendar.add([startTime, endTime])
        else:
            if end1 > startTime:
                return False
            self.calendar.add([startTime, endTime])
        return True

    def _find_right(self, node, index):
        left = 0
        right = len(self.calendar) - 1
        while left < right:
            mid = (left + right) // 2
            if node > self.calendar[mid][index]:
                left = mid + 1
            else:
                right = mid
        return left
```

### 思路

每个预定的时段不能出现重复，我第一时间想到了二分法，维护一个有序列表。

当每段新的预约时间出现后，找到第一个大于等于 startTime 的区段：

- 如果等于，因为时间>0，所以一定会重叠
- 如果大于的时段，且该时段的开始时间小于endTime，会重叠
  - 有大于的时段时，看一下index-1的小于时段有没有，如果有，该区段的结束时间如果大于startTime，则会重叠
- 如果没有找到大于的时段，直接找小于的时段，该区段的结束时间如果大于startTime，则会重叠

### 复杂度

操作次数：n，时间区段：m

时间复杂度：O(n * logm)

空间复杂度：O(m)
