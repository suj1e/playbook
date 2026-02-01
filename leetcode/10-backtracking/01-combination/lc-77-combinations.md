# 77. Combinations / 组合

[中文链接](https://leetcode.cn/problems/combinations/) | [English](https://leetcode.com/problems/combinations/)

**难度**：Medium
**标签**：回溯 | 组合数学
**相似题目**：[lc-46](./lc-46-permutations.md) | [lc-39](./) | [lc-401](./)
**面试频次**：⭐⭐⭐

---

## 题目描述

给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。

你可以按 任何顺序返回答案。

**示例 1**：
```
输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

**示例 2**：
```
输入：n = 1, k = 1
输出：[[1]]
```

**约束条件**：
- 1 <= k <= n <= 20

---

## 解题思路

### 方法一：回溯（推荐）

**思路**：
从 1 开始递归选择数字，选够 k 个就记录结果。

**复杂度**：
- 时间：O(C(n,k) × k)
- 空间：O(k)

**步骤**：
1. 从 start 开始遍历
2. 将当前数加入路径
3. 递归处理下一个数（从 i+1 开始）
4. 路径长度达到 k 时记录结果
5. 回溯撤销选择

---

## 代码实现

### Java

```java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(result, new ArrayList<>(), 1, n, k);
        return result;
    }

    private void backtrack(List<List<Integer>> result, List<Integer> path,
                           int start, int n, int k) {
        // 基准情况：选够 k 个数
        if (path.size() == k) {
            result.add(new ArrayList<>(path));
            return;
        }

        // 剪枝：剩余元素不足
        // 还需要选 (k - path.size()) 个数
        // 从 start 到 n 共有 (n - start + 1) 个数可用
        for (int i = start; i <= n - (k - path.size()) + 1; i++) {
            path.add(i);
            backtrack(result, path, i + 1, n, k);
            path.remove(path.size() - 1);  // 回溯
        }
    }
}
```

### Python

```python
from typing import List

class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        result = []
        self.backtrack(result, [], 1, n, k)
        return result

    def backtrack(self, result, path, start, n, k):
        # 基准情况
        if len(path) == k:
            result.append(path.copy())
            return

        # 从 start 开始选择
        for i in range(start, n + 1):
            path.append(i)
            self.backtrack(result, path, i + 1, n, k)
            path.pop()  # 回溯
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 剪枝优化 | 剩余元素不足时停止 |
| 不重复选择 | 每次从 i+1 开始 |
| path 复制 | 记录结果时要复制 path |
| 剪枝公式 | i <= n - (k - path.size()) + 1 |
| 时间复杂度 | C(n,k) 是组合数 |

---

## 图解

```
n = 4, k = 2

回溯树:
                []
         /      |      \
      [1]       [2]     [3]      [4]
     /  \         |
  [1,2] [1,3]  [2,3]
   |     |        |
[1,4] [1,4]   [2,4]
               |
             [3,4]

结果: [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]

---

剪枝分析:
对于 [1] 分支, path = [1], start = 2
还需要 k - 1 = 1 个数
可用范围: [2, 3, 4]

对于 [1,2], path = [1,2], 达到 k = 2, 记录

对于 [3], path = [3], start = 4
还需要 1 个数, 可用: [4], 继续

对于 [4], path = [4], start = 5
需要 1 个数, 但无可用元素, 不再递归
```

---

## 变种/延伸

### 变种1：组合总和
- **题目**：[lc-39 组合总和](./)
- **变化**：数字可以重复使用
- **解法**：回溯时从 i 开始（而非 i+1）

### 变种2：组合总和 II
- **题目**：[lc-40 组合总和 II](./)
- **变化**：每个数字只能用一次，且有重复
- **解法**：排序 + 跳过重复元素

### 变种3：组合总数
- **题目**：[lc-377 组合总和 Ⅳ](./)
- **变化**：只计算组合数量
- **解法**：动态规划

### 进阶思考

1. **为什么时间复杂度是 O(C(n,k) × k)？**
   - 共 C(n,k) 个组合
   - 每个组合需要 O(k) 时间复制

2. **如何理解剪枝条件？**
   - 还需要选 r = k - path.size() 个数
   - 剩余可用 = n - start + 1 个数
   - 如果剩余可用 < r，剪枝

3. **如何优化空间？**
   - 当前已经是 O(k) 空间
   - k 是路径长度，无法优化

4. **如果要求按字典序输出？**
   - 默认就是从小到大
   - 1,2,3,4 的顺序保证了字典序

5. **如何计算组合数 C(n,k)？**
   - 公式：C(n,k) = n! / (k! × (n-k)!)
   - 递推：C(n,k) = C(n-1,k-1) + C(n-1,k)
   - 动态规划计算
