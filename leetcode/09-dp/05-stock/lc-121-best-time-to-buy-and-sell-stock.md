# 121. Best Time to Buy and Sell Stock / 买卖股票的最佳时机

[中文链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/) | [English](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

**难度**：Easy
**标签**：数组 | 动态规划 | 贪心
**相似题目**：[lc-122](./lc-122-best-time-to-buy-and-sell-stock-ii.md) | [lc-123](./) | [lc[188](./)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 是一支给定股票第 `i` 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一只股票），设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的利润。如果你不能获取任何利润，返回 0 。

**示例 1**：
```
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

**示例 2**：
```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以利润为 0。
```

**约束条件**：
- 1 <= prices.length <= 10⁵
- 0 <= prices[i] <= 10⁴

---

## 解题思路

### 方法一：动态规划

**思路**：
记录到第 i 天时的最低价格，最大利润就是 max(当前价格 - 最低价格)。

**复杂度**：
- 时间：O(n)
- 空间：O(1)

**步骤**：
1. 记录最低价格 minPrice
2. 每天计算卖出的利润 prices[i] - minPrice
3. 更新最大利润和最低价格

---

### 方法二：贪心（推荐）

**思路**：
与 DP 相同，本质就是找最大差值。

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：动态规划/贪心
    public int maxProfit(int[] prices) {
        if (prices.length == 0) return 0;

        int minPrice = prices[0];
        int maxProfit = 0;

        for (int i = 1; i < prices.length; i++) {
            // 更新最大利润
            maxProfit = Math.max(maxProfit, prices[i] - minPrice);
            // 更新最低价格
            minPrice = Math.min(minPrice, prices[i]);
        }

        return maxProfit;
    }
}
```

### Python

```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices:
            return 0

        min_price = prices[0]
        max_profit = 0

        for price in prices[1:]:
            # 更新最大利润
            max_profit = max(max_profit, price - min_price)
            # 更新最低价格
            min_price = min(min_price, price)

        return max_profit
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 只能交易一次 | 找一个最大的上升差值 |
| 最低价格 | 必须在买入之前 |
| 遍历顺序 | 从前向后，记录遇到的最低价 |
| 边界情况 | 价格一直下降，利润为 0 |
| 时间复杂度 | 只需一次遍历，O(n) |

---

## 图解

```
prices = [7, 1, 5, 3, 6, 4]

天:     0  1  2  3  4  5
价格:   7  1  5  3  6  4

遍历过程:
i=0: minPrice=7, maxProfit=max(0, 7-7)=0

i=1: price=1, 1-7=-6 < 0
     maxProfit=0
     minPrice=min(7,1)=1

i=2: price=5, 5-1=4 > 0
     maxProfit=max(0,4)=4
     minPrice=min(1,5)=1

i=3: price=3, 3-1=2 < 4
     maxProfit=4
     minPrice=1

i=4: price=6, 6-1=5 > 4
     maxProfit=max(4,5)=5
     minPrice=1

i=5: price=4, 4-1=3 < 5
     maxProfit=5
     minPrice=1

结果: maxProfit = 5
买入: 第 1 天 (价格=1)
卖出: 第 4 天 (价格=6)
利润: 6 - 1 = 5
```

---

## 变种/延伸

### 变种1：买卖股票的最佳时机 II
- **题目**：[lc-122 买卖股票的最佳时机 II](./lc-122-best-time-to-buy-and-sell-stock-ii.md)
- **变化**：可以多次交易
- **解法**：贪心，累加所有上升差值

### 变种2：买卖股票的最佳时机 III
- **题目**：[lc-123 买卖股票的最佳时机 III](./)
- **变化**：最多完成两笔交易
- **解法**：DP，维护四个状态

### 变种3：买卖股票含冷冻期
- **题目**：[lc[309 买卖股票含冷冻期](./)
- **变化**：卖出后有冷冻期
- **解法**：DP，三个状态

### 进阶思考

1. **为什么贪心有效？**
   - 只需要一次最大差值
   - 最低买入价必须在卖出之前
   - 一次遍历即可找到

2. **如果允许多次交易怎么办？**
   - lc-122：累加所有上升段
   - 每次上涨都可以买卖

3. **如何限制交易次数？**
   - lc-123：最多 k 次交易
   - DP 状态：dp[k][i][0/1] 表示第 i 天第 k 次交易持有/未持有股票

4. **如果有手续费怎么办？**
   - 买卖差价要大于手续费才交易
   - 可以在 profit 计算时减去 fee

5. **如何初始化 minPrice？**
   - 用第一天的价格
   - 或者 Integer.MAX_VALUE
   - 确保 prices[0] - minPrice 不会异常
