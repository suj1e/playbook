# 动态规划专题

## 专题概述

动态规划是解决多阶段决策问题的强大方法，本专题涵盖：

- **线性 DP**：爬楼梯、打家劫舍
- **背包问题**：0-1 背包、完全背包
- **子序列问题**：最长公共子序列、编辑距离
- **区间 DP**：区间相关问题
- **股票问题**：买卖股票的最佳时机

### 核心知识点

1. DP 状态定义
2. 状态转移方程
3. 初始化
4. 遍历顺序

---

## 学习路径

1. 先阅读 [templates.md](./templates.md) 熟悉 DP 思想和模板
2. 按顺序完成各分类题目：
   - [x] 01-climbing（线性 DP 入门）
   - [x] 02-knapsack（背包问题）
   - [x] 03-subsequence（子序列问题）
   - [x] 04-interval（区间 DP）
   - [x] 05-stock（股票问题）

---

## 题目分类

| 分类 | 题目数 | 难度 | 说明 |
|------|-------|------|------|
| 01-climbing | 2 | ⭐⭐ | 线性 DP 入门 |
| 02-knapsack | 2 | ⭐⭐⭐ | 0-1 背包、目标和 |
| 03-subsequence | 3 | ⭐⭐⭐ | LIS、LCS、编辑距离 |
| 04-interval | 1 | ⭐⭐⭐⭐ | 最长回文子序列 |
| 05-stock | 2 | ⭐⭐⭐⭐ | 股票买卖 |

---

## 进度追踪

### 01-climbing
- [x] lc-70-climbing-stairs (Easy)
- [x] lc-746-min-cost-climbing-stairs (Easy)

### 02-knapsack
- [x] lc-416-partition-equal-subset-sum (Medium)
- [x] lc-494-target-sum (Medium)

### 03-subsequence
- [x] lc-300-longest-increasing-subsequence (Medium)
- [x] lc-1143-longest-common-subsequence (Medium)
- [x] lc-72-edit-distance (Medium)

### 04-interval
- [x] lc-516-longest-palindromic-subsequence (Medium)

### 05-stock
- [x] lc-121-best-time-to-buy-and-sell-stock (Medium)
- [x] lc-122-best-time-to-buy-and-sell-stock-ii (Medium)

---

## 解题模板总结

### DP 解题框架

```java
// 1. 定义 dp 数组
// dp[i] 表示 ...

// 2. 初始化
dp[0] = ...;

// 3. 状态转移
for (int i = 1; i < n; i++) {
    dp[i] = ...;
}

// 4. 返回结果
return dp[n - 1];
```

### 线性 DP（一维）

```java
// 示例：爬楼梯
int n = ...
int[] dp = new int[n + 1];
dp[0] = 1;
dp[1] = 1;

for (int i = 2; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
}

return dp[n];
```

### 0-1 背包（二维）

```java
int[] weight = ...;
int[] value = ...;
int W = ...;  // 背包容量
int n = weight.length;

int[][] dp = new int[n + 1][W + 1];

for (int i = 1; i <= n; i++) {
    for (int w = 0; w <= W; w++) {
        if (w < weight[i - 1]) {
            dp[i][w] = dp[i - 1][w];  // 不选
        } else {
            dp[i][w] = Math.max(
                dp[i - 1][w],                              // 不选
                dp[i - 1][w - weight[i - 1]] + value[i - 1]  // 选
            );
        }
    }
}

return dp[n][W];
```

### 0-1 背包（一维空间优化）

```java
int[] weight = ...;
int[] value = ...;
int W = ...;

int[] dp = new int[W + 1];

for (int i = 0; i < weight.length; i++) {
    // 注意：从后往前遍历
    for (int w = W; w >= weight[i]; w--) {
        dp[w] = Math.max(dp[w], dp[w - weight[i]] + value[i]);
    }
}

return dp[W];
```

### 最长公共子序列（LCS）

```java
String text1 = ..., text2 = ...;
int m = text1.length(), n = text2.length();
int[][] dp = new int[m + 1][n + 1];

for (int i = 1; i <= m; i++) {
    for (int j = 1; j <= n; j++) {
        if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
            dp[i][j] = dp[i - 1][j - 1] + 1;
        } else {
            dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
        }
    }
}

return dp[m][n];
```

### 最长递增子序列（LIS）

```java
int[] nums = ...;
int n = nums.length;
int[] dp = new int[n];
int maxLen = 1;

Arrays.fill(dp, 1);  // 每个元素自身构成长度为 1 的子序列

for (int i = 1; i < n; i++) {
    for (int j = 0; j < i; j++) {
        if (nums[j] < nums[i]) {
            dp[i] = Math.max(dp[i], dp[j] + 1);
        }
    }
    maxLen = Math.max(maxLen, dp[i]);
}

return maxLen;
```

### 编辑距离

```java
String word1 = ..., word2 = ...;
int m = word1.length(), n = word2.length();
int[][] dp = new int[m + 1][n + 1];

// 初始化边界
for (int i = 0; i <= m; i++) dp[i][0] = i;
for (int j = 0; j <= n; j++) dp[0][j] = j;

for (int i = 1; i <= m; i++) {
    for (int j = 1; j <= n; j++) {
        if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
            dp[i][j] = dp[i - 1][j - 1];
        } else {
            dp[i][j] = Math.min(
                dp[i - 1][j - 1],    // 替换
                Math.min(
                    dp[i - 1][j],    // 删除
                    dp[i][j - 1]      // 插入
                )
            ) + 1;
        }
    }
}

return dp[m][n];
```

---

## 参考资源

- [LeetCode 动态规划标签](https://leetcode.cn/tag/dynamic-programming/)
