# 1. Two Sum / 两数之和

[中文链接](https://leetcode.cn/problems/two-sum/) | [English](https://leetcode.com/problems/two-sum/)

**难度**：Easy
**标签**：数组 | 哈希表
**相似题目**：[lc-167](../../../01-array/01-two-pointers/lc-167-two-sum-ii.md) | [lc-15](../../../01-array/04-others/lc-15-3sum.md)
**面试频次**：⭐⭐⭐⭐⭐ (必考题)

---

## 题目描述

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出和为目标值 `target` 的那两个整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

**示例 1**：
```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2**：
```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3**：
```
输入：nums = [3,3], target = 6
输出：[0,1]
```

**约束条件**：
- 2 <= nums.length <= 10⁴
- -10⁹ <= nums[i] <= 10⁹
- -10⁹ <= target <= 10⁹
- 只会存在一个有效答案

**进阶**：你可以想出一个时间复杂度小于 O(n²) 的算法吗？

---

## 解题思路

### 方法一：暴力枚举

**思路**：
双重循环枚举所有可能的两数组合，找到和为 target 的组合。

**复杂度**：
- 时间：O(n²)
- 空间：O(1)

**步骤**：
1. 外层循环 i 从 0 到 n-2
2. 内层循环 j 从 i+1 到 n-1
3. 如果 nums[i] + nums[j] == target，返回 [i, j]

---

### 方法二：哈希表（推荐）

**思路**：
用哈希表存储每个数的索引，遍历时检查 target - nums[i] 是否在表中。

**复杂度**：
- 时间：O(n)
- 空间：O(n)

**步骤**：
1. 创建哈希表 map
2. 遍历数组，对于每个 nums[i]：
   - 检查 target - nums[i] 是否在 map 中
   - 如果存在，返回 [map[target-nums[i]], i]
   - 否则，将 nums[i]: i 存入 map
3. 无解返回空数组

**关键点**：
- **先查后存**：必须先检查补数，再存入当前数（避免自己匹配自己）

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：暴力枚举
    public int[] twoSum(int[] nums, int target) {
        int n = nums.length;
        for (int i = 0; i < n - 1; i++) {           // 外层循环
            for (int j = i + 1; j < n; j++) {        // 内层循环，从 i+1 开始
                if (nums[i] + nums[j] == target) {   // 找到目标
                    return new int[]{i, j};
                }
            }
        }
        return new int[]{};  // 无解
    }

    // 方法二：哈希表（推荐）
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();  // 数值 -> 索引

        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];         // 目标补数

            if (map.containsKey(complement)) {        // 检查补数是否存在
                return new int[]{map.get(complement), i};
            }

            map.put(nums[i], i);                      // 先查后存！
        }

        return new int[]{};  // 题目保证必有解，这里兜底
    }
}
```

### Python

```python
from typing import List

class Solution:
    # 方法一：暴力枚举
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        n = len(nums)
        for i in range(n - 1):                # 外层循环
            for j in range(i + 1, n):         # 内层循环
                if nums[i] + nums[j] == target:
                    return [i, j]
        return []  # 无解

    # 方法二：哈希表（推荐）
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        num_map = {}  # 数值 -> 索引

        for i, num in enumerate(nums):
            complement = target - num          # 目标补数

            if complement in num_map:          # 检查补数是否存在
                return [num_map[complement], i]

            num_map[num] = i                  # 先查后存！

        return []  # 题目保证必有解
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 互补数关系 | 两数之和为 target，则一数是另一数的补数 |
| 先查后存 | 哈希表法必须先检查补数，再存入当前数（避免自己匹配自己） |
| 边界条件 | 题目保证必有解，但代码仍应处理无解情况 |
| 哈希表键值 | 键存数值，值存索引 |
| 时间优化 | 哈希表将时间从 O(n²) 降到 O(n)，空间换时间 |

---

## 变种/延伸

### 变种1：数组已排序
- **题目**：[lc-167 两数之和 II - 输入有序数组](../../../01-array/01-two-pointers/lc-167-two-sum-ii.md)
- **变化**：数组升序排列
- **解法**：双指针（头尾向中间），不需要额外空间

### 变种2：返回所有组合
- **题目**：[lc-15 三数之和](../../../01-array/04-others/lc-15-3sum.md)
- **变化**：找出所有不重复的三元组，和为0
- **解法**：排序 + 双指针 + 去重

### 变种3：返回元素值而非索引
- **变化**：只需要返回那两个数的值，不需要索引
- **解法**：可以用 HashSet，不需要 HashMap

### 进阶思考

1. **如果有多个解怎么办？**
   - 当前方法返回第一个找到的解
   - 可以继续遍历找到所有解存入列表

2. **如果数组中有重复元素怎么办？**
   - 如 [3, 3] 这种情况，先存第一个 3，第二个 3 找到它的补数（第一个 3）
   - "先查后存"保证正确处理重复元素

3. **如果数组非常大（10⁹ 级别），无法全部加载到内存怎么办？**
   - 可以使用外部排序 + 归并策略
   - 或者使用布隆过滤器等概率数据结构

4. **如果要找出三个数的和等于 target？**
   - 三重循环 O(n³)
   - 优化：固定一个数 + 哈希表 O(n²)
   - 更优：排序 + 双指针 O(n²)
