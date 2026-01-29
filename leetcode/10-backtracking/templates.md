# 回溯算法代码模板

回溯本质是 DFS + 状态重置，用于穷举所有可能。

---

## 1. 回溯框架

### 1.1 通用模板

```
回溯树结构:
                    start
                  /  |  \
                 opt1 opt2 opt3
                /  \
             opt1-1 opt1-2
             /      \
          result   backtrack

过程: 选择 -> 递归 -> 撤销选择
```

#### Java
```java
public void backtrack(参数) {
    // 终止条件
    if (满足条件) {
        收集结果;
        return;
    }

    for (选择 : 所有选择) {
        做选择;  // 处理节点

        backtrack(下一层);  // 递归

        撤销选择;  // 回溯
    }
}
```

#### Python
```python
def backtrack(参数):
    # 终止条件
    if 满足条件:
        收集结果
        return

    for 选择 in 所有选择:
        做选择  # 处理节点

        backtrack(下一层)  # 递归

        撤销选择  # 回溯
```

---

## 2. 组合问题

### 2.1 标准组合 (C(n,k))

**适用场景**：从 n 个数中选 k 个

```
n=4, k=2, 选择 [1,2,3,4]

回溯树:
              []
         /    |    \
       [1]   [2]   [3]  [4]
      /  \    |      |
   [1,2][1,3][1,4][2,3][2,4][3,4]

结果: [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
```

#### Java
```java
public List<List<Integer>> combine(int n, int k) {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();

    backtrack(n, k, 1, path, result);
    return result;
}

private void backtrack(int n, int k, int startIndex,
                       LinkedList<Integer> path, List<List<Integer>> result) {
    if (path.size() == k) {
        result.add(new ArrayList<>(path));
        return;
    }

    for (int i = startIndex; i <= n; i++) {
        path.add(i);
        backtrack(n, k, i + 1, path, result);
        path.removeLast();
    }
}
```

#### Python
```python
def combine(n, k):
    result = []
    path = []

    def backtrack(start_index):
        if len(path) == k:
            result.append(path.copy())
            return

        for i in range(start_index, n + 1):
            path.append(i)
            backtrack(i + 1)
            path.pop()

    backtrack(1)
    return result
```

---

## 3. 排列问题

### 3.1 全排列

**适用场景**：生成所有排列

```
nums = [1,2,3]

回溯树:
              []
         /    |    \
       [1]   [2]   [3]
      /  \    |     \
   [1,2][1,3][2,1][2,3][3,1][3,2]
      |      |    |
   [1,2,3][1,3,2]...

结果: [[1,2,3],[1,3,2],[2,1,3],...]
```

#### Java
```java
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    boolean[] used = new boolean[nums.length];
    LinkedList<Integer> path = new LinkedList<>();

    backtrack(nums, used, path, result);
    return result;
}

private void backtrack(int[] nums, boolean[] used,
                       LinkedList<Integer> path, List<List<Integer>> result) {
    if (path.size() == nums.length) {
        result.add(new ArrayList<>(path));
        return;
    }

    for (int i = 0; i < nums.length; i++) {
        if (used[i]) continue;

        path.add(nums[i]);
        used[i] = true;
        backtrack(nums, used, path, result);
        path.removeLast();
        used[i] = false;
    }
}
```

#### Python
```python
def permute(nums):
    result = []
    path = []
    used = [False] * len(nums)

    def backtrack():
        if len(path) == len(nums):
            result.append(path.copy())
            return

        for i in range(len(nums)):
            if used[i]:
                continue

            path.append(nums[i])
            used[i] = True
            backtrack()
            path.pop()
            used[i] = False

    backtrack()
    return result
```

---

## 4. 子集问题

### 4.1 不含重复元素的子集

**适用场景**：生成所有子集

