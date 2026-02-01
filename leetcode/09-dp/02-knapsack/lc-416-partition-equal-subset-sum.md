# 416. Partition Equal Subset Sum / 分割等和子集

[中文链接](https://leetcode.cn/problems/partition-equal-subset-sum/) | [English](https://leetcode.com/problems/partition-equal-subset-sum/)

**难度**：Medium
**标签**：数组 | 动态规划
**相似题目**：[lc-494](./lc-494-target-sum.md) | [lc-698](./) | [lc-1049](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给你一个 **只包含正整数** 的 **非空** 数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

**示例 1**：
```
输入：nums = [1,5,11,5]
输出：true
解释：数组可以分割成 [1, 5, 5] 和 [11]
```

**示例 2**：
```
输入：nums = [1,2,3,5]
输出：false
解释：数组不能分割成两个元素和相等的子集
```

**约束条件**：
- 1 <= nums.length <= 200
- 1 <= nums[i] <= 100

---

## 解题思路

### 方法一：动态规划（0-1 背包）

**思路**：
转化为 0-1 背包问题：是否能从 nums 中选一些数，使和为 total/2。

**复杂度**：
- 时间：O(n × target)
- 空间：O(target)

**步骤**：
1. 计算总和，必须是偶数
2. 目标和 = total / 2
3. dp[j] = 是否能用 nums[0..i] 凑出和 j
4. 状态转移：dp[j] = dp[j] || dp[j - nums[i]]

---

## 代码实现

### Java

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }

        // 总和必须是偶数
        if (sum % 2 != 0) return false;

        int target = sum / 2;
        boolean[] dp = new boolean[target + 1];
        dp[0] = true;  // 和为 0 总是可以实现（不选任何数）

        for (int num : nums) {
            // 从后往前遍历，避免重复使用
            for (int j = target; j >= num; j--) {
                dp[j] = dp[j] || dp[j - num];
            }
        }

        return dp[target];
    }
}
```

### Python

```python
from typing import List

class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        total = sum(nums)

        # 总和必须是偶数
        if total % 2 != 0:
            return False

        target = total // 2
        dp = [False] * (target + 1)
        dp[0] = True

        for num in nums:
            # 从后往前遍历
            for j in range(target, num - 1, -1):
                dp[j] = dp[j] or dp[j - num]

        return dp[target]
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 总和偶数 | 奇数不可能平分 |
| 目标和 | total / 2 |
| 0-1 背包 | 每个数只能用一次 |
| 倒序遍历 | 避免重复使用 |
| dp[0] = true | 和为 0 总是可以实现 |

---

## 图解

```
nums = [1, 5, 11, 5]

total = 22, target = 11

dp 初始: [T, F, F, F, F, F, F, F, F, F, F, F]
索引:     0  1  2  3  4  5  6  7  8  9 10 11

处理 num = 1:
j = 11..1: dp[1] = dp[1] || dp[0] = T
dp: [T, T, F, F, F, F, F, F, F, F, F, F]

处理 num = 5:
j = 11..5: dp[5] = T || dp[0] = T
          dp[6] = F || dp[1] = T
dp: [T, T, F, F, F, T, T, F, F, F, F, F]

处理 num = 11:
j = 11..11: dp[11] = F || dp[0] = T
dp: [T, T, F, F, F, T, T, F, F, F, F, T]

处理 num = 5:
j = 11..5: dp[5] = T || T = T (已存在)
          dp[6] = T || T = T
          dp[10] = F || dp[5] = T
          dp[11] = T || dp[6] = T
dp: [T, T, F, F, F, T, T, F, F, F, T, T]

dp[11] = T, 返回 true

验证: [1,5,5] 和 = 11, [11] 和 = 11 ✓
```

---

## 变种/延伸

### 变种1：目标和
- **题目**：[lc-494 目标和](./lc-494-target-sum.md)
- **变化**：给数字加减号，使和为 target
- **解法**：0-1 背包，偏移量处理负数

### 变种2：最后一块石头的重量 II
- **题目**：[lc-1049 最后一块石头的重量 II](./)
- **变化**：分成两堆，使最小重量差最小
- **解法**：0-1 背包找最接近 total/2 的和

### 变种3：分割数组成最大和
- **题目**：[lc-1043 分割数组成最大和](./)
- **变化**：分成 m 个子数组，使最大和最小
- **解法**：动态规划 + 二分查找

### 进阶思考

1. **为什么是 0-1 背包？**
   - 每个数只能用一次
   - 要么选，要么不选

2. **为什么从后往前遍历？**
   - 避免同一个数被多次使用
   - 正序遍历会重复使用当前数

3. **如果和很大怎么办？**
   - 使用 bitset 优化
   - Python 可以用位运算加速

4. **如何返回具体的分割方案？**
   - 记录选择路径
   - 回溯 dp 数组找出选了哪些数

5. **如何处理负数？**
   - 本题全是正整数
   - 如果有负数，需要偏移量处理
   - dp 数组索引可能为负，需要平移
- **题目**：[lc-494 目标和](./)
