---
title: 【每日算法题】新增道路查询后的最短距离 I
date: 2024-11-20 10:47:58
tags:
  - 广度遍历
categories:
  - 学习 
  - 代码
  - 算法
---

### 新增道路查询后的最短距离 I

给你一个整数 n 和一个二维整数数组 queries。

有 n 个城市，编号从 0 到 n - 1。初始时，每个城市 i 都有一条单向道路通往城市 i + 1（ 0 <= i < n - 1）。

queries[i] = [ui, vi] 表示新建一条从城市 ui 到城市 vi 的单向道路。每次查询后，你需要找到从城市 0 到城市 n - 1 的最短路径的长度。

返回一个数组 answer，对于范围 [0, queries.length - 1] 中的每个 i，answer[i] 是处理完前 i + 1 个查询后，从城市 0 到城市 n - 1 的最短路径的长度。

### 解答

```python
class Solution:
    def shortestDistanceAfterQueries(self, n: int, queries: List[List[int]]) -> List[int]:
        g = [[i + 1] for i in range(n - 1)]
        vis = [-1] * (n - 1)

        def bfs(i: int) -> int:
            q = [0]
            step = 1
            while True:
                new_q = []
                for node in q:
                    for neighbor in g[node]:
                        if neighbor == n - 1:
                            return step
                        if vis[neighbor] != i:
                            vis[neighbor] = i
                            new_q.append(neighbor)
                step += 1
                q = new_q

        ans = [0] * len(queries)
        for i, (l, r) in enumerate(queries):
            g[l].append(r)
            ans[i] = bfs(i)
        return ans
```

### 思路

构造图，随后每次新增路径后，维护图信息。重新进行遍历，需要优化点是，已经在当前遍历中到达过的点就不会在遍历了。

例如：

1可以到 2、3，2可以到3。我在访问到1时，已经遍历计算达到3了，那么2再去到3的时候，可以忽略这个3的重复计算了。

### 复杂度

n 为 N 的长度，m 为 queries的长度.

时间复杂度：O(n*(n+m))

空间复杂度：O(m+n)
