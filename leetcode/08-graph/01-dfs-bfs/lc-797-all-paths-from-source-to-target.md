# 797. All Paths From Source to Target / 所有可能的路径

[中文链接](https://leetcode.cn/problems/all-paths-from-source-to-target/) | [English](https://leetcode.com/problems/all-paths-from-source-to-target/)

**难度**：Medium
**标签**：深度优先搜索 | 广度优先搜索 | 图 | 回溯
**相似题目**：[lc-207](../02-topological/lc-207-course-schedule.md) | [lc-79](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给你一个有 `n` 个节点的 **有向无环图（DAG）**，请你找出所有从节点 `0` 到节点 `n-1` 的路径并输出（不要求按特定顺序）。

`graph[i]` 是一个从节点 i 可以访问的所有节点的列表（即从节点 i 到节点 graph[i][j] 存在一条有向边）。

**示例 1**：
```
输入：graph = [[1,2],[3],[3],[]]
输出：[[0,1,3],[0,2,3]]
解释：有两条路径 0 -> 1 -> 3 和 0 -> 2 -> 3
```

**示例 2**：
```
输入：graph = [[4,3,1],[3,2,4],[3],[4],[]]
输出：[[0,4],[0,3,4],[0,1,3,4],[0,1,2,3,4],[0,1,4]]
```

**约束条件**：
- n == graph.length
- 2 <= n <= 15
- 0 <= graph[i].length < n
- graph[i] 不包含 i
- graph[i] 中的所有元素 **互不相同**
- 保证输入为有向无环图（DAG）

---

## 解题思路

### 方法一：DFS + 回溯（推荐）

**思路**：
从节点 0 开始 DFS，每走到一个节点就加入路径，到达 n-1 时记录路径。

**核心思想**：
- 图的 DFS 遍历 + 路径记录
- 回溯：从当前节点返回时，从路径中移除

**复杂度**：
- 时间：O(2ⁿ × n) - 最坏情况下路径数是指数级
- 空间：O(n) - 递归深度 + 路径存储

**步骤**：
1. 从节点 0 开始 DFS
2. 当前节点加入路径
3. 如果到达 n-1，将当前路径加入结果
4. 否则，遍历所有邻居，递归 DFS
5. 回溯：从路径中移除当前节点

### 方法二：BFS

**思路**：
使用 BFS，队列中存储当前路径，到达 n-1 时记录。

**复杂度**：
- 时间：O(2ⁿ × n)
- 空间：O(2ⁿ × n) - 队列存储所有路径

---

## 代码实现

### Java - DFS + 回溯

```java
class Solution {
    private List<List<Integer>> result;
    private int[][] graph;
    private int n;

    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        this.result = new ArrayList<>();
        this.graph = graph;
        this.n = graph.length;

        List<Integer> path = new ArrayList<>();
        path.add(0);  // 从节点 0 开始
        dfs(0, path);

        return result;
    }

    private void dfs(int node, List<Integer> path) {
        if (node == n - 1) {
            // 到达目标节点，记录路径
            result.add(new ArrayList<>(path));
            return;
        }

        // 遍历所有邻居
        for (int next : graph[node]) {
            path.add(next);       // 选择
            dfs(next, path);      // 递归
            path.remove(path.size() - 1);  // 撤销
        }
    }
}
```

### Java - BFS 版本

```java
class Solution {
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        int n = graph.length;
        List<List<Integer>> result = new ArrayList<>();

        // 队列存储当前路径
        Queue<List<Integer>> queue = new LinkedList<>();
        List<Integer> startPath = new ArrayList<>();
        startPath.add(0);
        queue.offer(startPath);

        while (!queue.isEmpty()) {
            List<Integer> path = queue.poll();
            int node = path.get(path.size() - 1);

            if (node == n - 1) {
                result.add(path);
                continue;
            }

            for (int next : graph[node]) {
                List<Integer> newPath = new ArrayList<>(path);
                newPath.add(next);
                queue.offer(newPath);
            }
        }

        return result;
    }
}
```

### Python - DFS + 回溯

```python
from typing import List

class Solution:
    def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
        n = len(graph)
        result = []

        def dfs(node, path):
            if node == n - 1:
                result.append(path[:])
                return

            for next_node in graph[node]:
                path.append(next_node)
                dfs(next_node, path)
                path.pop()

        dfs(0, [0])
        return result
```

### Python - BFS 版本

```python
from typing import List
from collections import deque

class Solution:
    def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
        n = len(graph)
        result = []

        queue = deque([[0]])

        while queue:
            path = queue.popleft()
            node = path[-1]

            if node == n - 1:
                result.append(path)
                continue

            for next_node in graph[node]:
                queue.append(path + [next_node])

        return result
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| DAG 特性 | 有向无环图，不用担心死循环 |
| 回溯 | 路径记录 + 撤销 |
| 深拷贝 | result.add(new ArrayList<>(path)) |
| 起点终点 | 从 0 开始，到 n-1 结束 |
| 路径收集 | 到达目标节点时收集 |

---

## 图解

```
graph = [[1,2],[3],[3],[]]

图结构:
    0
   / \
  1   2
  \   /
    3

邻接表表示:
0 -> [1, 2]
1 -> [3]
2 -> [3]
3 -> []

DFS 过程:

path = [0]
  |
  +-- 访问 1: path = [0, 1]
  |      |
  |      +-- 访问 3: path = [0, 1, 3]
  |             |
  |             +-- 到达目标，记录 [0, 1, 3]
  |             +-- 回溯: path = [0, 1]
  |      |
  |      +-- 回溯: path = [0]
  |
  +-- 访问 2: path = [0, 2]
         |
         +-- 访问 3: path = [0, 2, 3]
                |
                +-- 到达目标，记录 [0, 2, 3]
                +-- 回溯: path = [0, 2]
         |
         +-- 回溯: path = [0]

结果: [[0, 1, 3], [0, 2, 3]]

---

graph = [[4,3,1],[3,2,4],[],4,[3],[]]

图结构:
        0
     /  |  \
    4   3   1
    |   |   | \
    |   |   2  4
    |   |      |
    3   +------+  (从2通过4到3)
    |
   [终点]

所有路径:
0 -> 4 -> 3
0 -> 3 -> 4
0 -> 1 -> 3 -> 4
0 -> 1 -> 2 -> 4 -> 3
0 -> 1 -> 4 -> 3
```

---

## 变种/延伸

### 变种1：课程表
- **题目**：[lc-207 课程表](../02-topological/lc-207-course-schedule.md)
- **变化**：判断是否能完成所有课程（是否有环）
- **解法**：拓扑排序

### 变种2：课程表 II
- **题目**：[lc-210 课程表 II](./)
- **变化**：返回课程的学习顺序
- **解法**：拓扑排序 + 路径记录

### 变种3：单词搜索 II
- **题目**：[lc-212 单词搜索 II](./)
- **变化**：在网格中搜索多个单词
- **解法**：Trie + DFS

### 进阶思考

1. **为什么不用担心死循环？**
   - 题目保证是有向无环图（DAG）
   - 没有环意味着路径不会重复访问节点
   - 如果有环，需要 visited 数组去重

2. **如果有环怎么办？**
   - 添加 visited 数组
   - 在访问节点前检查是否已访问
   - 或者用拓扑排序预处理

3. **如何找到最短路径？**
   - 使用 BFS
   - BFS 第一次到达目标时就是最短路径

4. **如何判断路径是否存在？**
   - 只需要返回 true/false
   - 可以用 DFS/BFS，找到目标就返回

5. **如何处理超大图？**
   - 路径数量可能是指数级的
   - 可能需要限制输出数量
   - 或者使用流式输出

6. **回溯的时序？**
   - 进入节点：加入路径
   - 递归调用
   - 离开节点：从路径移除
   - 这样确保路径始终正确
