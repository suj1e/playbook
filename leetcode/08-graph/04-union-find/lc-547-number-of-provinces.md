# 547. Number of Provinces / 省份数量

[中文链接](https://leetcode.cn/problems/number-of-provinces/) | [English](https://leetcode.com/problems/number-of-provinces/)

**难度**：Medium
**标签**：深度优先搜索 | 广度优先搜索 | 并查集 | 图
**相似题目**：[lc-200](./lc-200-number-of-islands.md) | [lc-323](./) | [lc-1319](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

有 `n` 个城市，其中一些彼此相连，另一些没有相连。如果城市 `a` 和城市 `b` 直接相连，且城市 `b` 和城市 `c` 直接相连，那么城市 `a` 和城市 `c` 间接相连。

省份 是一组直接或间接相连的城市，组内不含其他没有相连的城市。

给你一个 `n x n` 的矩阵 `isConnected` ，其中 `isConnected[i][j] = 1` 表示第 `i` 个城市和第 `j` 个城市直接相连，而 `isConnected[i][j] = 0` 表示二者不直接相连。

返回矩阵中 **省份** 的数量。

**示例 1**：
```
输入：isConnected = [[1,1,0],[1,1,0],[0,0,1]]
输出：2
```

**示例 2**：
```
输入：isConnected = [[1,0,0],[0,1,0],[0,0,1]]
输出：3
```

**约束条件**：
- 1 <= n <= 200
- n == isConnected.length
- n == isConnected[i].length
- isConnected[i][j] 为 1 或 0
- isConnected[i][i] == 1
- isConnected[i][j] == isConnected[j][i]

---

## 解题思路

### 方法一：DFS

**思路**：
将城市看作图的节点，相连看作边，找连通分量数量。

**复杂度**：
- 时间：O(n²)
- 空间：O(n)

**步骤**：
1. 初始化 visited 数组
2. 遍历所有城市
3. 未访问过的城市，开始 DFS 标记相连城市
4. 每次新 DFS 代表一个新省份

---

### 方法二：BFS

**思路**：
与 DFS 类似，使用队列进行 BFS。

---

### 方法三：并查集（推荐）

**思路**：
将相连的城市合并到同一个集合。

**复杂度**：
- 时间：O(n² × α(n)) ≈ O(n²)
- 空间：O(n)

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：DFS
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        boolean[] visited = new boolean[n];
        int count = 0;

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(isConnected, visited, i);
                count++;
            }
        }

        return count;
    }

    private void dfs(int[][] isConnected, boolean[] visited, int i) {
        visited[i] = true;

        for (int j = 0; j < isConnected.length; j++) {
            if (isConnected[i][j] == 1 && !visited[j]) {
                dfs(isConnected, visited, j);
            }
        }
    }

    // 方法二：BFS
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        boolean[] visited = new boolean[n];
        int count = 0;
        Queue<Integer> queue = new LinkedList<>();

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                queue.offer(i);
                visited[i] = true;
                count++;

                while (!queue.isEmpty()) {
                    int city = queue.poll();
                    for (int j = 0; j < n; j++) {
                        if (isConnected[city][j] == 1 && !visited[j]) {
                            queue.offer(j);
                            visited[j] = true;
                        }
                    }
                }
            }
        }

        return count;
    }

    // 方法三：并查集
    public int findCircleNum(int[][] isConnected) {
        int n = isConnected.length;
        UnionFind uf = new UnionFind(n);

        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (isConnected[i][j] == 1) {
                    uf.union(i, j);
                }
            }
        }

        return uf.getCount();
    }

    class UnionFind {
        private int[] parent;
        private int[] rank;
        private int count;

        public UnionFind(int n) {
            parent = new int[n];
            rank = new int[n];
            count = n;
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }

        public int find(int x) {
            if (parent[x] != x) {
                parent[x] = find(parent[x]);  // 路径压缩
            }
            return parent[x];
        }

        public void union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);

            if (rootX != rootY) {
                if (rank[rootX] < rank[rootY]) {
                    parent[rootX] = rootY;
                } else if (rank[rootX] > rank[rootY]) {
                    parent[rootY] = rootX;
                } else {
                    parent[rootY] = rootX;
                    rank[rootX]++;
                }
                count--;
            }
        }

        public int getCount() {
            return count;
        }
    }
}
```

### Python

```python
from typing import List
from collections import deque

