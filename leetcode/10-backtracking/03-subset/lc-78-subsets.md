# 78. Subsets / 子集

[中文链接](https://leetcode.cn/problems/subsets/) | [English](https://leetcode.com/problems/subsets/)

**难度**：Medium
**标签**：位运算 | 数组 | 回溯
**相似题目**：[lc-90](./) | [lc-77](../01-combination/lc-77-combinations.md)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

给你一个整数数组 `nums`，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

**示例 1**：
```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**示例 2**：
```
输入：nums = [0]
输出：[[],[0]]
```

**约束条件**：
- 1 <= nums.length <= 10
- -10 <= nums[i] <= 10
- nums 中的所有元素 **互不相同**

---

## 解题思路

### 方法一：回溯（推荐）

**思路**：
每个元素有两种选择：选或不选，递归枚举所有可能。

**核心思想**：
- 回溯三要素：路径、选择列表、结束条件
- 每次递归都做选择或跳过当前元素

**复杂度**：
- 时间：O(2ⁿ) - 每个元素都有选/不选两种可能
- 空间：O(n) - 递归深度

**步骤**：
1. 每层递归处理当前位置的元素
2. 选择：加入当前元素到路径
3. 不选：跳过当前元素
4. 到达末尾时，将当前路径加入结果

### 方法二：迭代

**思路**：
逐个枚举元素，每次将新元素加入已有子集形成新子集。

**复杂度**：
- 时间：O(2ⁿ)
- 空间：O(1) - 不算结果存储

### 方法三：位运算

**思路**：
n 个元素对应 2ⁿ 个子集，每个子集可以用 n 位二进制表示。

**复杂度**：
- 时间：O(n × 2ⁿ)
- 空间：O(1)

---

## 代码实现

### Java - 回溯法

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(result, new ArrayList<>(), nums, 0);
        return result;
    }

    private void backtrack(List<List<Integer>> result, List<Integer> path,
                          int[] nums, int start) {
        // 每个位置都是有效的子集
        result.add(new ArrayList<>(path));

        // 从 start 开始尝试每个元素
        for (int i = start; i < nums.length; i++) {
            path.add(nums[i]);           // 选择
            backtrack(result, path, nums, i + 1);  // 递归
            path.remove(path.size() - 1); // 撤销
        }
    }
}
```

### Java - 选/不选写法

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(result, new ArrayList<>(), nums, 0);
        return result;
    }

    private void backtrack(List<List<Integer>> result, List<Integer> path,
                          int[] nums, int index) {
        if (index == nums.length) {
            result.add(new ArrayList<>(path));
            return;
        }

        // 不选当前元素
        backtrack(result, path, nums, index + 1);

        // 选当前元素
        path.add(nums[index]);
        backtrack(result, path, nums, index + 1);
        path.remove(path.size() - 1);
    }
}
```

### Java - 迭代法

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        result.add(new ArrayList<>());  // 空集

        for (int num : nums) {
            int size = result.size();
            for (int i = 0; i < size; i++) {
                List<Integer> newSubset = new ArrayList<>(result.get(i));
                newSubset.add(num);
                result.add(newSubset);
            }
        }

        return result;
    }
}
```

### Java - 位运算

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        int n = nums.length;
        int total = 1 << n;  // 2^n

        for (int mask = 0; mask < total; mask++) {
            List<Integer> subset = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                if ((mask >> i & 1) == 1) {
                    subset.add(nums[i]);
                }
            }
            result.add(subset);
        }

        return result;
    }
}
```

### Python - 回溯法

```python
from typing import List

class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        result = []

        def backtrack(path, start):
            result.append(path[:])  # 添加当前子集的副本

            for i in range(start, len(nums)):
                path.append(nums[i])      # 选择
                backtrack(path, i + 1)    # 递归
                path.pop()                # 撤销

        backtrack([], 0)
        return result
```

### Python - 迭代法

```python
from typing import List

class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        result = [[]]

        for num in nums:
            result += [curr + [num] for curr in result]

        return result
```

### Python - 位运算

```python
from typing import List

class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        result = []

        for mask in range(1 << n):
            subset = [nums[i] for i in range(n) if (mask >> i) & 1]
            result.append(subset)

        return result
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 回溯框架 | path、选择列表、结束条件 |
| start 参数 | 避免重复使用同一元素 |
| 深拷贝 | result.add(new ArrayList<>(path)) |
| 每层都加结果 | 子集问题在每个节点都收集结果 |
| 2ⁿ 个子集 | n 个元素，每个选/不选 |

---

## 图解

```
nums = [1, 2, 3]

回溯树（枚举写法）:
                    []
              /      |      \
           [1]       [2]      [3]
          /   \       |
      [1,2]  [1,3]  [2,3]
        |
    [1,2,3]

收集结果（每个节点都是一个子集）:
[]              <- 第1层
[1], [2], [3]   <- 第2层
[1,2], [1,3], [2,3] <- 第3层
[1,2,3]         <- 第4层

共 2³ = 8 个子集

---

选/不选写法递归树:
                        []
              (不选1)  /   \  (选1)
                    []     [1]
             (不选2)/ \    / \ (选2)
                 []  [2] [1] [1,2]
            (不选3)/ \ ...\ ... (选3)
              [] [3] [2] [2,3] [1] [1,3] [1,2] [1,2,3]

每个叶子节点是一个子集
```

---

## 变种/延伸

### 变种1：子集 II（有重复元素）
- **题目**：[lc-90 子集 II](./)
- **变化**：数组可能有重复元素
- **解法**：排序 + 跳过重复（if i > start && nums[i] == nums[i-1] continue）

### 变种2：组合
- **题目**：[lc-77 组合](../01-combination/lc-77-combinations.md)
- **变化**：固定大小 k 的子集
- **解法**：加结束条件 path.size() == k

### 变种3：组合总和
- **题目**：[lc-39 组合总和](./)
- **变化**：找出和为 target 的组合
- **解法**：加 target 判断和剪枝

### 进阶思考

1. **为什么是 2ⁿ 个子集？**
   - 每个元素有选/不选两种可能
   - n 个元素 → 2 × 2 × ... × 2 = 2ⁿ

2. **回溯 vs 迭代 vs 位运算？**
   - 回溯：直观，容易扩展
   - 迭代：简洁，但不便剪枝
   - 位运算：技巧性强，适合小规模

3. **如何优化空间？**
   - 使用位运算可以避免递归栈
   - 但时间复杂度不变

4. **如何处理超大输入？**
   - 子集数量是指数级的
   - n > 20 时结果会非常大
   - 可能需要流式输出或限制数量

5. **子集和组合的区别？**
   - 子集：所有可能的组合（0 到 n 个元素）
   - 组合：固定大小 k 的子集
   - 子集是组合的扩展

6. **如何生成字典序的子集？**
   - 迭代法天然按长度递增
   - 如需严格字典序，需要排序后生成
