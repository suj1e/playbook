# 207. Course Schedule / 课程表

[中文链接](https://leetcode.cn/problems/course-schedule/) | [English](https://leetcode.com/problems/course-schedule/)

**难度**：Medium
**标签**：深度优先搜索 | 广度优先搜索 | 图 | 拓扑排序
**相似题目**：[lc-210](./) | [lc-802](./) | [lc-630](./)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

你这个学期必须选修 `numCourses` 门课程，记为 `0` 到 `numCourses - 1` 。

在选修某些课程之前需要一些先修课程。 先修课程按数组 `prerequisites` 给出，其中 `prerequisites[i] = [ai, bi]` ，表示如果要学习课程 `ai` 必须 先学习课程 `bi` 。

- 例如，先修课程对 `[0, 1]` 表示：想要学习课程 `0` ，你需要先完成课程 `1` 。

请你判断是否可能完成所有课程的学习？如果可以，返回 `true` ；否则，返回 `false` 。

**示例 1**：
```
输入：numCourses = 2, prerequisites = [[1,0]]
输出：true
解释：总共有 2 门课程。学习课程 1 之前，你需要完成课程 0。这是可能的。
```

**示例 2**：
```
输入：numCourses = 2, prerequisites = [[1,0],[0,1]]
输出：false
解释：总共有 2 门课程。学习课程 1 之前，你需要先完成课程 0；并且学习课程 0 之前，你还应先完成课程 1。这是不可能的。
```

**约束条件**：
- 1 <= numCourses <= 10⁵
- 0 <= prerequisites.length <= 5000
- prerequisites[i].length == 2
- 0 <= ai, bi < numCourses
- ai != bi

---

## 解题思路

### 方法一：拓扑排序（Kahn 算法）- BFS

**思路**：
构建有向图，使用入度表进行拓扑排序。如果存在环，则无法完成课程。

**复杂度**：
- 时间：O(V + E)
- 空间：O(V + E)

**步骤**：
1. 构建图和入度表
2. 入度为 0 的课程加入队列
3. 处理队列，减少依赖课程的入度
4. 如果处理的课程数 == 总课程数，返回 true

---

### 方法二：DFS（检测环）

**思路**：
使用 DFS 检测图中是否有环。

**复杂度**：
- 时间：O(V + E)
- 空间：O(V)

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：BFS 拓扑排序
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        // 构建图和入度表
        List<List<Integer>> graph = new ArrayList<>();
        int[] inDegree = new int[numCourses];

        for (int i = 0; i < numCourses; i++) {
            graph.add(new ArrayList<>());
        }

        for (int[] prereq : prerequisites) {
            int course = prereq[0];
            int prereqCourse = prereq[1];
            graph.get(prereqCourse).add(course);
            inDegree[course]++;
        }

        // 入度为 0 的课程加入队列
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < numCourses; i++) {
            if (inDegree[i] == 0) {
                queue.offer(i);
            }
        }

        // BFS 处理
        int completed = 0;
        while (!queue.isEmpty()) {
            int course = queue.poll();
            completed++;

            for (int next : graph.get(course)) {
                if (--inDegree[next] == 0) {
                    queue.offer(next);
                }
            }
        }

        return completed == numCourses;
    }

    // 方法二：DFS 检测环
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < numCourses; i++) {
            graph.add(new ArrayList<>());
        }

        for (int[] prereq : prerequisites) {
            graph.get(prereq[1]).add(prereq[0]);
        }

        // 0: 未访问, 1: 访问中, 2: 已访问
        int[] visited = new int[numCourses];

        for (int i = 0; i < numCourses; i++) {
            if (hasCycle(graph, i, visited)) {
                return false;
            }
        }

        return true;
    }

    private boolean hasCycle(List<List<Integer>> graph, int node, int[] visited) {
        if (visited[node] == 1) return true;  // 有环
        if (visited[node] == 2) return false; // 已访问过

        visited[node] = 1; // 标记为访问中

        for (int next : graph.get(node)) {
            if (hasCycle(graph, next, visited)) {
                return true;
            }
        }

        visited[node] = 2; // 标记为已访问
        return false;
    }
}
```

### Python

```python
from typing import List
from collections import deque, defaultdict

