# 图代码模板

图是力扣中的高级数据结构，包括图的表示、遍历（DFS/BFS）、最短路径、拓扑排序、并查集等内容。

---

## 1. 图的表示

### 1.1 邻接表（最常用）

#### Java

```java
import java.util.*;

// 方法1：使用 List<数组>
// 适用于带权图，edges[i] = [from, to] 或 [from, to, weight]
class Graph {
    private int n;  // 节点数
    private List<int[]>[] adj;  // 邻接表

    public Graph(int n) {
        this.n = n;
        this.adj = new List[n];
        for (int i = 0; i < n; i++) {
            adj[i] = new ArrayList<>();
        }
    }

    // 添加边（无向图）
    public void addEdge(int from, int to, int weight) {
        adj[from].add(new int[]{to, weight});
        adj[to].add(new int[]{from, weight});  // 无向图需要双向添加
    }

    // 添加边（有向图）
    public void addDirectedEdge(int from, int to, int weight) {
        adj[from].add(new int[]{to, weight});
    }
}

// 方法2：使用 Map 表示（节点编号不连续）
Map<Integer, List<int[]>> graph = new HashMap<>();
for (int[] edge : edges) {
    int from = edge[0], to = edge[1], weight = edge[2];
    graph.computeIfAbsent(from, k -> new ArrayList<>()).add(new int[]{to, weight});
}
```

#### Python

```python
from collections import defaultdict
from typing import List

# 方法1：使用 defaultdict
graph = defaultdict(list)
for edge in edges:
    fr, to, weight = edge
    graph[fr].append((to, weight))
    graph[to].append((fr, weight))  # 无向图需要双向添加

# 方法2：使用列表的列表（节点编号 0 到 n-1）
n = len(nodes)
adj = [[] for _ in range(n)]
for fr, to, weight in edges:
    adj[fr].append((to, weight))
    adj[to].append((fr, weight))  # 无向图
```

---

### 1.2 邻接矩阵

#### Java

```java
// 适用于稠密图或快速判断边是否存在
int[][] graph = new int[n][n];
// graph[i][j] 表示边 i→j 的权重，0 表示没有边

// 添加边
graph[from][to] = weight;

// 判断边是否存在
if (graph[from][to] > 0) {
    // 边存在
}
```

#### Python

```python
# 邻接矩阵
n = len(nodes)
graph = [[0] * n for _ in range(n)]

# 添加边
graph[fr][to] = weight

# 判断边是否存在
if graph[fr][to] > 0:
    # 边存在
```

---

### 1.3 边集数组

#### Java

```java
// 适用于 Kruskal 算法等
int[][] edges = {{from, to, weight}, ...};
```

#### Python

```python
# 边集
edges = [(fr, to, weight), ...]
```

---

## 2. DFS 遍历

### 2.1 递归 DFS

#### Java

```java
class GraphDFS {
    private List<List<Integer>> adj;
    private boolean[] visited;

    public void dfs(int node) {
        visited[node] = true;
        // 处理当前节点
        System.out.println(node);

        for (int neighbor : adj.get(node)) {
            if (!visited[neighbor]) {
                dfs(neighbor);
            }
        }
    }

    // 主函数
    public void traverse(int start, int n) {
        visited = new boolean[n];
        dfs(start);
    }
}
```

#### Python

```python
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()

    visited.add(start)
    print(start)  # 处理当前节点

    for neighbor in graph[start]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
```

---

### 2.2 迭代 DFS（使用栈）

#### Java

```java
public void dfsIterative(int start) {
    boolean[] visited = new boolean[n];
    Deque<Integer> stack = new ArrayDeque<>();

    stack.push(start);
    visited[start] = true;

    while (!stack.isEmpty()) {
        int node = stack.pop();
        // 处理当前节点
        System.out.println(node);

        for (int neighbor : adj.get(node)) {
            if (!visited[neighbor]) {
                stack.push(neighbor);
                visited[neighbor] = true;
            }
        }
    }
}
```

#### Python

```python
def dfs_iterative(graph, start):
    visited = set()
    stack = [start]
    visited.add(start)

    while stack:
        node = stack.pop()
        print(node)  # 处理当前节点

        for neighbor in graph[node]:
            if neighbor not in visited:
                stack.append(neighbor)
                visited.add(neighbor)
```

---

## 3. BFS 遍历

### 3.1 标准 BFS

#### Java

