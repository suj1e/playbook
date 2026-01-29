# 回溯专题

## 专题概述

回溯是一种通过穷举所有可能情况来寻找问题解的算法，本专题涵盖：

- **组合**：从 n 个元素中选 k 个的组合
- **排列**：n 个元素的全排列
- **子集**：所有可能的子集
- **棋盘问题**：N 皇后、数独等

### 核心知识点

1. 回溯的本质：深度优先搜索 + 状态恢复
2. 回溯三要素：路径、选择列表、结束条件
3. 剪枝：提前终止不可能的分支

---

## 学习路径

1. 先阅读 [templates.md](./templates.md) 熟悉回溯框架
2. 按顺序完成各分类题目：
   - [ ] 01-combination（组合问题）
   - [ ] 02-permutation（排列问题）
   - [ ] 03-subset（子集问题）
   - [ ] 04-sudoku（棋盘问题）

---

## 题目分类

| 分类 | 题目数 | 难度 | 说明 |
|------|-------|------|------|
| 01-combination | 1 | ⭐⭐⭐ | 组合问题 |
| 02-permutation | 2 | ⭐⭐⭐ | 全排列 |
| 03-subset | 1 | ⭐⭐⭐ | 子集枚举 |
| 04-sudoku | 1 | ⭐⭐⭐⭐ | N 皇后 |

---

## 进度追踪

### 01-combination
- [ ] lc-77-combinations (Medium)

### 02-permutation
- [ ] lc-46-permutations (Medium)
- [ ] lc-47-permutations-ii (Medium)

### 03-subset
- [ ] lc-78-subsets (Medium)

### 04-sudoku
- [ ] lc-51-n-queens (Hard)

---

## 解题模板总结

### 回溯算法框架

```java
void backtrack(路径, 选择列表) {
    if (满足结束条件) {
        result.add(路径);
        return;
    }

    for (选择 : 选择列表) {
        // 做选择
        路径.add(选择);
        backtrack(路径, 选择列表);
        // 撤销选择
        路径.remove(选择);
    }
}
```

### 组合问题（C(n, k)）

```java
public List<List<Integer>> combine(int n, int k) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(result, new ArrayList<>(), 1, n, k);
    return result;
}

private void backtrack(List<List<Integer>> result, List<Integer> path,
                       int start, int n, int k) {
    if (path.size() == k) {
        result.add(new ArrayList<>(path));
        return;
    }

    // 剪枝：剩余元素不足
    for (int i = start; i <= n - (k - path.size()) + 1; i++) {
        path.add(i);
        backtrack(result, path, i + 1, n, k);
        path.remove(path.size() - 1);
    }
}
```

### 排列问题

```java
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    boolean[] used = new boolean[nums.length];
    backtrack(result, new ArrayList<>(), nums, used);
    return result;
}

private void backtrack(List<List<Integer>> result, List<Integer> path,
                       int[] nums, boolean[] used) {
    if (path.size() == nums.length) {
        result.add(new ArrayList<>(path));
        return;
    }

    for (int i = 0; i < nums.length; i++) {
        if (used[i]) continue;

        used[i] = true;
        path.add(nums[i]);
        backtrack(result, path, nums, used);
        path.remove(path.size() - 1);
        used[i] = false;
    }
}
```

### 子集问题

```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(result, new ArrayList<>(), nums, 0);
    return result;
}

private void backtrack(List<List<Integer>> result, List<Integer> path,
                       int[] nums, int start) {
    result.add(new ArrayList<>(path));

    for (int i = start; i < nums.length; i++) {
        path.add(nums[i]);
        backtrack(result, path, nums, i + 1);
        path.remove(path.size() - 1);
    }
}
```

### N 皇后

```java
public List<List<String>> solveNQueens(int n) {
    List<List<String>> result = new ArrayList<>();
    char[][] board = new char[n][n];
    for (int i = 0; i < n; i++) {
        Arrays.fill(board[i], '.');
    }
    backtrack(result, board, 0);
    return result;
}

private void backtrack(List<List<String>> result, char[][] board, int row) {
    if (row == board.length) {
        result.add(construct(board));
        return;
    }

    for (int col = 0; col < board.length; col++) {
        if (!isValid(board, row, col)) continue;

        board[row][col] = 'Q';
        backtrack(result, board, row + 1);
        board[row][col] = '.';
    }
}

private boolean isValid(char[][] board, int row, int col) {
    // 检查列
    for (int i = 0; i < row; i++) {
        if (board[i][col] == 'Q') return false;
    }

    // 检查左上对角线
    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
        if (board[i][j] == 'Q') return false;
    }

    // 检查右上对角线
    for (int i = row - 1, j = col + 1; i >= 0 && j < board.length; i--, j++) {
        if (board[i][j] == 'Q') return false;
    }

    return true;
}
```

---

## 参考资源

- [LeetCode 回溯标签](https://leetcode.cn/tag/backtracking/)
