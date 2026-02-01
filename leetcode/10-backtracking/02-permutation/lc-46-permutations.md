# 46. Permutations / 全排列

[中文链接](https://leetcode.cn/problems/permutations/) | [English](https://leetcode.com/problems/permutations/)

**难度**：Medium
**标签**：数组 | 回溯 | 回溯算法
**相似题目**：[lc-47](./lc-47-permutations-ii.md) | [lc-77](./lc-77-combinations.md) | [lc-60](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给定一个不含重复数字的数组 `nums` ，返回其 **所有可能的全排列** 。你可以 **按任意顺序** 返回答案。

**示例 1**：
```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**示例 2**：
```
输入：nums = [0,1]
输出：[[0,1],[1,0]]
```

**示例 3**：
```
输入：nums = [1]
输出：[[1]]
```

**约束条件**：
- 1 <= nums.length <= 6
- -10 <= nums[i] <= 10
- nums 中的所有整数 互不相同

---

## 解题思路

### 方法一：回溯（推荐）

**思路**：
每次选择一个未使用的数字加入路径，全部选完后记录结果。

**复杂度**：
- 时间：O(n! × n)
- 空间：O(n)

**步骤**：
1. 用 used 数组记录已使用的数字
2. 遍历 nums，选择未使用的数字
3. 递归处理，全部选完后记录结果
4. 回溯撤销选择

---

## 代码实现

### Java

```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        boolean[] used = new boolean[nums.length];
        backtrack(result, new ArrayList<>(), nums, used);
        return result;
    }

    private void backtrack(List<List<Integer>> result, List<Integer> path,
                          int[] nums, boolean[] used) {
        // 基准情况：所有数字都已使用
        if (path.size() == nums.length) {
            result.add(new ArrayList<>(path));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (used[i]) continue;  // 跳过已使用的数字

            // 做选择
            path.add(nums[i]);
            used[i] = true;

            backtrack(result, path, nums, used);

            // 撤销选择
            path.remove(path.size() - 1);
            used[i] = false;
        }
    }
}
```

### Python

```python
from typing import List

class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        result = []
        used = [False] * len(nums)

        def backtrack(path):
            if len(path) == len(nums):
                result.append(path.copy())
                return

            for i in range(len(nums)):
                if used[i]:
                    continue

                path.append(nums[i])
                used[i] = True

                backtrack(path)

                path.pop()
                used[i] = False

        backtrack([])
        return result
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| used 数组 | 记录哪些数字已被使用 |
| path 复制 | 记录结果时必须复制 path |
| 排列数量 | n 个数字的排列数 = n! |
| 时间复杂度 | n! 个排列，每个需要 O(n) 复制 |
| 选择顺序 | 按索引顺序选择，保证不重复 |

---

## 图解

```
nums = [1, 2, 3]

回溯树:
                []
         /    |    \
       [1]   [2]   [3]
      /  \    / \    / \
    [1,2][1,3]...   ...
     |
  [1,2,3]

详细过程:

path = [], used = [F,F,F]

i=0: 选择 1, path=[1], used=[T,F,F]
    i=1: 选择 2, path=[1,2], used=[T,T,F]
        i=2: 选择 3, path=[1,2,3], used=[T,T,T]
            达到长度 3, 记录 [1,2,3]
        回退: path=[1,2], used=[T,T,F]
    回退: path=[1], used=[T,F,F]
    i=2: 选择 3, path=[1,3], used=[T,F,T]
        i=0: 跳过(已用), i=1: 选择 2, path=[1,3,2], used=[T,T,T]
            达到长度 3, 记录 [1,3,2]
        ...

所有排列:
[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]
```

---

## 变种/延伸

### 变种1：全排列 II
- **题目**：[lc-47 全排列 II](./lc-47-permutations-ii.md)
- **变化**：数组有重复元素
- **解法**：排序 + 跳过重复元素

### 变种2：下一个排列
- **题目**：[lc-31 下一个排列](./)
- **变化**：按字典序的下一个排列
- **解法**：从后往前找第一个可以增大的位置

### 变种3：第 k 个排列
- **题目**：[lc-60 第k个排列](./)
- **变化**：返回第 k 个排列
- **解法**：数学计算，避免生成所有排列

### 进阶思考

1. **为什么时间复杂度是 O(n! × n)？**
   - 共 n! 个排列
   - 每个排列需要 O(n) 时间复制

2. **如何处理重复元素？**
   - 先排序
   - 跳过同一层已使用过的值
   - if (i > 0 && nums[i] == nums[i-1] && !used[i-1]) continue

3. **空间复杂度可以优化吗？**
   - used 数组 O(n)
   - 递归栈 O(n)
   - 总空间 O(n)，无法优化

4. **如何生成字典序的排列？**
   - 按索引顺序选择即可
   - 默认就是字典序

5. **如何求第 k 个排列？**
   - 用数学计算
   - 第 0 个位置有 (n-1)! 种可能
   - k / (n-1)! 确定第 0 个位置的数
   - 类似"数位进制"的思路
- **题目**：[lc-60 第k个排列](./)
