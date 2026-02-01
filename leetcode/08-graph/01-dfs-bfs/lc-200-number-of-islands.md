# 200. Number of Islands / 岛屿数量

[中文链接](https://leetcode.cn/problems/number-of-islands/) | [English](https://leetcode.com/problems/number-of-islands/)

**难度**：Medium
**标签**：深度优先搜索 | 广度优先搜索 | 并查集 | 矩阵
**相似题目**：[lc-695](./) | [lc-547](./lc-547-number-of-provinces.md) | [lc-130](./)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

**示例 1**：
```
输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","1","1"]
]
输出：3
```

**示例 2**：
```
输入：grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
输出：3
```

**约束条件**：
- m == grid.length
- n == grid[i].length
- 1 <= m, n <= 300
- grid[i][j] 的值为 '0' 或 '1'

---

## 解题思路

### 方法一：DFS（推荐）

**思路**：
遍历网格，遇到陆地 '1' 时，使用 DFS 将相连的陆地都标记为已访问。

**复杂度**：
- 时间：O(m × n)
- 空间：O(m × n) 最坏情况

**步骤**：
1. 遍历每个格子
2. 遇到 '1'，岛屿数 +1
3. DFS 将相连的 '1' 标记为 '0'
4. 继续遍历

---

### 方法二：BFS

**思路**：
使用队列进行 BFS，与 DFS 类似。

---

### 方法三：并查集

**思路**：
将相连的陆地合并到同一个集合，最后统计集合数量。

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：DFS
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) return 0;

        int m = grid.length, n = grid[0].length;
        int count = 0;

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    count++;
                    dfs(grid, i, j);
                }
            }
        }

        return count;
    }

    private void dfs(char[][] grid, int i, int j) {
        int m = grid.length, n = grid[0].length;

        // 越界或已访问
        if (i < 0 || i >= m || j < 0 || j >= n || grid[i][j] == '0') {
            return;
        }

        // 标记为已访问
        grid[i][j] = '0';

        // DFS 四个方向
        dfs(grid, i + 1, j);
        dfs(grid, i - 1, j);
        dfs(grid, i, j + 1);
        dfs(grid, i, j - 1);
    }

    // 方法二：BFS
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) return 0;

        int m = grid.length, n = grid[0].length;
        int count = 0;
        Queue<int[]> queue = new LinkedList<>();

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    count++;
                    grid[i][j] = '0';
                    queue.offer(new int[]{i, j});

                    while (!queue.isEmpty()) {
                        int[] cell = queue.poll();
                        int row = cell[0], col = cell[1];

                        // 四个方向
                        int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
                        for (int[] dir : dirs) {
                            int newRow = row + dir[0];
                            int newCol = col + dir[1];

                            if (newRow >= 0 && newRow < m && newCol >= 0 && newCol < n
                                && grid[newRow][newCol] == '1') {
                                grid[newRow][newCol] = '0';
                                queue.offer(new int[]{newRow, newCol});
                            }
                        }
                    }
                }
            }
        }

        return count;
    }
}
```

### Python

```python
from typing import List
from collections import deque

class Solution:
    # 方法一：DFS
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid or not grid[0]:
            return 0

        m, n = len(grid), len(grid[0])
        count = 0

        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1':
                    count += 1
                    self.dfs(grid, i, j)

        return count

    def dfs(self, grid, i, j):
        m, n = len(grid), len(grid[0])

        if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] == '0':
            return

        grid[i][j] = '0'

        self.dfs(grid, i + 1, j)
        self.dfs(grid, i - 1, j)
        self.dfs(grid, i, j + 1)
        self.dfs(grid, i, j - 1)

    # 方法二：BFS
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid or not grid[0]:
            return 0

        m, n = len(grid), len(grid[0])
        count = 0

        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1':
                    count += 1
                    grid[i][j] = '0'
                    queue = deque([(i, j)])

                    while queue:
                        row, col = queue.popleft()

                        for dr, dc in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                            nr, nc = row + dr, col + dc
                            if 0 <= nr < m and 0 <= nc < n and grid[nr][nc] == '1':
                                grid[nr][nc] = '0'
                                queue.append((nr, nc))

        return count
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 标记已访问 | 将访问过的 '1' 改为 '0'，避免重复访问 |
| 四个方向 | 上下左右相邻的格子 |
| DFS vs BFS | DFS 递归简洁，BFS 队列实现 |
| 越界检查 | 检查索引是否在有效范围内 |
| 连通分量 | 每次找到新的 '1' 就是一个新的连通分量 |

---

## 图解

```
网格:
[1,1,0,0,0]
[1,1,0,1,1]
[0,0,0,1,1]

遍历过程:
(0,0) 是 '1', count = 1
DFS 从 (0,0) 开始:
  标记 (0,0) -> '0'
  DFS (1,0) -> '0', DFS (1,1) -> '0'
  DFS (0,1) -> '0'

网格变为:
[0,0,0,0,0]
[0,0,0,1,1]
[0,0,0,1,1]

继续遍历, (1,3) 是 '1', count = 2
DFS 从 (1,3) 开始, 标记 (1,3), (1,4), (2,3), (2,4)

网格变为:
[0,0,0,0,0]
[0,0,0,0,0]
[0,0,0,0,0]

最终 count = 2
```

---

## 变种/延伸

### 变种1：岛屿的最大面积
- **题目**：[lc-695 岛屿的最大面积](./)
- **变化**：找最大岛屿的面积
- **解法**：DFS/BFS 时计算面积

### 变种2：省份数量
- **题目**：[lc-547 省份数量](./lc-547-number-of-provinces.md)
- **变化**：给定城市连接矩阵，找省份数量
- **解法**：DFS/BFS 或并查集

### 变种3：被围绕的区域
- **题目**：[lc-130 被围绕的区域](./)
- **变化**：找不能从边界到达的 'O'
- **解法**：从边界的 'O' 开始 DFS 标记

### 进阶思考

1. **DFS 和 BFS 哪个更好？**
   - DFS 代码更简洁
   - BFS 可以用队列，避免递归栈溢出

2. **如何处理超大网格？**
   - 使用 BFS + 迭代
   - 避免递归栈溢出
   - 可以使用并查集

3. **如果只需要判断是否形成岛屿？**
   - 第一次 DFS 后，检查是否还有 '1'
   - 如果有，说明有多个岛屿

4. **并查集如何实现？**
   - 每个 '1' 是一个集合
   - 相邻的 '1' 合并
   - 最后统计集合数量

5. **如何求岛屿周长？**
   - DFS 时计算
   - 每个方向是水或边界，周长 +1
- **题目**：[lc-463 岛屿的周长](./)
