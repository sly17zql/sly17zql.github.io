---
title: 冗余连接 II
tags:
  - 图
  - 并查集
description: 每天一道算法题，让生活变得更好
categories:
  - 学习
  - 代码
  - 算法
abbrlink: 38545
date: 2024-10-28 09:33:00
updated: 2024-10-28 09:33:00
---

#### 冗余连接 II

在本问题中，有根树指满足以下条件的 有向 图。该树只有一个根节点，所有其他节点都是该根节点的后继。该树除了根节点之外的每一个节点都有且只有一个父节点，而根节点没有父节点。

输入一个有向图，该图由一个有着 n 个节点（节点值不重复，从 1 到 n）的树及一条附加的有向边构成。附加的边包含在 1 到 n 中的两个不同顶点间，这条附加的边不属于树中已存在的边。

结果图是一个以边组成的二维数组 edges 。 每个元素是一对 [ui, vi]，用以表示 有向 图中连接顶点 ui 和顶点 vi 的边，其中 ui 是 vi 的一个父节点。

返回一条能删除的边，使得剩下的图是有 n 个节点的有根树。若有多个答案，返回最后出现在给定二维数组的答案。

#### 解答

```python
class UnionFind:
    def __init__(self, n):
        self.parent = {
            i: i
            for i in range(1, n+1)
        }

    def find(self, node):
        if self.parent[node] != node:
            self.parent[node] = self.find(self.parent[node])
        return self.parent[node]

    def union(self, node1, node2):
        self.parent[self.find(node1)] = self.find(node2)


class Solution:
    def findRedundantDirectedConnection(self, edges: List[List[int]]) -> List[int]:
        union_find = UnionFind(len(edges))
        node_parent_map = {}
        for (start, end) in edges:
            if end in node_parent_map:
                # 先计算入度值，如果入度为 2 ，则表示两条边有一条是重复边
                for remove in [[start, end], [node_parent_map[end], end]]:
                    if self.is_link(edges, remove):
                        return remove
            node_parent_map[end] = start
            if union_find.find(start) == union_find.find(end):
                # 成环
                circle = [start, end]
            else:
                union_find.union(start, end)
        return circle

    def is_link(self, edges, remove):
        union_find = UnionFind(len(edges))
        for i in range(len(edges)):
            if edges[i] == remove:
                continue
            union_find.union(edges[i][0], edges[i][1])
        root = {union_find.find(i) for i in range(1, len(edges)+1)}
        return True if len(root) == 1 else False
```

#### 思路

本题可以继续套用的昨天的方法，遍历edges + 图的深度遍历，不过需要注意的是这次是有向图，需要做略微调整。
{% post_link 冗余连接 %}

又学习了一下使用并查集的方法来进行编写。
- 根据题意，本次是多了一条边，那么有向树多了一条边，可以考虑两种情况：1）多一条边的终点是根节点；2）多一条边的终点是非根节点。
- 如果是根节点，那么可以得知，那么必然会成环，那在遍历过程中，可以使用并查集判断当前edge是否为环上的edge，我们只需要pop最后一条环上的边即可。
- 如果是非根节点，那么可以得知，那么必然有一个节点的入度为2，那么就是这两条之中的其中一条是多出来的边。
- 代码方面，我们需要实现一个并查集，实现一个判断删除某条边是否可以成树的判断函数。

#### 复杂度

时间复杂度：O(Nα(N))，其中 α 为阿克曼函数的反函数，是并查集的一个时间复杂度。

空间复杂度：O(N)