```java
public void bfs(int start) {
    boolean[] visited = new boolean[n];
    Queue<Integer> queue = new LinkedList<>();

    queue.offer(start);
    visited[start] = true;

    while (!queue.isEmpty()) {
        int node = queue.poll();
        // 处理当前节点
        System.out.println(node);

        for (int neighbor : adj.get(node)) {
            if (!visited[neighbor]) {
                queue.offer(neighbor);
                visited[neighbor] = true;
            }
        }
    }
}
```

#### Python

```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)

    while queue:
        node = queue.popleft()
        print(node)  # 处理当前节点

        for neighbor in graph[node]:
            if neighbor not in visited:
                queue.append(neighbor)
                visited.add(neighbor)
```

---

### 3.2 BFS 计算层级/距离

#### Java

```java
public int[] bfsDistance(int start) {
    int[] distance = new int[n];
    Arrays.fill(distance, -1);  // -1 表示未访问

    Queue<Integer> queue = new LinkedList<>();
    queue.offer(start);
    distance[start] = 0;

    while (!queue.isEmpty()) {
        int node = queue.poll();

        for (int neighbor : adj.get(node)) {
            if (distance[neighbor] == -1) {
                distance[neighbor] = distance[node] + 1;
                queue.offer(neighbor);
            }
        }
    }

    return distance;
}
```

#### Python

```python
def bfs_distance(graph, start, n):
    distance = [-1] * n
    queue = deque([start])
    distance[start] = 0

    while queue:
        node = queue.popleft()
        for neighbor in graph[node]:
            if distance[neighbor] == -1:
                distance[neighbor] = distance[node] + 1
                queue.append(neighbor)

    return distance
```

---

## 4. 拓扑排序（Kahn 算法）

**适用场景**：有向无环图（DAG），检测环、课程安排等

### Java

```java
public int[] topologicalSort(int n, int[][] edges) {
    // 1. 构建图和入度数组
    List<List<Integer>> adj = new ArrayList<>();
    int[] inDegree = new int[n];

    for (int i = 0; i < n; i++) {
        adj.add(new ArrayList<>());
    }

    for (int[] edge : edges) {
        int from = edge[0], to = edge[1];
        adj.get(from).add(to);
        inDegree[to]++;
    }

    // 2. 将入度为 0 的节点入队
    Queue<Integer> queue = new LinkedList<>();
    for (int i = 0; i < n; i++) {
        if (inDegree[i] == 0) {
            queue.offer(i);
        }
    }

    // 3. BFS 处理
    int[] result = new int[n];
    int index = 0;

    while (!queue.isEmpty()) {
        int node = queue.poll();
        result[index++] = node;

        for (int neighbor : adj.get(node)) {
            inDegree[neighbor]--;
            if (inDegree[neighbor] == 0) {
                queue.offer(neighbor);
            }
        }
    }

    // 4. 判断是否有环
    if (index != n) {
        return new int[]{};  // 有环
    }

    return result;
}
```

### Python

```python
from collections import deque
from typing import List

def topological_sort(n: int, edges: List[tuple]):
    # 1. 构建图和入度数组
    adj = [[] for _ in range(n)]
    in_degree = [0] * n

    for fr, to in edges:
        adj[fr].append(to)
        in_degree[to] += 1

    # 2. 将入度为 0 的节点入队
    queue = deque([i for i in range(n) if in_degree[i] == 0])

    # 3. BFS 处理
    result = []
    while queue:
        node = queue.popleft()
        result.append(node)

        for neighbor in adj[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    # 4. 判断是否有环
    if len(result) != n:
        return []  # 有环

    return result
```

---

## 5. Dijkstra 最短路径

**适用场景**：带权有向图，边权非负

### Java

```java
public int[] dijkstra(int n, int[][] edges, int start) {
    // 1. 构建图
    List<List<int[]>> adj = new ArrayList<>();
    for (int i = 0; i < n; i++) {
        adj.add(new ArrayList<>());
    }
    for (int[] edge : edges) {
        int from = edge[0], to = edge[1], weight = edge[2];
        adj.get(from).add(new int[]{to, weight});
    }

    // 2. 初始化距离数组
    int[] dist = new int[n];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[start] = 0;

    // 3. 最小堆：[距离, 节点]
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
    pq.offer(new int[]{0, start});

    // 4. Dijkstra
    while (!pq.isEmpty()) {
        int[] curr = pq.poll();
        int d = curr[0], node = curr[1];

        // 跳过过期的节点
        if (d > dist[node]) continue;

        for (int[] neighbor : adj.get(node)) {
            int next = neighbor[0], weight = neighbor[1];
            if (dist[node] + weight < dist[next]) {
                dist[next] = dist[node] + weight;
                pq.offer(new int[]{dist[next], next});
            }
        }
    }

    return dist;
}
```

