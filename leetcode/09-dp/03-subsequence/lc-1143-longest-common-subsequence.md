# 1143. Longest Common Subsequence / 最长公共子序列

[中文链接](https://leetcode.cn/problems/longest-common-subsequence/) | [English](https://leetcode.com/problems/longest-common-subsequence/)

**难度**：Medium
**标签**：字符串 | 动态规划
**相似题目**：[lc-72](./lc-72-edit-distance.md) | [lc[583](./) | [[712](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长公共子序列的长度。如果不存在公共子序列，返回 0 。

一个字符串的 **子序列** 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

- 例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。

两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。

**示例 1**：
```
输入：text1 = "abcde", text2 = "ace"
输出：3
解释：最长公共子序列是 "ace"，长度为 3。
```

**示例 2**：
```
输入：text1 = "abc", text2 = "abc"
输出：3
解释：最长公共子序列是 "abc"，长度为 3。
```

**示例 3**：
```
输入：text1 = "abc", text2 = "def"
输出：0
解释：两个字符串没有公共子序列，返回 0。
```

**约束条件**：
- 1 <= text1.length, text2.length <= 1000
- text1 和 text2 仅由小写英文字符组成。

---

## 解题思路

### 方法一：动态规划（推荐）

**思路**：
dp[i][j] = text1[0..i-1] 和 text2[0..j-1] 的 LCS 长度

**状态转移**：
- 如果 text1[i-1] == text2[j-1]：dp[i][j] = dp[i-1][j-1] + 1
- 否则：dp[i][j] = max(dp[i-1][j], dp[i][j-1])

**复杂度**：
- 时间：O(m × n)
- 空间：O(m × n)

---

## 代码实现

### Java

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
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

### Python

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
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
| dp 定义 | dp[i][j] = 前缀 LCS 长度 |
| 字符相等 | 来自左上角 +1 |
| 字符不等 | 来自上方或左边的较大值 |
| 边界条件 | dp[0][j] = dp[i][0] = 0 |
| 空间优化 | 可以优化到 O(min(m,n)) |

---

## 图解

```
text1 = "abcde", text2 = "ace"

构建 DP 表:
      ""  a   c   e
""     0   0   0   0
a      0   1   1   1
b      0   1   1   1
c      0   1   2   2
d      0   1   2   2
e      0   1   2   3

填充过程:
i=1, j=1: 'a'=='a', dp[1][1]=dp[0][0]+1=1

i=1, j=2: 'a'!='c', dp[1][2]=max(dp[0][2],dp[1][1])=1
i=1, j=3: 'a'!='e', dp[1][3]=max(dp[0][3],dp[1][2])=1

i=2, j=1: 'b'!='a', dp[2][1]=max(dp[1][1],dp[2][0])=1
i=2, j=2: 'b'!='c', dp[2][2]=max(dp[1][2],dp[2][1])=1
i=2, j=3: 'b'!='e', dp[2][3]=max(dp[1][3],dp[2][2])=1

i=3, j=1: 'c'=='a', dp[3][1]=max(dp[2][1],dp[3][0])=1
i=3, j=2: 'c'=='c', dp[3][2]=dp[2][1]+1=2
i=3, j=3: 'c'!='e', dp[3][3]=max(dp[2][3],dp[3][2])=2

i=4, j=1: 'd'!='a', dp[4][1]=max(dp[3][1],dp[4][0])=1
i=4, j=2: 'd'!='c', dp[4][2]=max(dp[3][2],dp[4][1])=2
i=4, j=3: 'd'!='e', dp[4][3]=max(dp[2][3],dp[4][2])=2

i=5, j=1: 'e'!='a', dp[5][1]=max(dp[4][1],dp[5][0])=1
i=5, j=2: 'e'!='c', dp[5][2]=max(dp[4][2],dp[5][1])=2
i=5, j=3: 'e'=='e', dp[5][3]=dp[4][2]+1=3

结果: dp[5][3] = 3
LCS = "ace"
```

---

## 变种/延伸

### 变种1：编辑距离
- **题目**：[lc-72 编辑距离](./lc-72-edit-distance.md)
- **变化**：求最少的插入/删除/替换操作数
- **解法**：类似 DP，但有三个操作

### 变种2：两个字符串的删除操作
- **题目**：[lc[583 两个字符串的删除操作](./)
- **变化**：删除字符使两字符串相等
- **解法**：LCS 变种

### 变种3：最短公共超序列
- **题目**：[712 最短公共超序列](./)
- **变化**：包含两字符串的最短字符串
- **解法**：反向 DP 重建

### 进阶思考

1. **如何重建 LCS 字符串？**
   - 从 dp[m][n] 开始回溯
   - 字符相等往左上走，不等往较大值方向
   - 递归或迭代重建

2. **空间如何优化？**
   - 只需要上一行数据
   - 用两行或一行 DP
   - space: O(min(m,n))

3. **如果字符串很长怎么办？**
   - 使用空间优化
   - 或者使用 Hirschberg 算法
   - 空间 O(min(m,n))

4. **如何求所有 LCS？**
   - 记录所有路径
   - 回溯时收集所有解
   - 数量可能很大

5. **LCS 和最长公共子串的区别？**
   - 子序列：不要求连续
   - 子串：要求连续
   - 子串用不同的 DP 方法
