# 47. Permutations II / 全排列 II

[中文链接](https://leetcode.cn/problems/permutations-ii/) | [English](https://leetcode.com/problems/permutations-ii/)

**难度**：Medium
**标签**：数组 | 回溯
**相似题目**：[lc-46](./lc-46-permutations.md) | [lc-90](../03-subset/lc-78-subsets.md)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给定一个可包含重复数字的序列 `nums` ，按任意顺序 返回所有不重复的全排列。

**示例 1**：
```
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

**示例 2**：
```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**约束条件**：
- 1 <= nums.length <= 8
- -10 <= nums[i] <= 10

---

## 解题思路

### 方法一：回溯 + 排序（推荐）

**思路**：
先排序，然后在回溯时跳过同一层中重复的元素。

**核心思想**：
- 排序让重复元素相邻
- `if i > 0 && nums[i] == nums[i-1] && !used[i-1]` 跳过重复

**关键判断**：
- 如果 `nums[i] == nums[i-1]` 且 `used[i-1] == false`，说明是同一层的重复选择

**复杂度**：
- 时间：O(n × n!) - 最多 n! 个排列，每个需要 O(n) 复制
- 空间：O(n) - used 数组 + 递归深度

**步骤**：
1. 对 nums 排序
2. 使用 used 数组标记已使用元素
3. 回溯时跳过同一层的重复元素

### 方法二：回溯 + 交换

**思路**：
使用交换法生成排列，用 Set 去重（不推荐，效率低）。

---

## 代码实现

### Java - 回溯 + 排序

```java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);  // 排序让重复元素相邻
        backtrack(result, new ArrayList<>(), nums, new boolean[nums.length]);
        return result;
    }

    private void backtrack(List<List<Integer>> result, List<Integer> path,
                          int[] nums, boolean[] used) {
        if (path.size() == nums.length) {
            result.add(new ArrayList<>(path));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            // 跳过已使用的元素
            if (used[i]) continue;

            // 跳过同一层的重复元素
            // nums[i] == nums[i-1]: 当前元素与前一个相同
            // !used[i-1]: 前一个元素没被使用，说明在同一层
            if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
                continue;
            }

            path.add(nums[i]);
            used[i] = true;
            backtrack(result, path, nums, used);
            path.remove(path.size() - 1);
            used[i] = false;
        }
    }
}
```

### Python - 回溯 + 排序

```python
from typing import List

class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        result = []
        nums.sort()  # 排序
        used = [False] * len(nums)

        def backtrack(path):
            if len(path) == len(nums):
                result.append(path[:])
                return

            for i in range(len(nums)):
                # 跳过已使用的元素
                if used[i]:
                    continue

                # 跳过同一层的重复元素
                if i > 0 and nums[i] == nums[i - 1] and not used[i - 1]:
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
| 先排序 | 让重复元素相邻，便于去重 |
| used 数组 | 标记元素是否已使用 |
| 关键判断 | `i > 0 && nums[i] == nums[i-1] && !used[i-1]` |
| !used[i-1] 含义 | 前一个相同元素没用过，说明在同一层 |
| 时间复杂度 | O(n × n!) |

---

## 图解

```
nums = [1, 1, 2]

排序后: [1, 1, 2]

去重判断详解:
如果 nums[i] == nums[i-1] 且 used[i-1] == false:
  说明 nums[i-1] 在同一层已被考虑过
  当前 i 的选择会与之前重复

回溯树（带剪枝）:
                           []
              1(0)         1(1)×         2(2)
           /     \          跳过          |
        1(1)    2(2)                   1(0)×
         |        |                    跳过
        2(2)    1(1)                1(1)
         |        |                    |
       [1,1,2]  [1,2,1]              2(2)
                                    |
                                  [2,1,1]

说明:
- 1(0), 1(1), 2(2) 表示索引为0,1,2的元素
- × 表示被剪枝跳过
- 当选第一个1时，第二个1可以选（used[i-1]=true，不同层）
- 当跳过第一个1时，第二个1被剪枝（used[i-1]=false，同一层）

去重原理:
同一位置的选择应该是唯一的
如果第一个位置选了1，第二个1就不应该再在第一位置出现
因为这样会产生相同的排列
```

---

## 变种/延伸

### 变种1：全排列
- **题目**：[lc-46 全排列](./lc-46-permutations.md)
- **变化**：数组元素不重复
- **解法**：不需要排序和去重判断

### 变种2：下一个排列
- **题目**：[lc-31 下一个排列](./)
- **变化**：求字典序的下一个排列
- **解法**：两遍扫描 + 交换

### 变种3：第 k 个排列
- **题目**：[lc-60 第 k 个排列](./)
- **变化**：求第 k 小的排列
- **解法**：数学计算 + 阶乘

### 进阶思考

1. **为什么 !used[i-1] 能判断同一层？**
   - used[i-1] == true：前一个元素在递归树的上层被使用
   - used[i-1] == false：前一个元素在同一层被撤销
   - 只有在同一层才需要跳过重复

2. **为什么需要排序？**
   - 让重复元素相邻
   - 保证去重判断的正确性
   - 不排序的话，重复元素可能不相邻

3. **如何用交换法去重？**
   - 用 Set 记录每层交换过的值
   - 如果当前值已交换过，跳过
   - 但效率不如排序+used数组

4. **如何处理大规模输入？**
   - n! 增长极快
   - n > 8 时结果会非常大
   - 可能需要限制输出数量

5. **与子集 II 的区别？**
   - 子集：每个位置选或不选
   - 排列：每个位置填一个数
   - 去重思想相同，但收集结果的时机不同

6. **如何生成字典序的排列？**
   - 方法1：排序后回溯
   - 方法2：先用 lc-46 生成所有，再排序（效率低）