### Python

```python
import heapq
from typing import List

def dijkstra(n: int, edges: List[tuple], start: int):
    # 1. 构建图
    adj = [[] for _ in range(n)]
    for fr, to, weight in edges:
        adj[fr].append((to, weight))

    # 2. 初始化距离数组
    dist = [float('inf')] * n
    dist[start] = 0

    # 3. 最小堆：(距离, 节点)
    pq = [(0, start)]

    # 4. Dijkstra
    while pq:
        d, node = heapq.heappop(pq)

        # 跳过过期的节点
        if d > dist[node]:
            continue

        for neighbor, weight in adj[node]:
            if dist[node] + weight < dist[neighbor]:
                dist[neighbor] = dist[node] + weight
                heapq.heappush(pq, (dist[neighbor], neighbor))

    return dist
```

---

## 6. 并查集（Union-Find）

### 6.1 基本实现（路径压缩 + 按秩合并）

#### Java

```java
class UnionFind {
    private int[] parent;  // 父节点
    private int[] rank;    // 秩（树的高度）

    public UnionFind(int n) {
        parent = new int[n];
        rank = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            rank[i] = 1;
        }
    }

    // 查找根节点（带路径压缩）
    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);  // 路径压缩
        }
        return parent[x];
    }

    // 合并两个集合
    public boolean union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);

        if (rootX == rootY) return false;  // 已在同一集合

        // 按秩合并
        if (rank[rootX] < rank[rootY]) {
            parent[rootX] = rootY;
        } else if (rank[rootX] > rank[rootY]) {
            parent[rootY] = rootX;
        } else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }

        return true;
    }

    // 判断是否在同一集合
    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }

    // 获取集合数量
    public int getCount() {
        int count = 0;
        for (int i = 0; i < parent.length; i++) {
            if (parent[i] == i) count++;
        }
        return count;
    }
}
```

#### Python

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [1] * n

    def find(self, x):
        # 路径压缩
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)

        if root_x == root_y:
            return False

        # 按秩合并
        if self.rank[root_x] < self.rank[root_y]:
            self.parent[root_x] = root_y
        elif self.rank[root_x] > self.rank[root_y]:
            self.parent[root_y] = root_x
        else:
            self.parent[root_y] = root_x
            self.rank[root_x] += 1

        return True

    def connected(self, x, y):
        return self.find(x) == self.find(y)

    def get_count(self):
        return sum(1 for i in range(len(self.parent)) if self.parent[i] == i)
```

---

### 6.2 并查集应用：岛屿数量

#### Java

```java
public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) return 0;

    int m = grid.length, n = grid[0].length;
    UnionFind uf = new UnionFind(m * n);

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == '1') {
                // 检查右边
                if (j + 1 < n && grid[i][j + 1] == '1') {
                    uf.union(i * n + j, i * n + j + 1);
                }
                // 检查下边
                if (i + 1 < m && grid[i + 1][j] == '1') {
                    uf.union(i * n + j, (i + 1) * n + j);
                }
            }
        }
    }

    // 统计岛屿数量（值为 1 的独立集合数）
    Set<Integer> lands = new HashSet<>();
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == '1') {
                lands.add(uf.find(i * n + j));
            }
        }
    }

    return lands.size();
}
```

#### Python

```python
from typing import List

class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [1] * n

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

def num_islands(grid):
    if not grid or not grid[0]:
        return 0

    m, n = len(grid), len(grid[0])
    uf = UnionFind(m * n)

    for i in range(m):
        for j in range(n):
            if grid[i][j] == '1':
                if j + 1 < n and grid[i][j + 1] == '1':
                    uf.union(i * n + j, i * n + j + 1)
                if i + 1 < m and grid[i + 1][j] == '1':
                    uf.union(i * n + j, (i + 1) * n + j)

    lands = set()
    for i in range(m):
        for j in range(n):
            if grid[i][j] == '1':
                lands.add(uf.find(i * n + j))

    return len(lands)
```

---

## 7. 网格图 DFS/BFS 模板

**适用场景**：岛屿数量、腐烂橘子、扫雷等

### 7.1 网格 DFS

#### Java

```java
// 四个方向：上右下左
int[][] directions = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};

