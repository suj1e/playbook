# 图代码模板

图是力扣中的高级数据结构，包括 DFS/BFS、拓扑排序、并查集、最短路径等。

---

## 1. 图的表示

### 1.1 邻接表

```
图: 1 -> 2 -> 3
    |    |
    v    v
    4 <- 5

邻接表表示:
1: [2, 4]
2: [3, 5]
3: []
4: []
5: [4]
```

#### Java
```java
// List<数组> 表示
List<int[]>[] adj = new ArrayList[List<int[]>>();

// 添加边
adj[from].add(new int[]{to, weight});

// Map 表示
Map<Integer, List<int[]>> graph = new HashMap<>();
for (int[] edge : edges) {
    int from = edge[0], to = edge[1];
    graph.computeIfAbsent(from, k -> new ArrayList<>()).add(new int[]{to, weight});
}
```

#### Python
```python
from collections import defaultdict

# defaultdict 表示
graph = defaultdict(list)
for fr, to, weight in edges:
    graph[fr].append((to, weight))
```

---

## 2. DFS 遍历

### 2.1 递归 DFS

**适用场景**：路径查找、连通性判断

```
图遍历示例:
    1
   /|\
  2 3 4
  |
  5

DFS 过程:
dfs(1) -> visit 1
  -> dfs(2) -> visit 2
    -> dfs(5) -> visit 5, 返回
  -> dfs(3) -> visit 3, 返回
  -> dfs(4) -> visit 4, 返回

访问顺序: 1 -> 2 -> 5 -> 3 -> 4
```

#### Java
```java
public void dfs(int node, boolean[] visited, List<List<Integer>> adj) {
    visited[node] = true;
    // 处理当前节点

    for (int neighbor : adj.get(node)) {
        if (!visited[neighbor]) {
            dfs(neighbor, visited, adj);
        }
    }
}
```

#### Python
```python
def dfs(node, visited, adj):
    visited[node] = True
    # 处理当前节点

    for neighbor in adj[node]:
        if not visited[neighbor]:
            dfs(neighbor, visited, adj)
```

---

## 3. BFS 遍历

### 3.1 标准 BFS

**适用场景**：最短路径、层序遍历

```
图遍历示例:
    1
   / \
  2   3
 / \
4   5

BFS 过程:
queue: [1]        -> 层1: [1]
      -> [2,3]     -> 层2: [2,3]
      -> [4,5]     -> 层3: [4,5]

访问顺序: 1 -> 2 -> 3 -> 4 -> 5
```

#### Java
```java
public void bfs(int start, int n, List<List<Integer>> adj) {
    boolean[] visited = new boolean[n];
    Queue<Integer> queue = new LinkedList<>();

    queue.offer(start);
    visited[start] = true;

    while (!queue.isEmpty()) {
        int node = queue.poll();
        // 处理当前节点

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

def bfs(start, n, adj):
    visited = [False] * n
    queue = deque([start])
    visited[start] = True

    while queue:
        node = queue.popleft()
        # 处理当前节点

        for neighbor in adj[node]:
            if not visited[neighbor]:
                queue.append(neighbor)
                visited[neighbor] = True
```

---

## 4. 并查集

### 4.1 路径压缩 + 按秩合并

**适用场景**：连通性问题、动态连通

```
初始状态: 0  1  2  3  4
          ●  ●  ●  ●  ●  (各自独立)

union(0,1):
parent: [0, 0, 2, 3, 4]  (1 的父节点是 0)
rank:   [1, 0, 0, 0, 0]

union(1,2):
parent: [0, 0, 0, 3, 4]  (2 的父节点是 0)
rank:   [1, 0, 0, 0, 0]

find(3): 返回 3 (根节点)
connected(0,2): find(0)==find(2) -> true
```

#### Java
```java
class UnionFind {
    private int[] parent;
    private int[] rank;

    public UnionFind(int n) {
        parent = new int[n];
        rank = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            rank[i] = 1;
        }
    }

    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);  // 路径压缩
        }
        return parent[x];
    }

    public boolean union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);

        if (rootX == rootY) return false;

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

    public boolean connected(int x, int y) {
        return find(x) == find(y);
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
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # 路径压缩
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
```

---

## 5. 拓扑排序（Kahn 算法）

**适用场景**：课程安排、任务依赖

```
有向图: 0 -> 1 -> 3
       |    |
       v    v
       2 -> 4

入度: in[0]=0, in[1]=1, in[2]=1, in[3]=2, in[4]=2

拓扑排序:
1. 入度为 0 的节点入队: [0]
2. 弹出 0, result=[0], 移除边 0->1,0->2
   in[1]=0, in[2]=0, 入队: [1,2]
3. 弹出 1, result=[0,1], 移除边 1->3,1->4
   in[3]=1, in[4]=1, 入队: [2]
4. 弹出 2, result=[0,1,2], 移除边 2->4
   in[3]=1, in[4]=0, 入队: [4]
5. 弹出 4, result=[0,1,2,4], 移除边 4->3
   in[3]=0, 入队: [3]
6. 弹出 3, result=[0,1,2,4,3]

结果: [0,1,2,4,3]
```

#### Java
```java
public int[] topologicalSort(int n, int[][] edges) {
    List<List<Integer>> adj = new ArrayList<>();
    int[] inDegree = new int[n];

    for (int i = 0; i < n; i++) {
        adj.add(new ArrayList<>());
    }

    for (int[] edge : edges) {
        adj.get(edge[0]).add(edge[1]);
        inDegree[edge[1]]++;
    }

    Queue<Integer> queue = new LinkedList<>();
    for (int i = 0; i < n; i++) {
        if (inDegree[i] == 0) {
            queue.offer(i);
        }
    }

    int[] result = new int[n];
    int index = 0;

    while (!queue.isEmpty()) {
        int node = queue.poll();
        result[index++] = node;

        for (int neighbor : adj.get(node)) {
            if (--inDegree[neighbor] == 0) {
                queue.offer(neighbor);
            }
        }
    }

    return index == n ? result : new int[]{};  // 有环返回空
}
```

#### Python
```python
from collections import deque

def topological_sort(n, edges):
    adj = [[] for _ in range(n)]
    in_degree = [0] * n

    for fr, to in edges:
        adj[fr].append(to)
        in_degree[to] += 1

    queue = deque([i for i in range(n) if in_degree[i] == 0])
    result = []

    while queue:
        node = queue.popleft()
        result.append(node)

        for neighbor in adj[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    return result if len(result) == n else []  # 有环返回空
```

---

## 6. 复杂度总结

| 算法 | 时间复杂度 | 空间复杂度 |
|------|-----------|-----------|
| DFS | O(V + E) | O(V) |
| BFS | O(V + E) | O(V) |
| 并查集操作 | O(α(n)) ≈ O(1) | O(V) |
| 拓扑排序 | O(V + E) | O(V) |
| Dijkstra | O((V + E) log V) | O(V) |

**说明**：V 是顶点数，E 是边数，α(n) 是反阿克曼函数