```
nums = [1,2,3]

回溯树（每个节点都是子集）:
              []           <- 层0
         /    |    \
      [1]   [2]   [3]     <- 层1
      /  \    |
   [1,2][1,3][2,3]         <- 层2
     |
   [1,2,3]                  <- 层3

结果: [[],[1],[2],[3],[1,2],[1,3],[2,3],[1,2,3]]
```

#### Java
```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    LinkedList<Integer> path = new LinkedList<>();

    backtrack(nums, 0, path, result);
    return result;
}

private void backtrack(int[] nums, int startIndex,
                       LinkedList<Integer> path, List<List<Integer>> result) {
    result.add(new ArrayList<>(path));  // 每个节点都是子集

    for (int i = startIndex; i < nums.length; i++) {
        path.add(nums[i]);
        backtrack(nums, i + 1, path, result);
        path.removeLast();
    }
}
```

#### Python
```python
def subsets(nums):
    result = []
    path = []

    def backtrack(start_index):
        result.append(path.copy())  # 每个节点都是子集

        for i in range(start_index, len(nums)):
            path.append(nums[i])
            backtrack(i + 1)
            path.pop()

    backtrack(0)
    return result
```

---

## 5. N 皇后

### 5.1 经典回溯

**适用场景**：N 皇后问题

```
4x4 棋盘:
. Q . .     Q 在 (0,1)
. . . Q     Q 在 (1,3)
Q . . .     Q 在 (2,0)
. . Q .     Q 在 (3,2)

约束: 每行、每列、每条对角线只能有一个 Q

回溯: 逐行放置，检查位置是否有效
```

#### Java
```java
public List<List<String>> solveNQueens(int n) {
    List<List<String>> result = new ArrayList<>();
    char[][] board = new char[n][n];

    for (char[] row : board) {
        Arrays.fill(row, '.');
    }

    backtrack(board, n, 0, result);
    return result;
}

private void backtrack(char[][] board, int n, int row, List<List<String>> result) {
    if (row == n) {
        result.add(construct(board));
        return;
    }

    for (int col = 0; col < n; col++) {
        if (isValid(board, row, col, n)) {
            board[row][col] = 'Q';
            backtrack(board, n, row + 1, result);
            board[row][col] = '.';  // 回溯
        }
    }
}

private boolean isValid(char[][] board, int row, int col, int n) {
    // 检查列
    for (int i = 0; i < row; i++) {
        if (board[i][col] == 'Q') return false;
    }

    // 检查 45° 对角线
    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
        if (board[i][j] == 'Q') return false;
    }

    // 检查 135° 对角线
    for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
        if (board[i][j] == 'Q') return false;
    }

    return true;
}
```

#### Python
```python
def solveNQueens(n):
    result = []
    board = [['.' for _ in range(n)] for _ in range(n)]

    def backtrack(row):
        if row == n:
            result.append([''.join(row) for row in board])
            return

        for col in range(n):
            if is_valid(board, row, col, n):
                board[row][col] = 'Q'
                backtrack(row + 1)
                board[row][col] = '.'  # 回溯

    def is_valid(board, row, col, n):
        # 检查列
        for i in range(row):
            if board[i][col] == 'Q':
                return False

        # 检查 45° 对角线
        for i, j in zip(range(row - 1, -1, -1), range(col - 1, -1, -1)):
            if board[i][j] == 'Q':
                return False

        # 检查 135° 对角线
        for i, j in zip(range(row - 1, -1, -1), range(col + 1, n)):
            if board[i][j] == 'Q':
                return False

        return True

    backtrack(0)
    return result
```

---

## 6. 复杂度总结

| 问题类型 | 时间复杂度 | 空间复杂度 |
|---------|-----------|-----------|
| 组合 C(n,k) | O(C(n,k) × k) | O(k) |
| 排列 | O(n! × n) | O(n) |
| 子集 | O(n × 2^n) | O(n) |
| N 皇后 | O(n!) | O(n) |
| 分割回文串 | O(n × 2^n) | O(n) |
