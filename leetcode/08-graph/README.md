# 图专题

## 专题概述

图是由节点和边组成的数据结构，本专题涵盖：

- **DFS/BFS**：深度优先和广度优先搜索
- **拓扑排序**：有向无环图的应用
- **最短路径**：Dijkstra、Floyd-Warshall 等
- **并查集**：解决连通性问题

### 核心知识点

1. 图的表示：邻接矩阵、邻接表
2. 图的遍历：DFS、BFS
3. 并查集：路径压缩、按秩合并
4. 拓扑排序：Kahn 算法、DFS 算法

---

## 学习路径

1. 先阅读 [templates.md](./templates.md) 熟悉图操作模板
2. 按顺序完成各分类题目：
   - [ ] 01-dfs-bfs（图的遍历）
   - [ ] 02-topological（拓扑排序）
   - [ ] 03-shortest-path（最短路径）
   - [ ] 04-union-find（并查集）

---

## 题目分类

| 分类 | 题目数 | 难度 | 说明 |
|------|-------|------|------|
| 01-dfs-bfs | 3 | ⭐⭐⭐ | 图的遍历框架 |
| 02-topological | 1 | ⭐⭐⭐ | 拓扑排序 |
| 03-shortest-path | 1 | ⭐⭐⭐ | 最短路径 |
| 04-union-find | 2 | ⭐⭐⭐ | 并查集应用 |

---

## 进度追踪

### 01-dfs-bfs
- [ ] lc-200-number-of-islands (Medium)
- [ ] lc-797-all-paths-from-source-to-target (Medium)
- [ ] lc-994-rotting-oranges (Medium)

### 02-topological
- [ ] lc-207-course-schedule (Medium)

### 03-shortest-path
- [ ] lc-743-network-delay-time (Medium)

### 04-union-find
- [ ] lc-200-number-of-islands (Medium)
- [ ] lc-547-number-of-provinces (Medium)

---

## 解题模板总结

### 图的表示（邻接表）
```java
// 无向图
Map<Integer, List<Integer>> graph = new HashMap<>();
for (int[] edge : edges) {
    int u = edge[0], v = edge[1];
    graph.computeIfAbsent(u, k -> new ArrayList<>()).add(v);
    graph.computeIfAbsent(v, k -> new ArrayList<>()).add(u);
}
```

### DFS 模板
```java
public void dfs(int node, Set<Integer> visited) {
    if (visited.contains(node)) return;

    visited.add(node);
    // 处理当前节点

    for (int neighbor : graph.get(node)) {
        dfs(neighbor, visited);
    }
}
```

### BFS 模板
```java
public void bfs(int start) {
    Queue<Integer> queue = new LinkedList<>();
    Set<Integer> visited = new HashSet<>();

    queue.offer(start);
    visited.add(start);

    while (!queue.isEmpty()) {
        int node = queue.poll();
        // 处理当前节点

        for (int neighbor : graph.get(node)) {
            if (!visited.contains(neighbor)) {
                queue.offer(neighbor);
                visited.add(neighbor);
            }
        }
    }
}
```

### 并查集模板
```java
class UnionFind {
    private int[] parent;
    private int[] rank;

    public UnionFind(int n) {
        parent = new int[n];
        rank = new int[n];
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
            // 按秩合并
            if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
            } else if (rank[rootX] > rank[rootY]) {
                parent[rootY] = rootX;
            } else {
                parent[rootY] = rootX;
                rank[rootX]++;
            }
        }
    }
}
```

### 拓扑排序（Kahn 算法）
```java
public List<Integer> topologicalSort(int numCourses, int[][] prerequisites) {
    // 构建图和入度数组
    Map<Integer, List<Integer>> graph = new HashMap<>();
    int[] inDegree = new int[numCourses];

    for (int[] edge : prerequisites) {
        int from = edge[1], to = edge[0];
        graph.computeIfAbsent(from, k -> new ArrayList<>()).add(to);
        inDegree[to]++;
    }

    // 将入度为 0 的节点加入队列
    Queue<Integer> queue = new LinkedList<>();
    for (int i = 0; i < numCourses; i++) {
        if (inDegree[i] == 0) {
            queue.offer(i);
        }
    }

    List<Integer> result = new ArrayList<>();
    while (!queue.isEmpty()) {
        int node = queue.poll();
        result.add(node);

        if (graph.containsKey(node)) {
            for (int neighbor : graph.get(node)) {
                if (--inDegree[neighbor] == 0) {
                    queue.offer(neighbor);
                }
            }
        }
    }

    return result.size() == numCourses ? result : new ArrayList<>();
}
```

---

## 参考资源

- [LeetCode 图标签](https://leetcode.cn/tag/graph/)
- [LeetCode 深度优先搜索标签](https://leetcode.cn/tag/depth-first-search/)
- [LeetCode 广度优先搜索标签](https://leetcode.cn/tag/breadth-first-search/)
