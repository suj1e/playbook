# 122. Best Time to Buy and Sell Stock II / 买卖股票的最佳时机 II

[中文链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/) | [English](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

**难度**：Medium
**标签**：贪心 | 数组 | 动态规划
**相似题目**：[lc-121](./lc-121-best-time-to-buy-and-sell-stock.md) | [lc-123](./) | [lc-188](./)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

给你一个整数数组 `prices`，其中 `prices[i]` 是某支给定股票第 `i` 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 **最多** 只能持有 **一股** 股票。你也可以先购买，然后在 **同一天** 出售。

返回你能获得的 **最大** 利润。

**示例 1**：
```
输入：prices = [7,1,5,3,6,4]
输出：7
解释：在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出,
这笔交易所能获得利润 = 5 - 1 = 4 。
随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出,
这笔交易所能获得利润 = 6 - 3 = 3 。
总利润为 4 + 3 = 7 。
```

**示例 2**：
```
输入：prices = [1,2,3,4,5]
输出：4
解释：在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出,
这笔交易所能获得利润 = 5 - 1 = 4 。
总利润为 4 。
注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```

**示例 3**：
```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

**约束条件**：
- 1 <= prices.length <= 3 * 10⁴
- 0 <= prices[i] <= 10⁴

---

## 解题思路

### 方法一：贪心（推荐）

**思路**：
只要今天比昨天贵，就昨天买入、今天卖出，累加所有正收益。

**核心思想**：
- 将所有上升段的收益累加
- 等价于"尽可能多地完成交易"
- 贪心选择：只要有利可图就交易

**复杂度**：
- 时间：O(n)
- 空间：O(1)

**步骤**：
1. 遍历价格数组
2. 如果 prices[i] > prices[i-1]，累加差值
3. 返回总收益

### 方法二：动态规划

**思路**：
dp[i][0] = 第 i 天不持有股票的最大收益
dp[i][1] = 第 i 天持有股票的最大收益

**状态转移**：
- dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
  - 不持有：本来就不持有 或 今天卖出
- dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])
  - 持有：本来就持有 或 今天买入

**复杂度**：
- 时间：O(n)
- 空间：O(n) → 可优化到 O(1)

---

## 代码实现

### Java - 贪心法

```java
class Solution {
    public int maxProfit(int[] prices) {
        int profit = 0;

        for (int i = 1; i < prices.length; i++) {
            // 只要今天比昨天贵，就卖出
            if (prices[i] > prices[i - 1]) {
                profit += prices[i] - prices[i - 1];
            }
        }

        return profit;
    }
}
```

### Java - DP 版本

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[][] dp = new int[n][2];

        // 初始化
        dp[0][0] = 0;          // 第0天不持有
        dp[0][1] = -prices[0]; // 第0天持有（买入）

        for (int i = 1; i < n; i++) {
            // 不持有：本来就不持有 或 今天卖出
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            // 持有：本来就持有 或 今天买入
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
        }

        return dp[n - 1][0];  // 最后不持有股票
    }
}
```

### Java - DP 空间优化

```java
class Solution {
    public int maxProfit(int[] prices) {
        int hold = -prices[0];  // 持有股票的最大收益
        int cash = 0;           // 不持有股票的最大收益

        for (int i = 1; i < prices.length; i++) {
            // 新的 cash：保持不持有 或 卖出
            int newCash = Math.max(cash, hold + prices[i]);
            // 新的 hold：保持持有 或 买入
            int newHold = Math.max(hold, cash - prices[i]);

            cash = newCash;
            hold = newHold;
        }

        return cash;
    }
}
```

### Python - 贪心法

```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        profit = 0

        for i in range(1, len(prices)):
            if prices[i] > prices[i - 1]:
                profit += prices[i] - prices[i - 1]

        return profit
```

### Python - DP 版本

```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        hold, cash = -prices[0], 0

        for i in range(1, len(prices)):
            cash = max(cash, hold + prices[i])
            hold = max(hold, cash - prices[i])

        return cash
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 贪心策略 | 累加所有正收益 |
| 可以多次交易 | 与 lc-121 不同 |
| 同一天可买卖 | 买入后立即卖出收益为 0 |
| DP 状态 | 持有/不持有两种状态 |
| 贪心正确性 | 多个上升段 = 整体上升 |

---

## 图解

```
prices = [7, 1, 5, 3, 6, 4]

贪心法图解:
第1天→第2天: 7→1, 下跌，不交易
第2天→第3天: 1→5, 上涨, 利润 = 5-1 = 4
第3天→第4天: 5→3, 下跌，不交易
第4天→第5天: 3→6, 上涨, 利润 = 6-3 = 3
第5天→第6天: 6→4, 下跌，不交易

总利润 = 4 + 3 = 7

价格图:
    7 | *
      |   *
    6 |           *
      |               *
    5 |       *
      |               *
    4 |                   *
      |               *
    3 |           *
      |   *
    1 |       *   *
      |___________________
        1 2 3 4 5 6

交易机会:
买入@1, 卖出@5: +4
买入@3, 卖出@6: +3

---

prices = [1, 2, 3, 4, 5]

贪心法:
1→2: +1
2→3: +1
3→4: +1
4→5: +1
总计: +4

等价于:
买入@1, 卖出@5: +4

贪心原理:
连续上升段中，分段买卖收益 = 整体买卖收益
(2-1) + (3-2) + (4-3) + (5-4) = 5-1 = 4
```

---

## 变种/延伸

### 变种1：买卖股票的最佳时机
- **题目**：[lc-121 买卖股票的最佳时机](./lc-121-best-time-to-buy-and-sell-stock.md)
- **变化**：只能买卖一次
- **解法**：记录最低价格，计算最大差值

### 变种2：买卖股票的最佳时机 III
- **题目**：[lc-123 买卖股票的最佳时机 III](./)
- **变化**：最多买卖两次
- **解法**：DP，4 个状态

### 变种3：买卖股票的最佳时机 IV
- **题目**：[lc-188 买卖股票的最佳时机 IV](./)
- **变化**：最多买卖 k 次
- **解法**：DP，k 个买入+卖出状态

### 变种4：含手续费
- **题目**：[lc-714 买卖股票的最佳时机含手续费](./)
- **变化**：每次交易有手续费
- **解法**：DP，在卖出时减去手续费

### 变种5：含冻结期
- **题目**：[lc-309 买卖股票的最佳时机含冷冻期](./)
- **变化**：卖出后有一天冻结期
- **解法**：DP，三种状态

### 进阶思考

1. **为什么贪心是正确的？**
   - 连续上升段中，分段收益 = 整体收益
   - 多个上升段可以分别交易
   - 下降段不交易，避免亏损

2. **贪心 vs DP？**
   - 贪心：简洁，适合本题
   - DP：更通用，适合所有股票问题

3. **如果交易有手续费？**
   - 贪心：只有收益 > 手续费才交易
   - DP：在卖出时减去手续费

4. **如果有交易次数限制？**
   - 贪心不再适用
   - 需要用 DP 记录交易次数

5. **如何处理整数溢出？**
   - 最大利润不超过 10⁴ × 3 × 10⁴ = 3 × 10⁸
   - int 范围内，不会溢出

6. **与 lc-121 的本质区别？**
   - lc-121：只能交易一次，找最大差值
   - lc-122：可无限交易，累加正收益
