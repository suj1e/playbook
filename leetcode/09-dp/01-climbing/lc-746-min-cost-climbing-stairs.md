# 746. Min Cost Climbing Stairs / 使用最小花费爬楼梯

[中文链接](https://leetcode.cn/problems/min-cost-climbing-stairs/) | [English](https://leetcode.com/problems/min-cost-climbing-stairs/)

**难度**：Easy
**标签**：数组 | 动态规划
**相似题目**：[lc-70](./lc-70-climbing-stairs.md) | [lc-198](./)
**面试频次**：⭐⭐⭐

---

## 题目描述

给你一个整数数组 `cost` ，其中 `cost[i]` 是从楼梯第 `i` 个台阶向上爬需要支付的费用。一旦你支付此费用，即可选择向上爬一个或者两个台阶。

你可以选择从下标为 0 或下标为 1 的台阶开始爬楼梯。

请你计算并返回达到楼梯顶部的最低花费。

**示例 1**：
```
输入：cost = [10,15,20]
输出：15
解释：你将从下标为 1 的台阶开始，支付 15 ，然后向上爬两个台阶，到达楼梯顶部。
总花费为 15 。
```

**示例 2**：
```
输入：cost = [1,100,1,1,1,100,1,1,100,1]
输出：6
解释：你将从下标为 0 的台阶开始。
- 支付 1 ，向上爬两个台阶，到达下标为 2 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 4 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 6 的台阶。
- 支付 1 ，向上爬一个台阶，到达下标为 7 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 9 的台阶。
- 支付 1 ，向上爬一个台阶，到达楼梯顶部。
总花费为 6 。
```

**约束条件**：
- 2 <= cost.length <= 1000
- 0 <= cost[i] <= 999

---

## 解题思路

### 方法一：动态规划（推荐）

**思路**：
dp[i] 表示到达第 i 个台阶的最低花费。

**状态转移**：
- dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])
- 可以从 i-1 跨一步到达 i，花费 dp[i-1] + cost[i-1]
- 可以从 i-2 跨两步到达 i，花费 dp[i-2] + cost[i-2]

**复杂度**：
- 时间：O(n)
- 空间：O(n) → 可优化到 O(1)

**步骤**：
1. 初始化 dp[0] = dp[1] = 0（可以从 0 或 1 开始，花费为 0）
2. 从 i=2 开始遍历到 n
3. dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])
4. 返回 dp[n]

---

## 代码实现

### Java - 基础版

```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int n = cost.length;
        int[] dp = new int[n + 1];

        // 可以从第 0 或第 1 个台阶开始，初始花费为 0
        dp[0] = 0;
        dp[1] = 0;

        for (int i = 2; i <= n; i++) {
            dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
        }

        return dp[n];
    }
}
```

### Java - 空间优化版

```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int n = cost.length;

        // prev2 = dp[i-2], prev1 = dp[i-1]
        int prev2 = 0, prev1 = 0;

        for (int i = 2; i <= n; i++) {
            int curr = Math.min(prev1 + cost[i - 1], prev2 + cost[i - 2]);
            prev2 = prev1;
            prev1 = curr;
        }

        return prev1;
    }
}
```

### Python - 基础版

```python
from typing import List

class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        n = len(cost)
        dp = [0] * (n + 1)

        for i in range(2, n + 1):
            dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2])

        return dp[n]
```

### Python - 空间优化版

```python
from typing import List

class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        prev2 = prev1 = 0

        for i in range(2, len(cost) + 1):
            curr = min(prev1 + cost[i - 1], prev2 + cost[i - 2])
            prev2, prev1 = prev1, curr

        return prev1
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| dp 含义 | dp[i] = 到达第 i 个台阶的最低花费 |
| 初始条件 | dp[0] = dp[1] = 0 |
| 状态转移 | min(从 i-1 来, 从 i-2 来) |
| 顶部位置 | 第 n 个位置（超出数组边界） |
| 空间优化 | 只需保留前两个状态 |

---

## 图解

```
cost = [10, 15, 20]

位置:   0    1    2    3(顶部)
cost:  10   15   20
dp:    0    0    ?    ?

计算 dp[2]:
dp[2] = min(dp[1] + cost[1], dp[0] + cost[0])
     = min(0 + 15, 0 + 10)
     = min(15, 10)
     = 10

计算 dp[3]（到达顶部）:
dp[3] = min(dp[2] + cost[2], dp[1] + cost[1])
     = min(10 + 20, 0 + 15)
     = min(30, 15)
     = 15

最优路径:
从位置 1 开始（花费 0）
→ 跨两步到位置 3（顶部）
总花费 = 15

---

cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]

最优路径（花费 6）:
0 → 2 → 4 → 6 → 7 → 9 → 10(顶部)
1    1    1    1    1    1   = 6

图示:
[1] [100] [1] [1] [1] [100] [1] [1] [100] [1]  [顶部]
 ↓                    ↓     ↓  ↓     ↓
 2步                 2步   1步 2步   1步
```

---

## 变种/延伸

### 变种1：爬楼梯
- **题目**：[lc-70 爬楼梯](./lc-70-climbing-stairs.md)
- **变化**：没有花费，只求方法数
- **解法**：dp[i] = dp[i-1] + dp[i-2]

### 变种2：打家劫舍
- **题目**：[lc-198 打家劫舍](./)
- **变化**：不能偷相邻的房子
- **解法**：类似的 DP，状态转移略有不同

### 变种3：可以爬更多步
- **变化**：每次可以爬 1、2 或 3 步
- **解法**：dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2], dp[i-3] + cost[i-3])

### 进阶思考

1. **为什么 dp[0] = dp[1] = 0？**
   - 可以选择从第 0 或第 1 个台阶开始
   - 如果从第 0 个开始，到达第 0 个的花费是 0
   - 如果从第 1 个开始，到达第 1 个的花费是 0

2. **顶部位置为什么是 n？**
   - 题目说"到达楼梯顶部"
   - 顶部是最后一个台阶再往上一格
   - 所以是位置 n，而不是 n-1

3. **如果限制只能从 0 开始？**
   - dp[0] = 0, dp[1] = cost[0]
   - 其余不变

4. **如果每次可以爬 1-3 步？**
   - dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2], dp[i-3] + cost[i-3])
   - 需要处理 i=2, i=3 的边界情况

5. **如何恢复具体路径？**
   - 记录每个位置是从哪里来的
   - 用 parent 数组或重建路径

6. **与爬楼梯问题的区别？**
   - 爬楼梯：求方法数（累加）
   - 本题：求最小花费（取 min）
