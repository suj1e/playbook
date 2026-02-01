# 994. Rotting Oranges / 腐烂的橘子

[中文链接](https://leetcode.cn/problems/rotting-oranges/) | [English](https://leetcode.com/problems/rotting-oranges/)

**难度**：Medium
**标签**：广度优先搜索 | 数组 | 矩阵
**相似题目**：[lc-200](./lc-200-number-of-islands.md) | [lc-286](./) | [lc-1765](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

在给定的 `m x n` 网格 `grid` 中，每个单元格可以有以下三个值之一：

- 值 `0` 代表空单元格
- 值 `1` 代表新鲜橘子
- 值 `2` 代表腐烂的橘子

每分钟，腐烂的橘子 **周围 4 个方向上** 相邻的新鲜橘子都会腐烂。

返回直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 `-1`。

**示例 1**：
```
输入：grid = [[2,1,1],[1,1,0],[0,1,1]]
输出：4
```

**示例 2**：
```
输入：grid = [[2,1,1],[0,1,1],[1,0,1]]
输出：-1
解释：左下角的橘子（第 2 行，第 0 列）永远不会腐烂，因为腐烂的橘子（第 0 行，第 0 列）每分钟只会向 4 个方向移动。
```

**示例 3**：
```
输入：grid = [[0,2]]
输出：0
解释：因为 0 分钟时已经没有新鲜橘子了，所以答案就是 0 。
```

**约束条件**：
- m == grid.length
- n == grid[i].length
- 1 <= m, n <= 10
- grid[i][j] 仅为 0、1 或 2

---

## 解题思路

### 方法一：BFS 多源（推荐）

**思路**：
将所有初始腐烂的橘子作为 BFS 的起点同时入队，模拟腐烂过程。

**核心思想**：
- **多源 BFS**：多个腐烂橘子同时扩散
- **层序遍历**：每分钟对应 BFS 的一层
- **计数**：统计剩余的新鲜橘子数量

**复杂度**：
- 时间：O(m × n) - 每个单元格最多访问一次
- 空间：O(m × n) - 队列最坏情况存储所有单元格

**步骤**：
1. 将所有腐烂橘子（2）入队，同时统计新鲜橘子数量
2. BFS 层序遍历，每分钟处理一层
3. 每处理一个腐烂橘子，将其邻居新鲜橘子腐烂并入队
4. 新鲜橘子数量减 1
5. 遍历结束后，如果还有新鲜橘子返回 -1，否则返回分钟数

---

## 代码实现

### Java - BFS 多源

```java
class Solution {
    public int orangesRotting(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        Queue<int[]> queue = new LinkedList<>();
        int fresh = 0;  // 新鲜橘子数量

        // 将所有腐烂橘子入队，统计新鲜橘子
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 2) {
                    queue.offer(new int[]{i, j});
                } else if (grid[i][j] == 1) {
                    fresh++;
                }
            }
        }

        // 如果没有新鲜橘子，直接返回 0
        if (fresh == 0) return 0;

        // 如果没有腐烂橘子但有新鲜橘子，返回 -1
        if (queue.isEmpty()) return -1;

        int minutes = 0;
        int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

        // BFS
        while (!queue.isEmpty()) {
            int size = queue.size();

            // 处理当前层的所有腐烂橘子
            for (int i = 0; i < size; i++) {
                int[] curr = queue.poll();
                int row = curr[0], col = curr[1];

                // 尝试腐烂 4 个方向的橘子
                for (int[] dir : dirs) {
                    int newRow = row + dir[0];
                    int newCol = col + dir[1];

                    // 边界检查 + 是否是新鲜橘子
                    if (newRow >= 0 && newRow < m && newCol >= 0 && newCol < n
                        && grid[newRow][newCol] == 1) {
                        grid[newRow][newCol] = 2;  // 腐烂
                        fresh--;
                        queue.offer(new int[]{newRow, newCol});
                    }
                }
            }

            // 如果还有新腐烂的橘子，分钟数加 1
            if (!queue.isEmpty()) {
                minutes++;
            }
        }

        return fresh == 0 ? minutes : -1;
    }
}
```

### Python - BFS 多源

```python
from typing import List
from collections import deque

class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        queue = deque()
        fresh = 0

        # 将所有腐烂橘子入队，统计新鲜橘子
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 2:
                    queue.append((i, j))
                elif grid[i][j] == 1:
                    fresh += 1

        if fresh == 0:
            return 0

        if not queue:
            return -1

        minutes = 0
        directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]

        while queue:
            size = len(queue)

            for _ in range(size):
                row, col = queue.popleft()

                for dr, dc in directions:
                    new_row, new_col = row + dr, col + dc

                    if (0 <= new_row < m and 0 <= new_col < n
                        and grid[new_row][new_col] == 1):
                        grid[new_row][new_col] = 2
                        fresh -= 1
                        queue.append((new_row, new_col))

            if queue:
                minutes += 1

        return minutes if fresh == 0 else -1
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 多源 BFS | 所有腐烂橘子同时入队 |
| 层序遍历 | 每分钟对应 BFS 一层 |
| 计数新鲜橘子 | 判断是否全部腐烂 |
| 边界检查 | 确保不越界 |
| 原地修改 | 直接修改 grid，不需要 visited |

---

## 图解

```
grid = [[2,1,1],
        [1,1,0],
        [0,1,1]]

初始状态 (minute = 0):
2 1 1
1 1 0
0 1 1

queue = [(0,0)], fresh = 5

---

Minute 1 (第一层):
处理 (0,0)，腐烂 (0,1) 和 (1,0)

2 2 1
2 1 0
0 1 1

queue = [(0,1), (1,0)], fresh = 3

---

Minute 2 (第二层):
处理 (0,1)，腐烂 (0,2)
处理 (1,0)，腐烂 (1,1)

2 2 2
2 2 0
0 1 1

queue = [(0,2), (1,1)], fresh = 2

---

Minute 3 (第三层):
处理 (0,2)，无新鲜邻居
处理 (1,1)，腐烂 (2,1)

2 2 2
2 2 0
0 2 1

queue = [(2,1)], fresh = 1

---

Minute 4 (第四层):
处理 (2,1)，腐烂 (2,2)

2 2 2
2 2 0
0 2 2

queue = [(2,2)], fresh = 0

---

Minute 5:
队列为空，fresh = 0

返回: 4 分钟

---

BFS 层次示意:
     (0,0) - Minute 0 (初始)
      /   \
  (0,1)  (1,0) - Minute 1
    |      |
  (0,2)  (1,1) - Minute 2
           |
        (2,1) - Minute 3
           |
        (2,2) - Minute 4
```

---

## 变种/延伸

### 变种1：岛屿数量
- **题目**：[lc-200 岛屿数量](./lc-200-number-of-islands.md)
- **变化**：统计连通区域数量
- **解法**：DFS/BFS 遍历

### 变种2：墙壁和门
- **题目**：[lc-286 墙壁和门](./)
- **变化**：从多个门出发填充距离
- **解法**：类似的多源 BFS

### 变种3：地图中的最高点
- **题目**：[lc-1765 地图中的最高点](./)
- **变化**：水域到陆地的最短距离
- **解法**：多源 BFS

### 进阶思考

1. **为什么用 BFS 而不是 DFS？**
   - BFS 按层遍历，天然对应时间
   - 第一层 = 1 分钟后
   - DFS 无法保证按时间顺序处理

2. **为什么多源 BFS 要同时入队？**
   - 确保所有腐烂橘子同时扩散
   - 分批处理会导致分钟数计算错误

3. **如何判断不可能？**
   - BFS 结束后仍有新鲜橘子
   - 说明这些橘子被 0 包围，无法腐烂

4. **如果返回腐烂过程？**
   - 记录每个橘子被腐烂的时间
   - 可以用额外的 2D 数组存储

5. **如何优化空间？**
   - 原地修改 grid，不需要 visited 数组
   - 队列可以用坐标压缩优化

6. **与单源 BFS 的区别？**
   - 单源：从一个起点开始
   - 多源：从多个起点同时开始
   - 多源 BFS 可以看作超级源点连接所有起点
