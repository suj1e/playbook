# 10-backtracking / 回溯专题

## 专题概述

回溯是一种通过穷举所有可能情况来寻找问题解的算法，本专题涵盖：

- **组合**：从 n 个元素中选 k 个的组合
- **排列**：n 个元素的全排列
- **子集**：所有可能的子集
- **棋盘问题**：N 皇后、数独等

### 核心知识点

1. 回溯的本质：深度优先搜索 + 状态恢复
2. 回溯三要素：路径、选择列表、结束条件
3. 剪枝优化：提前终止不可能的分支
4. 回溯 vs DFS：回溯强调状态恢复

---

## 学习路径

1. 先阅读 [templates.md](./templates.md) 熟悉回溯框架
2. 按顺序完成各分类题目：
   - [x] 01-combination（组合问题）
   - [x] 02-permutation（排列问题）
   - [x] 03-subset（子集问题）
   - [x] 04-sudoku（棋盘问题）

---

## 题目分类

| 分类 | 题目数 | 难度 | 说明 |
|------|-------|------|------|
| 01-combination | 1 | ⭐⭐⭐ | 组合问题 |
| 02-permutation | 2 | ⭐⭐⭐⭐ | 全排列 |
| 03-subset | 1 | ⭐⭐⭐ | 子集枚举 |
| 04-sudoku | 1 | ⭐⭐⭐⭐ | N 皇后 |

---

## 进度追踪

### 01-combination
- [x] lc-77-combinations (Medium)

### 02-permutation
- [x] lc-46-permutations (Medium)
- [x] lc-47-permutations-ii (Medium)

### 03-subset
- [x] lc-78-subsets (Medium)

### 04-sudoku
- [x] lc-51-n-queens (Hard)

---

## 解题模板总结

### 回溯算法框架

```java
void backtrack(参数) {
    if (满足结束条件) {
        result.add(路径);
        return;
    }

    for (选择 : 所有选择) {
        做选择;
        backtrack(下一层);
        撤销选择;
    }
}
```

### 组合问题（C(n,k)）

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

    for (int i = start; i <= n; i++) {
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

private void backtrack(List<List<Integer>> result, List<Integer>> path,
                      int[] nums, boolean[] used) {
    if (path.size() == nums.length) {
        result.add(new ArrayList<>(path));
        return;
    }

    for (int i = 0; i < nums.length; i++) {
        if (used[i]) continue;

        path.add(nums[i]);
        used[i] = true;
        backtrack(result, path, nums, used);
        path.remove(path.size() - 1);
        used[i] = false;
    }
}
```

---

## 参考资源

- [LeetCode 回溯标签](https://leetcode.cn/tag/backtracking/)