public void dfsGrid(char[][] grid, int i, int j) {
    int m = grid.length, n = grid[0].length;

    // 越界或不符合条件
    if (i < 0 || i >= m || j < 0 || j >= n || grid[i][j] != '1') {
        return;
    }

    // 标记已访问
    grid[i][j] = '0';  // 或使用 visited 数组

    // 向四个方向扩展
    for (int[] dir : directions) {
        int ni = i + dir[0];
        int nj = j + dir[1];
        dfsGrid(grid, ni, nj);
    }
}
```

#### Python

```python
# 四个方向：上右下左
directions = [(-1, 0), (0, 1), (1, 0), (0, -1)]

def dfs_grid(grid, i, j):
    m, n = len(grid), len(grid[0])

    # 越界或不符合条件
    if i < 0 or i >= m or j < 0 or j >= n or grid[i][j] != '1':
        return

    # 标记已访问
    grid[i][j] = '0'  # 或使用 visited 数组

    # 向四个方向扩展
    for di, dj in directions:
        ni, nj = i + di, j + dj
        dfs_grid(grid, ni, nj)
```

---

### 7.2 网格 BFS

#### Java

```java
public int bfsGrid(int[][] grid, int start_i, int start_j) {
    int m = grid.length, n = grid[0].length;
    boolean[][] visited = new boolean[m][n];
    Queue<int[]> queue = new LinkedList<>();

    queue.offer(new int[]{start_i, start_j});
    visited[start_i][start_j] = true;

    int steps = 0;
    int[][] directions = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};

    while (!queue.isEmpty()) {
        int size = queue.size();
        steps++;

        for (int i = 0; i < size; i++) {
            int[] curr = queue.poll();
            int x = curr[0], y = curr[1];

            for (int[] dir : directions) {
                int nx = x + dir[0];
                int ny = y + dir[1];

                if (nx >= 0 && nx < m && ny >= 0 && ny < n &&
                    !visited[nx][ny] && grid[nx][ny] == 1) {
                    visited[nx][ny] = true;
                    queue.offer(new int[]{nx, ny});
                }
            }
        }
    }

    return steps;
}
```

#### Python

```python
from collections import deque
from typing import List

def bfs_grid(grid: List[List[int]], start_i: int, start_j: int) -> int:
    m, n = len(grid), len(grid[0])
    visited = [[False] * n for _ in range(m)]
    queue = deque([(start_i, start_j)])
    visited[start_i][start_j] = True

    steps = 0
    directions = [(-1, 0), (0, 1), (1, 0), (0, -1)]

    while queue:
        steps += 1
        for _ in range(len(queue)):
            x, y = queue.popleft()

            for dx, dy in directions:
                nx, ny = x + dx, y + dy
                if 0 <= nx < m and 0 <= ny < n and not visited[nx][ny] and grid[nx][ny] == 1:
                    visited[nx][ny] = True
                    queue.append((nx, ny))

    return steps
```

---

## 8. 复杂度总结

| 算法 | 时间复杂度 | 空间复杂度 |
|------|-----------|-----------|
| DFS | O(V + E) | O(V) |
| BFS | O(V + E) | O(V) |
| 拓扑排序 | O(V + E) | O(V) |
| Dijkstra（堆优化） | O((V + E) log V) | O(V) |
| 并查集操作 | O(α(n)) ≈ O(1) | O(V) |

**说明**：
- V 是顶点数，E 是边数
- α(n) 是反阿克曼函数，增长极慢，可视为常数

---

## 9. 常见应用场景

| 场景 | 算法 | 典型题目 |
|------|------|---------|
| 连通性判断 | DFS/BFS/并查集 | lc-200, lc-547 |
| 环检测 | 拓扑排序/DFS | lc-207, lc-210 |
| 最短路径（无权） | BFS | lc-1091 |
| 最短路径（带权） | Dijkstra | lc-743 |
| 最小生成树 | Kruskal/Prim | lc-1135 |
| 课程安排 | 拓扑排序 | lc-207, lc-210 |
| 网格遍历 | DFS/BFS | lc-200, lc-994 |

---

## 10. 常见陷阱

| 陷阱 | 说明 | 解决方案 |
|------|------|----------|
| 有向图 vs 无向图 | 无向图需要双向添加边 | 构图时注意 |
| 自环和重边 | 可能影响某些算法 | 判断时注意边界 |
| 图的节点编号 | 可能不连续或从 1 开始 | 使用 Map 或调整编号 |
| 访问标记 | 忘记标记导致死循环 | 访问前立即标记 |
| 并查集初始化 | 注意节点数量和编号 | 确认 0-index 还是 1-index |
| Dijkstra 负权边 | 不支持负权边 | 使用 Bellman-Ford |
| 拓扑排序有环 | 返回空或特殊值 | 检查结果数量是否等于节点数 |
