# 743. Network Delay Time / 网络延迟时间

[中文链接](https://leetcode.cn/problems/network-delay-time/) | [English](https://leetcode.com/problems/network-delay-time/)

**难度**：Medium
**标签**：深度优先搜索 | 广度优先搜索 | 图 | 最短路径 | 堆
**相似题目**：[lc-1514](./) | [lc-778](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

有 `n` 个网络节点，标记为 `1` 到 `n`。

给你一个列表 `times`，表示信号经过有向边的传递时间。`times[i] = (u, v, w)`，其中 `u` 是源节点，`v` 是目标节点，`w` 是一个信号从源节点传递到目标节点的时间。

现在，我们从某个节点 `K` 发出一个信号。需要多久才能使所有节点都收到信号？如果不能使所有节点收到信号，返回 `-1` 。

**示例 1**：
```
输入：times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
输出：2
```

**示例 2**：
```
输入：times = [[1,2,1]], n = 2, k = 1
输出：1
```

**示例 3**：
```
输入：times = [[1,2,1]], n = 2, k = 2
输出：-1
```

**约束条件**：
- 1 <= k <= n <= 100
- 1 <= times.length <= 6000
- times[i].length == 3
- 1 <= ui, vi <= n
- ui != vi
- 0 <= wi <= 100
- 所有 (ui, vi) 对都是唯一的（不含重复边）

---

## 解题思路

### 方法一：Dijkstra 算法（推荐）

**思路**：
计算从节点 K 到所有其他节点的最短路径，返回其中最大的值。

**核心思想**：
- **单源最短路径**：从起点到所有点的最短距离
- **贪心选择**：每次选择距离最小的未访问节点
- **优先队列优化**：使用堆优化获取最小距离

**复杂度**：
- 时间：O((V + E) log V) - 堆优化的 Dijkstra
- 空间：O(V + E) - 邻接表 + 距离数组

**步骤**：
1. 构建邻接表
2. 初始化距离数组，起点为 0，其他为无穷大
3. 使用优先队列（最小堆），存储 (距离, 节点)
4. 每次取出距离最小的节点
5. 更新其邻居的距离
6. 返回最大距离，如果有节点未访问则返回 -1

### 方法二：Bellman-Ford

**思路**：
动态规划，每次松弛所有边，最多 n-1 轮。

**复杂度**：
- 时间：O(V × E)
- 空间：O(V)

---

## 代码实现

### Java - Dijkstra（堆优化）

```java
class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        // 构建邻接表
        Map<Integer, List<int[]>> graph = new HashMap<>();
        for (int[] edge : times) {
            int from = edge[0], to = edge[1], weight = edge[2];
            graph.computeIfAbsent(from, x -> new ArrayList<>()).add(new int[]{to, weight});
        }

        // 距离数组，初始化为无穷大
        int[] dist = new int[n + 1];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[k] = 0;

        // 优先队列（最小堆）：[距离, 节点]
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        pq.offer(new int[]{0, k});

        while (!pq.isEmpty()) {
            int[] curr = pq.poll();
            int d = curr[0], node = curr[1];

            // 如果当前距离大于已知距离，跳过
            if (d > dist[node]) {
                continue;
            }

            // 遍历邻居
            if (graph.containsKey(node)) {
                for (int[] neighbor : graph.get(node)) {
                    int nextNode = neighbor[0], weight = neighbor[1];
                    int newDist = dist[node] + weight;

                    // 松弛操作
                    if (newDist < dist[nextNode]) {
                        dist[nextNode] = newDist;
                        pq.offer(new int[]{newDist, nextNode});
                    }
                }
            }
        }

        // 找出最大距离
        int maxDist = 0;
        for (int i = 1; i <= n; i++) {
            if (dist[i] == Integer.MAX_VALUE) {
                return -1;  // 有节点不可达
            }
            maxDist = Math.max(maxDist, dist[i]);
        }

        return maxDist;
    }
}
```

### Java - 简化版（数组优先队列）

```java
class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        // 构建邻接矩阵
        int[][] graph = new int[n + 1][n + 1];
        for (int i = 1; i <= n; i++) {
            Arrays.fill(graph[i], Integer.MAX_VALUE / 2);
        }
        for (int[] edge : times) {
            graph[edge[0]][edge[1]] = edge[2];
        }

        // 距离数组
        int[] dist = new int[n + 1];
        Arrays.fill(dist, Integer.MAX_VALUE / 2);
        dist[k] = 0;

        // 访问标记
        boolean[] visited = new boolean[n + 1];

        for (int i = 0; i < n; i++) {
            // 找未访问的最小距离节点
            int minDist = Integer.MAX_VALUE;
            int minNode = -1;
            for (int j = 1; j <= n; j++) {
                if (!visited[j] && dist[j] < minDist) {
                    minDist = dist[j];
                    minNode = j;
                }
            }

            if (minNode == -1) break;
            visited[minNode] = true;

            // 松弛邻居
            for (int j = 1; j <= n; j++) {
                if (!visited[j]) {
                    dist[j] = Math.min(dist[j], dist[minNode] + graph[minNode][j]);
                }
            }
        }

        int maxDist = 0;
        for (int i = 1; i <= n; i++) {
            if (dist[i] == Integer.MAX_VALUE / 2) return -1;
            maxDist = Math.max(maxDist, dist[i]);
        }

        return maxDist;
    }
}
```

### Python - Dijkstra（堆优化）

```python
import heapq
from typing import List

class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        # 构建邻接表
        graph = {i: [] for i in range(1, n + 1)}
        for u, v, w in times:
            graph[u].append((v, w))

        # 距离数组
        dist = {i: float('inf') for i in range(1, n + 1)}
        dist[k] = 0

        # 优先队列
        pq = [(0, k)]  # (距离, 节点)

        while pq:
            d, node = heapq.heappop(pq)

            if d > dist[node]:
                continue

            for neighbor, weight in graph[node]:
                new_dist = dist[node] + weight
                if new_dist < dist[neighbor]:
                    dist[neighbor] = new_dist
                    heapq.heappush(pq, (new_dist, neighbor))

        max_dist = max(dist.values())

        return max_dist if max_dist != float('inf') else -1
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 邻接表 | 用 Map 存储，稀疏图更高效 |
| 优先队列 | 最小堆，每次取最小距离 |
| 松弛操作 | if (newDist < dist[next]) 更新 |
| 跳过过期 | if (d > dist[node]) continue |
| 结果判断 | 所有节点最短距离的最大值 |

---

## 图解

```
times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2

图结构:
    1           4
    ↑           ↑
    |1          |1
    |           |
    2 ←-----→  3
         1

邻接表:
1: []
2: [(1,1), (3,1)]
3: [(4,1)]
4: []

---

Dijkstra 过程:

初始状态:
dist = [∞, 0, ∞, ∞, ∞]  (从索引1开始: dist[1]=∞, dist[2]=0, ...)
pq = [(0, 2)]

---

第1轮: 取出 (0, 2)
更新邻居 1: dist[1] = 0 + 1 = 1, pq = [(1, 1)]
更新邻居 3: dist[3] = 0 + 1 = 1, pq = [(1, 1), (1, 3)]

dist = [∞, 1, 0, 1, ∞]

---

第2轮: 取出 (1, 1)
节点 1 没有邻居

dist = [∞, 1, 0, 1, ∞]

---

第3轮: 取出 (1, 3)
更新邻居 4: dist[4] = 1 + 1 = 2, pq = [(2, 4)]

dist = [∞, 1, 0, 1, 2]

---

第4轮: 取出 (2, 4)
节点 4 没有邻居

dist = [∞, 1, 0, 1, 2]

---

pq 为空，结束

最大距离 = max(1, 0, 1, 2) = 2

返回: 2

---

路径示意:
从节点 2 出发:
2 → 1: 距离 1
2 → 3: 距离 1
2 → 3 → 4: 距离 2

最短距离数组:
节点 1: 1
节点 2: 0 (起点)
节点 3: 1
节点 4: 2

答案: 2
```

---

## 变种/延伸

### 变种1：路径总和
- **题目**：[lc-1514 概率最大的路径](./)
- **变化**：找最大概率路径（相当于最长路径）
- **解法**：Dijkstra 变种

### 变种2：游泳
- **题目**：[lc-778 游泳水位上升](./)
- **变化**：找最小最大值的路径
- **解法**：并查集或 Dijkstra

### 变种3：Cheapest Flights
- **题目**：[lc-787 最便宜的航班](./)
- **变化**：最多 K 站中转的最便宜价格
- **解法**：Bellman-Ford 或 DP

### 进阶思考

1. **为什么用 Dijkstra 而不是 BFS？**
   - 边权不为 1，BFS 无法处理
   - Dijkstra 可以处理任意非负权边

2. **如果边权有负数？**
   - Dijkstra 不适用
   - 需要用 Bellman-Ford 或 SPFA

3. **时间复杂度分析？**
   - 堆优化：O((V + E) log V)
   - 数组版：O(V²)
   - 稀疏图用堆，稠密图用数组

4. **如何恢复路径？**
   - 记录 parent 数组
   - parent[v] = u 表示 v 由 u 松弛得到

5. **空间优化？**
   - 邻接矩阵：O(V²)
   - 邻接表：O(V + E)
   - 稀疏图用邻接表更高效

6. **与最小生成树的区别？**
   - MST：无向图，连通所有顶点的最小边权和
   - 最短路径：有向/无向，单源到所有点的最小距离
