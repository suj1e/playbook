# 70. Climbing Stairs / 爬楼梯

[中文链接](https://leetcode.cn/problems/climbing-stairs/) | [English](https://leetcode.com/problems/climbing-stairs/)

**难度**：Easy
**标签**：数学 | 动态规划 | 记忆化搜索
**相似题目**：[lc-746](./lc-746-min-cost-climbing-stairs.md) | [lc-377](./) | [lc-198](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**示例 1**：
```
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```

**示例 2**：
```
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```

**约束条件**：
- 1 <= n <= 45

---

## 解题思路

### 方法一：动态规划

**思路**：
dp[i] = 爬到第 i 阶的方法数 = dp[i-1] + dp[i-2]

**复杂度**：
- 时间：O(n)
- 空间：O(n)

**步骤**：
1. 初始化 dp[0] = 1, dp[1] = 1
2. dp[i] = dp[i-1] + dp[i-2]
3. 返回 dp[n]

---

### 方法二：空间优化 DP

**思路**：
只需要前两个状态，不需要整个数组。

**复杂度**：
- 时间：O(n)
- 空间：O(1)

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：DP O(n) 空间
    public int climbStairs(int n) {
        if (n <= 1) return 1;

        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = 1;

        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }

        return dp[n];
    }

    // 方法二：空间优化 O(1) 空间
    public int climbStairs(int n) {
        if (n <= 1) return 1;

        int prev2 = 0, prev1 = 1;

        for (int i = 2; i <= n; i++) {
            int curr = prev1 + prev2;
            prev2 = prev1;
            prev1 = curr;
        }

        return prev1;
    }
}
```

### Python

```python
class Solution:
    # 方法一：DP O(n) 空间
    def climbStairs(self, n: int) -> int:
        if n <= 1:
            return 1

        dp = [0] * (n + 1)
        dp[0] = 1
        dp[1] = 1

        for i in range(2, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]

        return dp[n]

    # 方法二：空间优化 O(1) 空间
    def climbStairs(self, n: int) -> int:
        if n <= 1:
            return 1

        prev2, prev1 = 0, 1

        for _ in range(2, n + 1):
            curr = prev1 + prev2
            prev2, prev1 = prev1, curr

        return prev1
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 状态定义 | dp[i] = 爬到第 i 阶的方法数 |
| 状态转移 | dp[i] = dp[i-1] + dp[i-2] |
| 初始条件 | dp[0] = 1, dp[1] = 1 |
| 空间优化 | 只需保存前两个状态 |
| 类斐波那契 | 本质是斐波那契数列 |

---

## 图解

```
n = 4

dp[i] = 爬到第 i 阶的方法数

dp[0] = 1  (地面)
dp[1] = 1  (只能从 0 爬 1 步)

dp[2] = dp[1] + dp[0] = 1 + 1 = 2
       从第 1 阶爬 1 步
       从第 0 阶爬 2 步

dp[3] = dp[2] + dp[1] = 2 + 1 = 3
       从第 2 阶爬 1 步
       从第 1 阶爬 2 步

dp[4] = dp[3] + dp[2] = 3 + 2 = 5
       从第 3 阶爬 1 步
       从第 2 阶爬 2 步

结果: 5

---

斐波那契数列:
n:     0  1  2  3  4  5  6
fib:   0  1  1  2  3  5  8
dp:    1  1  2  3  5  8  13

dp[i] = fib(i+1)
```

---

## 变种/延伸

### 变种1：最小花费爬楼梯
- **题目**：[lc-746 使用最小花费爬楼梯](./lc-746-min-cost-climbing-stairs.md)
- **变化**：每层有花费，求最小花费
- **解法**：DP，dp[i] = min(dp[i-1]+cost[i-1], dp[i-2]+cost[i-2])

### 变种2：爬楼梯的最小变化
- **题目**：[lc-377 组合总和 Ⅳ](./)
- **变化**：可以爬 1~m 步，求排列数
- **解法**：DP，dp[i] = sum(dp[i-j] for j in 1..m)

### 变种3：打家劫舍
- **题目**：[lc-198 打家劫舍](./)
- **变化**：不能偷相邻的房子
- **解法**：类似 DP，dp[i] = max(dp[i-1], dp[i-2]+nums[i])

### 进阶思考

1. **为什么是斐波那契数列？**
   - 每次只能爬 1 或 2 步
   - 到第 i 阶只能从 i-1 或 i-2 来
   - 所以 dp[i] = dp[i-1] + dp[i-2]

2. **如果可以爬 1、2、3 步呢？**
   - dp[i] = dp[i-1] + dp[i-2] + dp[i-3]
   - 类似 tribonacci 数列

3. **如何用矩阵快速幂优化？**
   - [fib(n+2), fib(n+1)] = [[1,1],[1,0]]^n × [fib(2), fib(1)]
   - 可以在 O(log n) 时间计算

4. **如何计算所有可能的爬法？**
   - 回溯枚举所有路径
   - 时间复杂度 O(2^n)

5. **大数情况如何处理？**
   - 结果可能超过 int 范围
   - 使用 long 或 BigInteger
