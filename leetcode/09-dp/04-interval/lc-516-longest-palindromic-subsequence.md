# 516. Longest Palindromic Subsequence / 最长回文子序列

[中文链接](https://leetcode.cn/problems/longest-palindromic-subsequence/) | [English](https://leetcode.com/problems/longest-palindromic-subsequence/)

**难度**：Medium
**标签**：字符串 | 动态规划
**相似题目**：[lc-647](./) | [lc-5](../../02-string/01-reverse/lc-5-longest-palindromic-substring.md) | [lc-1143](../03-subsequence/lc-1143-longest-common-subsequence.md)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给你一个字符串 `s` ，找出其中最长的回文子序列，并返回该序列的长度。

子序列定义为：不改变剩余字符顺序的情况下，删除某些字符（也可以不删除）得到的新序列。

**示例 1**：
```
输入：s = "bbbab"
输出：4
解释：一个可能的最长回文子序列为 "bbbb" 。
```

**示例 2**：
```
输入：s = "cbbd"
输出：2
解释：一个可能的最长回文子序列为 "bb" 。
```

**约束条件**：
- 1 <= s.length <= 1000
- s 仅由小写英文字母组成

---

## 解题思路

### 方法一：动态规划（推荐）

**思路**：
dp[i][j] 表示 s[i...j] 的最长回文子序列长度。

**状态转移**：
- 如果 s[i] == s[j]：dp[i][j] = dp[i+1][j-1] + 2
- 如果 s[i] != s[j]：dp[i][j] = max(dp[i+1][j], dp[i][j-1])

**初始化**：
- dp[i][i] = 1（单个字符是回文）
- dp[i][j] = 0 when i > j

**复杂度**：
- 时间：O(n²)
- 空间：O(n²)

**步骤**：
1. 按长度从小到大枚举（从 2 到 n）
2. 对每个起点 i，计算终点 j = i + len - 1
3. 根据两端字符是否相等转移状态
4. 返回 dp[0][n-1]

### 方法二：转换成 LCS

**思路**：
最长回文子序列 = s 和 reverse(s) 的最长公共子序列。

**复杂度**：
- 时间：O(n²)
- 空间：O(n²)

---

## 代码实现

### Java - 区间 DP

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        int[][] dp = new int[n][n];

        // 单个字符是回文
        for (int i = 0; i < n; i++) {
            dp[i][i] = 1;
        }

        // 按长度从小到大枚举
        for (int len = 2; len <= n; len++) {
            for (int i = 0; i <= n - len; i++) {
                int j = i + len - 1;

                if (s.charAt(i) == s.charAt(j)) {
                    // 两端相等，可以加入回文
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                } else {
                    // 两端不等，只能用一边
                    dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }

        return dp[0][n - 1];
    }
}
```

### Java - LCS 转换

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        String reverse = new StringBuilder(s).reverse().toString();
        return longestCommonSubsequence(s, reverse);
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

### Python - 区间 DP

```python
from typing import List

class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        n = len(s)
        dp = [[0] * n for _ in range(n)]

        # 单个字符是回文
        for i in range(n):
            dp[i][i] = 1

        # 按长度从小到大枚举
        for length in range(2, n + 1):
            for i in range(n - length + 1):
                j = i + length - 1

                if s[i] == s[j]:
                    dp[i][j] = dp[i + 1][j - 1] + 2
                else:
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])

        return dp[0][n - 1]
```

### Python - LCS 转换

```python
from typing import List

class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        reverse = s[::-1]
        return self.lcs(s, reverse)

    def lcs(self, text1: str, text2: str) -> int:
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
| dp 定义 | dp[i][j] = s[i...j] 的最长回文子序列长度 |
| 区间 DP | 按长度从小到大枚举 |
| 两端相等 | dp[i][j] = dp[i+1][j-1] + 2 |
| 两端不等 | dp[i][j] = max(dp[i+1][j], dp[i][j-1]) |
| 初始化 | dp[i][i] = 1 |

---

## 图解

