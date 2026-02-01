# 494. Target Sum / 目标和

[中文链接](https://leetcode.cn/problems/target-sum/) | [English](https://leetcode.com/problems/target-sum/)

**难度**：Medium
**标签**：数组 | 动态规划 | 回溯
**相似题目**：[lc-39](./) | [lcl-377](./) | [lc-416](./lc-416-partition-equal-subset-sum.md)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给你一个整数数组 `nums` 和一个整数 `target` 。

向数组中的每个整数前添加 '+' 或 '-' ，然后串联起所有整数，可以构造一个 **表达式** ：

- 例如，nums = [2, 1] ，可以在 2 之前添加 '+' ，在 1 之前添加 '-' ，然后串联起来得到 "+2-1" 。

返回可以通过上述方法构造的、运算结果等于 `target` 的不同 **表达式** 的数目。

**示例 1**：
```
输入：nums = [1,1,1,1,1], target = 3
输出：5
解释：一共有 5 种方法让目标和为 3。
-1 + 1 + 1 + 1 - 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3 (不同的序列算一种)
+1 + 1 + 1 + 1 - 1 = 3
-1 + 1 + 1 + 1 + 1 = 3
```

**示例 2**：
```
输入：nums = [1], target = 1
输出：1
```

**约束条件**：
- 1 <= nums.length <= 20
- 0 <= nums[i] <= 1000
- 0 <= target <= 1000

---

## 解题思路

### 方法一：动态规划（推荐）

**思路**：
类似 0-1 背包，每个数可以用 + 或 -。

**核心转换**：
- 选取一些数作为 +（和为 sum）
- 剩余的数作为 -
- 需要满足：sum - (total - sum) = target
- 即：sum = (target + total) / 2

**复杂度**：
- 时间：O(n × sum)
- 空间：O(sum)

**步骤**：
1. 计算总和 total
2. 如果 (total + target) 是奇数，无法平分，返回 0
3. target = (total + target) / 2
4. dp[j] = 用前 i 个数凑出和为 j 的方法数
5. 状态转移：dp[j] = dp[j] + dp[j - nums[i]]

---

## 代码实现

### Java

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int total = 0;
        for (int num : nums) {
            total += num;
        }

        // 总和必须 >= target 且同奇偶
        if (total < target || (total + target) % 2 != 0) {
            return 0;
        }

        int sum = (total + target) / 2;  // 正数和的目标

        // dp[j] = 凑出和为 j 的方法数
        int[] dp = new int[sum + 1];
        dp[0] = 1;  // 和为 0 有 1 种方法（什么都不选）

        for (int num : nums) {
            // 从后往前遍历，避免重复使用
            for (int j = sum; j >= num; j--) {
                dp[j] += dp[j - num];
            }
        }

        return dp[sum];
    }
}
```

### Python

```python
from typing import List

class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        total = sum(nums)

        # 总和必须 >= target 且同奇偶
        if total < target or (total + target) % 2 != 0:
            return 0

        target_sum = (total + target) // 2

        dp = [0] * (target_sum + 1)
        dp[0] = 1

        for num in nums:
            # 从后往前遍历
            for j in range(target_sum, num - 1, -1):
                dp[j] += dp[j - num]

        return dp[target_sum]
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 奇偶检查 | (total + target) 必须是偶数 |
| 和的转换 | 正数和 = (total + target) / 2 |
| 0-1 背包 | 每个数只能用一次（+ 或 -） |
| 倒序遍历 | 避免重复使用当前数 |
| dp[0] = 1 | 和为 0 有 1 种方法 |

---

## 图解

```
nums = [1, 1, 1, 1, 1], target = 3

total = 5, target = (5 + 3) / 2 = 4

我们需要找出和为 4 的组合数量

dp 初始: [1, 0, 0, 0, 0]

处理 num = 1:
j=4: dp[4] += dp[3] = 0
j=3: dp[3] += dp[2] = 0
j=2: dp[2] += dp[1] = 0
j=1: dp[1] += dp[0] = 1
dp: [1, 1, 0, 0, 0]

处理 num = 1:
j=4: dp[4] += dp[3] = 0
j=3: dp[3] += dp[2] = 0
j=2: dp[2] += dp[1] = 1
j=1: dp[1] += dp[0] = 2
dp: [1, 2, 1, 0, 0]

处理 num = 1:
j=4: dp[4] += dp[3] = 1
j=3: dp[3] += dp[2] = 1
j=2: dp[2] += dp[1] = 3
j=1: dp[1] += dp[0] = 3
dp: [1, 3, 3, 1, 0]

处理 num = 1:
j=4: dp[4] += dp[3] = 4
j=3: dp[3] += dp[2] = 3
j=2: dp[2] += dp[1] = 6
j=1: dp[1] += dp[0] = 4
dp: [1, 4, 6, 4, 0]

处理 num = 1:
j=4: dp[4] += dp[3] = 7
j=3: dp[3] += dp[2] = 9
j=2: dp[2] += dp[1] = 10
j=1: dp[1] += dp[0] = 5
dp: [1, 5, 10, 7, 0]

结果: dp[4] = 5

验证:
和为 4 的组合:
[1,1,1,1] (4个+)
[1,1,1,1] (4个+)
[1,1,1,1] (其他组合)

对应原问题的表达式:
和为 4 的 5 种选法 + 剩余为 -
每种选法对应一种表达式
```

---

## 变种/延伸

### 变种1：分割等和子集
- **题目**：[lc-416 分割等和子集](./lc-416-partition-equal-subset-sum.md)
- **变化**：判断是否可以分成和相等的两个子集
- **解法**：类似的 0-1 背包

### 变种2：组合总和
- **题目**：[lc-39 组合总和](./)
- **变化**：找出所有和为 target 的组合
- **解法**：回溯/DP

### 进阶思考

1. **为什么需要奇偶检查？**
   - 正数和 + 负数和 = total
   - 正数和 - 负数和 = target
   - 相加得：2 × 正数和 = total + target
   - 所以 total + target 必须是偶数

2. **如何恢复具体的表达式？**
   - 使用 parent 数组记录选择
   - 回溯或迭代重建表达式

3. **如果 nums 有重复元素怎么办？**
   - 本题 nums 可能有重复
   - 每个数可以用多次，所以不需要去重
   - 如果需要去重，需要排序 + 跳过重复

4. **如何处理大数情况？**
   - 和可能很大
   - 使用 bitset 优化空间
   - Python 可以用 int 处理（通常够用）

5. **与组合总和的区别？**
   - 本题：每个数必须用，+ 或 -
   - 组合总和：每个数只能用一次，求组合
   - 目的：不同的问题，不同的约束
