# 516. 最长回文子序列

[中文](https://leetcode.cn/problems/longest-palindromic-subsequence/) | [English](https://leetcode.com/problems/longest-palindromic-subsequence/)

**难度**：Medium
**标签**：字符串 | 动态规划

---

## 题目描述

给你一个字符串 `s`，找出其中最长的回文子序列，并返回该序列的长度。

子序列定义为：不改变剩余字符顺序的情况下，删除某些字符或者不删除任何字符形成的新序列。

**示例 1**：
```
输入：s = "bbbab"
输出：4
解释：一个可能的最长回文子序列为 "bbbb"。
```

**示例 2**：
```
输入：s = "cbbd"
输出：2
解释：一个可能的最长回文子序列为 "bb"。
```

---

## 解题思路

### 方法一：动态规划（推荐）

**思路**：
- `dp[i][j]` 表示 `s[i..j]` 的最长回文子序列长度
- 如果 `s[i] == s[j]`：`dp[i][j] = dp[i+1][j-1] + 2`
- 如果 `s[i] != s[j]`：`dp[i][j] = max(dp[i+1][j], dp[i][j-1])`

**复杂度**：
- 时间：O(n²)
- 空间：O(n²)

**状态转移图**：
```
           s[i] == s[j]              s[i] != s[j]
    ┌─────────────────┐        ┌──────────────────┐
    │  dp[i][j] =     │        │  dp[i][j] = max( │
    │  dp[i+1][j-1]+2 │        │   dp[i+1][j],    │
    │                 │        │   dp[i][j-1]     │
    │  (两端都能用)    │        │  )               │
    └─────────────────┘        └──────────────────┘
         (去掉两端)                  (去掉左或右)

    i ← ← ← ← j
    ↓       ↓
  i+1     j-1
```

---

### 方法二：反转字符串 + LCS

**思路**：
- 回文子序列的正序和倒序是一样的
- 将 `s` 反转得到 `s'`
- 求 `s` 和 `s'` 的最长公共子序列（LCS）

**复杂度**：
- 时间：O(n²)
- 空间：O(n²)

---

## 代码实现

### 方法一：动态规划

#### Java

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        int[][] dp = new int[n][n];

        // 单个字符的最长回文子序列长度为 1
        for (int i = 0; i < n; i++) {
            dp[i][i] = 1;
        }

        // 枚举长度（从 2 开始）
        for (int len = 2; len <= n; len++) {
            for (int i = 0; i <= n - len; i++) {
                int j = i + len - 1;

                if (s.charAt(i) == s.charAt(j)) {
                    if (len == 2) {
                        dp[i][j] = 2;  // 两个相同字符
                    } else {
                        dp[i][j] = dp[i + 1][j - 1] + 2;
                    }
                } else {
                    dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }

        return dp[0][n - 1];
    }
}
```

#### Python

```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        n = len(s)
        dp = [[0] * n for _ in range(n)]

        # 单个字符的最长回文子序列长度为 1
        for i in range(n):
            dp[i][i] = 1

        # 枚举长度（从 2 开始）
        for length in range(2, n + 1):
            for i in range(n - length + 1):
                j = i + length - 1

                if s[i] == s[j]:
                    if length == 2:
                        dp[i][j] = 2  # 两个相同字符
                    else:
                        dp[i][j] = dp[i + 1][j - 1] + 2
                else:
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])

        return dp[0][n - 1]
```

---

### 方法二：反转字符串 + LCS

#### Java

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        String reversed = new StringBuilder(s).reverse().toString();
        return longestCommonSubsequence(s, reversed);
    }

    private int longestCommonSubsequence(String text1, String text2) {
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
    }
}
```

#### Python

```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        reversed_s = s[::-1]
        return self.longest_common_subsequence(s, reversed_s)

    def longest_common_subsequence(self, text1: str, text2: str) -> int:
        m, n = len(text1), len(text2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]

        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if text1[i - 1] == text2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1] + 1
                else:
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

        return dp[m][n]
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 状态定义 | `dp[i][j]` 表示 `s[i..j]` 的最长回文子序列长度 |
| 转移方程 | 相同则 `+2`，不同则取 max |
| 边界条件 | `dp[i][i] = 1`，单个字符长度为 1 |
| 遍历顺序 | 按长度从小到大枚举 |

---

## 图解

```
s = "bbbab"

构建 dp 表：
    0   1   2   3   4
    b   b   a   b
0 b 1   2   2   3   4
1 b     1   1   3   3
2 a         1   1   1
3 b             1   1
4 a                 1

答案：dp[0][4] = 4 ("bbbb")
```

---

## 方法对比

| 方法 | 时间 | 空间 | 说明 |
|------|------|------|------|
| 直接 DP | O(n²) | O(n²) | **推荐** |
| LCS | O(n²) | O(n²) | 思路巧妙，代码稍多 |

---

## 变种/延伸

- **变种1**：打印最长回文子序列
- **变种2**：最长回文子串（需要连续）
- **变种3**：判断字符串是否可以重新排列成回文串