```
s = "bbbab"

构建 DP 表（按长度从小到大）:

   0  1  2  3  4
   b  b  a  b  b

初始化（长度=1）:
   0  1  2  3  4
0 [1, 0, 0, 0, 0]  b
1 [0, 1, 0, 0, 0]  b
2 [0, 0, 1, 0, 0]  a
3 [0, 0, 0, 1, 0]  b
4 [0, 0, 0, 0, 1]  b

长度=2:
i=0,j=1: s[0]=b, s[1]=b, 相等
  dp[0][1] = dp[1][0] + 2 = 0 + 2 = 2
i=1,j=2: s[1]=b, s[2]=a, 不相等
  dp[1][2] = max(dp[2][2], dp[1][1]) = max(1, 1) = 1
i=2,j=3: s[2]=a, s[3]=b, 不相等
  dp[2][3] = max(dp[3][3], dp[2][2]) = 1
i=3,j=4: s[3]=b, s[4]=b, 相等
  dp[3][4] = dp[4][3] + 2 = 2

长度=3:
i=0,j=2: s[0]=b, s[2]=a, 不相等
  dp[0][2] = max(dp[1][2], dp[0][1]) = max(1, 2) = 2
i=1,j=3: s[1]=b, s[3]=b, 相等
  dp[1][3] = dp[2][2] + 2 = 1 + 2 = 3
i=2,j=4: s[2]=a, s[4]=b, 不相等
  dp[2][4] = max(dp[3][4], dp[2][3]) = max(2, 1) = 2

长度=4:
i=0,j=3: s[0]=b, s[3]=b, 相等
  dp[0][3] = dp[1][2] + 2 = 1 + 2 = 3
i=1,j=4: s[1]=b, s[4]=b, 相等
  dp[1][4] = dp[2][3] + 2 = 1 + 2 = 3

长度=5:
i=0,j=4: s[0]=b, s[4]=b, 相等
  dp[0][4] = dp[1][3] + 2 = 3 + 2 = 4

最终 DP 表:
   0  1  2  3  4
0 [1, 2, 2, 3, 4]
1 [0, 1, 1, 3, 3]
2 [0, 0, 1, 1, 2]
3 [0, 0, 0, 1, 2]
4 [0, 0, 0, 0, 1]

答案: dp[0][4] = 4 ("bbbb")
```

---

## 变种/延伸

### 变种1：最长回文子串
- **题目**：[lc-5 最长回文子串](../../02-string/01-reverse/lc-5-longest-palindromic-substring.md)
- **变化**：子串需要连续，子序列不需要
- **解法**：中心扩展或 Manacher 算法

### 变种2：回文子串个数
- **题目**：[lc-647 回文子串](./)
- **变化**：统计所有回文子串的个数
- **解法**：中心扩展或 DP

### 变种3：最长公共子序列
- **题目**：[lc-1143 最长公共子序列](../03-subsequence/lc-1143-longest-common-subsequence.md)
- **变化**：两个字符串的 LCS
- **解法**：类似 DP，但更简单

### 进阶思考

1. **为什么要按长度枚举？**
   - dp[i][j] 依赖 dp[i+1][j-1]、dp[i+1][j]、dp[i][j-1]
   - 这些都是更短的区间
   - 按长度从小到大确保依赖已计算

2. **与 LCS 的关系？**
   - 最长回文子序列 = s 和 reverse(s) 的 LCS
   - 但要注意：LCS 可能在原串中是交叉的
   - 对于回文子序列，这个问题不存在

3. **如何恢复具体的子序列？**
   - 记录 parent 数组
   - 回溯 dp 表重建子序列

4. **如何优化空间？**
   - 区间 DP 难以优化空间（依赖多个状态）
   - 可以用滚动数组，但实现复杂

5. **如果输出字典序最小的？**
   - 在 dp[i][j] 相等时选择更小的字符
   - 需要修改状态转移逻辑

6. **与最长回文子串的区别？**
   - 子串：字符必须连续
   - 子序列：字符不需要连续，只保持顺序
   - 本题更灵活，但 DP 更复杂