class Solution:
    # 方法一：DFS
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        n = len(isConnected)
        visited = [False] * n
        count = 0

        def dfs(i):
            visited[i] = True
            for j in range(n):
                if isConnected[i][j] == 1 and not visited[j]:
                    dfs(j)

        for i in range(n):
            if not visited[i]:
                dfs(i)
                count += 1

        return count

    # 方法二：BFS
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        n = len(isConnected)
        visited = [False] * n
        count = 0

        for i in range(n):
            if not visited[i]:
                queue = deque([i])
                visited[i] = True
                count += 1

                while queue:
                    city = queue.popleft()
                    for j in range(n):
                        if isConnected[city][j] == 1 and not visited[j]:
                            queue.append(j)
                            visited[j] = True

        return count

    # 方法三：并查集
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        n = len(isConnected)
        uf = UnionFind(n)

        for i in range(n):
            for j in range(i + 1, n):
                if isConnected[i][j] == 1:
                    uf.union(i, j)

        return uf.count

class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
        self.count = n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)

        if root_x != root_y:
            if self.rank[root_x] < self.rank[root_y]:
                self.parent[root_x] = root_y
            elif self.rank[root_x] > self.rank[root_y]:
                self.parent[root_y] = root_x
            else:
                self.parent[root_y] = root_x
                self.rank[root_x] += 1
            self.count -= 1
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 连通分量 | 省份就是图的连通分量 |
| visited 数组 | 标记已访问的城市 |
| 邻接矩阵 | isConnected[i][j] 表示连接关系 |
| 并查集 | 合并相连城市到同一集合 |
| 集合数量 | 最终的集合数就是省份数 |

---

## 图解

```
isConnected = [[1,1,0],
               [1,1,0],
               [0,0,1]]

图表示:
    0 ----- 1    2

DFS/BFS:
从 0 开始: visited = [T, T, F]
         count = 1

从 2 开始: visited = [T, T, T]
         count = 2

结果: 2 个省份

---

并查集:
初始: parent = [0, 1, 2], count = 3

i=0, j=1: isConnected[0][1] = 1
  union(0, 1): parent = [0, 0, 2], count = 2

i=0, j=2: isConnected[0][2] = 0, 跳过
i=1, j=2: isConnected[1][2] = 0, 跳过

最终: parent = [0, 0, 2]
      集合1: {0, 1}, 集合2: {2}
      count = 2
```

---

## 变种/延伸

### 变种1：岛屿数量
- **题目**：[lc-200 岛屿数量](./lc-200-number-of-islands.md)
- **变化**：网格图的连通分量
- **解法**：DFS/BFS/并查集

### 变种2：除法求值
- **题目**：[lc-399 除法求值](./)
- **变化**：给定方程，求解变量值
- **解法**：并查集判断依赖关系

### 变种3：冗余连接
- **题目**：[lc-684 冗余连接](./)
- **变化**：找到冗余的边
- **解法**：并查集检测环

### 进阶思考

1. **为什么题目保证 isConnected[i][i] = 1？**
   - 城市与自己肯定"相连"
   - 这是题目给定的条件

2. **并查集的路径压缩作用？**
   - 优化 find 操作
   - 将树状结构扁平化
   - 时间复杂度接近 O(1)

3. **如何判断两个城市在同一个省份？**
   - find(i) == find(j)
   - 根节点相同则属于同一集合

4. **如果图很大如何优化？**
   - 稀疏矩阵用邻接表
   - 避免存储全部矩阵

5. **如何输出每个省份包含的城市？**
   - 并查集：建立城市 -> 集合的映射
   - DFS/BFS：记录每个省份的城市列表
