# 72. Edit Distance / 编辑距离

[中文链接](https://leetcode.cn/problems/edit-distance/) | [English](https://leetcode.com/problems/edit-distance/)

**难度**：Hard
**标签**：字符串 | 动态规划
**相似题目**：[lc-161](./) | [lc-583](./) | [lc[712](./)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

给你两个单词 `word1` 和 `word2`，请返回将 `word1` 转换成 `word2` 所使用的最少操作数 。

你可以对一个单词进行如下三种操作：

- **插入** 一个字符
- **删除** 一个字符
- **替换** 一个字符

**示例 1**：
```
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```

**示例 2**：
```
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' �换换为 'c')
exection -> execution (插入 'u')
```

**约束条件**：
- 0 <= word1.length, word2.length <= 500
- word1 和 word2 由小写英文字母组成

---

## 解题思路

### 方法一：动态规划（推荐）

**思路**：
dp[i][j] = word1[0..i-1] 转换为 word2[0..j-1] 的最少操作数

**状态转移**：
- 如果 word1[i-1] == word2[j-1]：dp[i][j] = dp[i-1][j-1]
- 否则：dp[i][j] = min(删除, 插入, 替换) + 1
  - 删除：dp[i-1][j] + 1（删除 word1[i-1]）
  - 插入：dp[i][j-1] + 1（插入 word2[j-1]）
  - 替换：dp[i-1][j-1] + 1（替换 word1[i-1] 为 word2[j-1]）

**复杂度**：
- 时间：O(m × n)
- 空间：O(m × n)

---

## 代码实现

### Java

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        int[][] dp = new int[m + 1][n + 1];

        // 初始化边界
        for (int i = 0; i <= m; i++) {
            dp[i][0] = i;  // 删除 i 个字符
        }
        for (int j = 0; j <= n; j++) {
            dp[0][j] = j;  // 插入 j 个字符
        }

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];  // 无需操作
                } else {
                    dp[i][j] = Math.min(
                        dp[i - 1][j] + 1,      // 删除
                        dp[i][j - 1] + 1,      // 插入
                        dp[i - 1][j - 1] + 1   // 替换
                    );
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
    def minDistance(self, word1: str, word2: str) -> int:
        m, n = len(word1), len(word2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]

        # 初始化边界
        for i in range(m + 1):
            dp[i][0] = i
        for j in range(n + 1):
            dp[0][j] = j

        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if word1[i - 1] == word2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1]
                else:
                    dp[i][j] = min(
                        dp[i - 1][j] + 1,
                        dp[i][j - 1] + 1,
                        dp[i - 1][j - 1] + 1
                    )

        return dp[m][n]
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 三种操作 | 插入、删除、替换 |
| 字符相等 | 无需操作，继承左上角值 |
| 初始化 | dp[i][0] = i（删除），dp[0][j] = j（插入）|
| min 三者 | 取删除、插入、替换的最小值 |
| 边界含义 | 将空串转为某串的操作数 |

---

## 图解

```
word1 = "horse", word2 = "ros"

DP 表初始化:
      ""  r   o   s
""     0   1   2   3
h      1   1   2   3
o      2   2   1   2
r      3   2   2   2
s      4   3   3   2
e      5   4   4   3

填充过程:
i=1, j=1: 'h'!='r', dp[1][1]=min(dp[0][1]+1,dp[1][0]+1,dp[0][0]+1)=min(2,2,1)=1
i=1, j=2: 'h'!='o', dp[1][2]=min(3,3,2)=2
i=1, j=3: 'h'!='s', dp[1][3]=min(4,4,3)=3

i=2, j=1: 'o'!='r', dp[2][1]=min(3,2,2)=2
i=2, j=2: 'o'=='o', dp[2][2]=dp[1][1]=1
i=2, j=3: 'o'!='s', dp[2][3]=min(3,2,2)=2

i=3, j=1: 'r'=='r', dp[3][1]=dp[2][0]=2
i=3, j=2: 'r'!='o', dp[3][2]=min(3,3,2)=2
i=3, j=3: 'r'!='s', dp[3][3]=min(4,3,3)=3

i=4, j=1: 's'!='r', dp[4][1]=min(4,3,4)=3
i=4, j=2: 's'!='o', dp[4][2]=min(4,3,3)=3
i=4, j=3: 's'=='s', dp[4][3]=dp[3][2]=2

i=5, j=1: 'e'!='r', dp[5][1]=min(5,4,4)=4
i=5, j=2: 'e'!='o', dp[5][2]=min(5,4,4)=4
i=5, j=3: 'e'!='s', dp[5][3]=min(5,5,5)=5

结果: dp[5][3] = 5
```

---

## 变种/延伸

### 变种1：最长公共子序列
- **题目**：[lc-1143 最长公共子序列](./lc-1143-longest-common-subsequence.md)
- **变化**：只能删除，不能插入/替换
- **解法**：LCS 问题

### 变种2：两个字符串的删除操作
- **题目**：[lc-583 两个字符串的删除操作](./)
- **变化**：只能删除，使两字符串相等
- **解法**：LCS 变种，2*LCS 长度

### 进阶思考

1. **为什么三种操作足够？**
   - 插入 + 删除可以实现任意转换
   - 替换可以优化（否则需要删除+插入）
   - 这三种操作是完备的

2. **如何重建具体的编辑路径？**
   - 从 dp[m][n] 回溯
   - 根据来自的方向（删除、插入、替换）记录操作
   - 递归或迭代重建

3. **空间如何优化？**
   - 只需要前两行
   - 或者用一维数组 + 临时变量
   - 空间优化到 O(n)

4. **如何处理大量小操作？**
   - 批量操作优化
   - 或者限制最大编辑距离
   - 实际应用中的常用技巧

5. **与 Levenshtein 距离的关系？**
   - 编辑距离就是 Levenshtein 距离
   - 经典的字符串相似度度量
   - 广泛应用于拼写检查、DNA 比对等