class Solution:
    # 方法一：BFS 拓扑排序
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        # 构建图和入度表
        graph = defaultdict(list)
        in_degree = [0] * numCourses

        for course, prereq in prerequisites:
            graph[prereq].append(course)
            in_degree[course] += 1

        # 入度为 0 的课程加入队列
        queue = deque([i for i in range(numCourses) if in_degree[i] == 0])

        # BFS 处理
        completed = 0
        while queue:
            course = queue.popleft()
            completed += 1

            for next_course in graph[course]:
                in_degree[next_course] -= 1
                if in_degree[next_course] == 0:
                    queue.append(next_course)

        return completed == numCourses

    # 方法二：DFS 检测环
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        graph = defaultdict(list)
        for course, prereq in prerequisites:
            graph[prereq].append(course)

        # 0: 未访问, 1: 访问中, 2: 已访问
        visited = [0] * numCourses

        def has_cycle(node):
            if visited[node] == 1:
                return True
            if visited[node] == 2:
                return False

            visited[node] = 1

            for next_course in graph[node]:
                if has_cycle(next_course):
                    return True

            visited[node] = 2
            return False

        for i in range(numCourses):
            if has_cycle(i):
                return False

        return True
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 入度表 | 记录每个课程的前置课程数量 |
| 拓扑排序 | 先修课程必须在前 |
| 环检测 | 如果有环，无法完成拓扑排序 |
| 入度为 0 | 可以直接学习的课程 |
| visited 状态 | 0 未访问，1 访问中，2 已访问 |

---

## 图解

```
numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]

有向图:
    0
   / \
  1   2
   \ /
    3

入度: in[0]=0, in[1]=1, in[2]=1, in[3]=2

拓扑排序:
1. 入度为 0 的课程: [0]
   queue = [0], completed = 0

2. 弹出 0, completed = 1
   0 的邻居: 1, 2
   in[1]-- = 0, 入队
   in[2]-- = 0, 入队
   queue = [1, 2]

3. 弹出 1, completed = 2
   1 的邻居: 3
   in[3]-- = 1
   queue = [2]

4. 弹出 2, completed = 3
   2 的邻居: 3
   in[3]-- = 0, 入队
   queue = [3]

5. 弹出 3, completed = 4
   3 的邻居: 无
   queue = []

completed = 4 = numCourses
返回 true

---

有环的情况:
numCourses = 2, prerequisites = [[1,0],[0,1]]

图: 0 <-> 1 (有环)

入度: in[0]=1, in[1]=1

没有入度为 0 的课程
无法开始拓扑排序
返回 false
```

---

## 变种/延伸

### 变种1：课程表 II
- **题目**：[lc-210 课程表 II](./)
- **变化**：返回一种学习顺序
- **解法**：拓扑排序时记录顺序

### 变种2：并行课程
- **题目**：[lc-1136 并行课程](./)
- **变化**：最少学期数完成课程
- **解法**：按层拓扑排序

### 变种3：课程表 III
- **题目**：[lc-802 课程表 III](./)
- **变化**：可以选择不同难度
- **解法：BFS + 状态压缩

### 进阶思考

1. **拓扑排序的应用场景？**
   - 课程安排
   - 任务调度
   - 依赖管理
   - 编译顺序

2. **DFS 如何检测环？**
   - 访问中的节点再次被访问 -> 有环
   - 使用三色标记：白(0)、灰(1)、黑(2)

3. **如果要求返回所有可能的学习顺序？**
   - 每次有多个入度为 0 的节点
   - 需要枚举所有组合

4. **如何找出环的具体路径？**
   - DFS 时记录路径
   - 发现环时返回路径

5. **BFS 和 DFS 哪个更好？**
   - BFS：直观，容易理解
   - DFS：代码简洁，可能更快找到环
