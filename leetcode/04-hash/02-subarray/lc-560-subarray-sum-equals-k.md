# 560. Subarray Sum Equals K / 和为 K 的子数组

[中文链接](https://leetcode.cn/problems/subarray-sum-equals-k/) | [English](https://leetcode.com/problems/subarray-sum-equals-k/)

**难度**：Medium
**标签**：数组 | 哈希表 | 前缀和
**相似题目**：[lc-1](../01-frequency/lc-1-two-sum.md) | [lc-974](./) | [lc-1248](./)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 该数组中和为 `k` 的连续子数组的个数 。

**示例 1**：
```
输入：nums = [1,1,1], k = 2
输出：2
```

**示例 2**：
```
输入：nums = [1,2,3], k = 3
输出：2
```

**约束条件**：
- 1 <= nums.length <= 2 * 10⁴
- -1000 <= nums[i] <= 1000
- -10⁷ <= k <= 10⁷

---

## 解题思路

### 方法一：暴力枚举

**思路**：
双重循环枚举所有子数组，计算和。

**复杂度**：
- 时间：O(n²)
- 空间：O(1)

**步骤**：
1. 外层循环枚举子数组起点
2. 内层循环枚举子数组终点，同时计算和
3. 和等于 k 时计数加 1

---

### 方法二：前缀和 + 哈希表（推荐）

**思路**：
前缀和 + 哈希表优化。核心思想：**子数组 [i, j] 的和 = prefix[j+1] - prefix[i]**

**关键转换**：
```
求和为 k 的子数组：
prefix[j+1] - prefix[i] = k
等价于：prefix[i] = prefix[j+1] - k

也就是说：对于当前位置 j，需要找之前有多少个前缀和等于 prefix[j+1] - k
```

**复杂度**：
- 时间：O(n)
- 空间：O(n)

**步骤**：
1. 初始化哈希表，存入 {0: 1}（前缀和为 0 出现 1 次）
2. 遍历数组，累加得到当前前缀和
3. 检查 prefixSum - k 是否在哈希表中
4. 累加结果，更新哈希表

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：暴力枚举 O(n²)
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            int sum = 0;
            for (int j = i; j < n; j++) {
                sum += nums[j];
                if (sum == k) {
                    count++;
                }
            }
        }
        return count;
    }

    // 方法二：前缀和 + 哈希表 O(n)
    public int subarraySum(int[] nums, int k) {
        // 哈希表：前缀和 -> 出现次数
        Map<Integer, Integer> prefixCount = new HashMap<>();

        // 前缀和为 0 出现 1 次（初始状态）
        prefixCount.put(0, 1);

        int sum = 0;      // 当前前缀和
        int count = 0;    // 结果

        for (int num : nums) {
            sum += num;  // 当前前缀和

            // 检查是否存在 sum - k
            count += prefixCount.getOrDefault(sum - k, 0);

            // 记录当前前缀和
            prefixCount.put(sum, prefixCount.getOrDefault(sum, 0) + 1);
        }

        return count;
    }
}
```

### Python

```python
from typing import List
from collections import defaultdict

class Solution:
    # 方法一：暴力枚举 O(n²)
    def subarraySum(self, nums: List[int], k: int) -> int:
        count = 0
        n = len(nums)

        for i in range(n):
            total = 0
            for j in range(i, n):
                total += nums[j]
                if total == k:
                    count += 1

        return count

    # 方法二：前缀和 + 哈希表 O(n)
    def subarraySum(self, nums: List[int], k: int) -> int:
        # 哈希表：前缀和 -> 出现次数
        prefix_count = defaultdict(int)

        # 前缀和为 0 出现 1 次（初始状态）
        prefix_count[0] = 1

        total = 0   # 当前前缀和
        count = 0   # 结果

        for num in nums:
            total += num

            # 检查是否存在 total - k
            count += prefix_count[total - k]

            # 记录当前前缀和
            prefix_count[total] += 1

        return count
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 前缀和思想 | prefix[j] - prefix[i] = 子数组 [i+1, j] 的和 |
| 哈希表优化 | 用哈希表存储前缀和出现次数，O(1) 查找 |
| 初始状态 {0:1} | 处理从数组开头到当前位置的和等于 k 的情况 |
| sum - k 查找 | 当前前缀和为 sum，找之前是否存在前缀和为 sum - k |
| 顺序很重要 | 先查询、再存储（避免自己匹配自己） |
| 负数处理 | 方法对正负数都有效，不需要特殊处理 |

---

## 图解

```
数组: [1, 2, 3, 3, 2, 1], k = 6

前缀和数组:
索引:  -1   0   1   2   3   4   5
前缀和:  0   1   3   6   9  11  13

查找和为 6 的子数组:
位置2: sum=6, 6-6=0, prefixCount[0]=1, count=1  (子数组[0,2])
位置3: sum=9, 9-6=3, prefixCount[3]=1, count=2  (子数组[2,3])
位置4: sum=11, 11-6=5, prefixCount[5]=0
位置5: sum=13, 13-6=7, prefixCount[7]=0

验证:
[1,2,3] -> 1+2+3=6 ✓
[3,3] -> 3+3=6 ✓

结果: 2
```

---

## 变种/延伸

### 变种1：和可被 K 整除的子数组
- **题目**：[lc-974 和可被 K 整除的子数组](./)
- **变化**：子数组和对 K 取模为 0
- **解法**：前缀和取模 + 同余定理

### 变种2：和为 K 的最少子数组元素个数
- **变化**：不要求连续，找最少元素
- **解法**：动态规划

### 变种3：最长的和为 K 的子数组
- **题目**：[lc-325 和为 k 的最长子数组长度](./)
- **变化**：找最长的满足条件的子数组
- **解法**：哈希表记录每个前缀和首次出现的位置

### 变种4：和为 K 的子数组数量（含负数）
- 本题已经支持负数，方法同样有效

### 进阶思考

1. **为什么哈希表要初始化 {0: 1}？**
   - 处理从数组开头到当前位置的和等于 k 的情况
   - 例如：数组 [3, ...]，k=3，第一个元素就满足

2. **如果数组元素都是正数，可以优化吗？**
   - 可以用滑动窗口，O(n) 时间，O(1) 空间
   - 但本题有负数，滑动窗口不适用

3. **如何找到所有满足条件的子数组？**
   - 当前方法只统计数量
   - 可以用哈希表存储索引列表，遍历找出所有子数组

4. **如果 k = 0，怎么处理？**
   - 方法依然有效，找前缀和相等的区间
   - 也就是找和为 0 的子数组

5. **如何理解 "先查询再存储"？**
   - 当前位置作为子数组的终点
   - 查询的是"之前"的前缀和
   - 所以必须先查询，再存储当前前缀和
