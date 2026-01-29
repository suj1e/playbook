# 5. 最长回文子串

[中文](https://leetcode.cn/problems/longest-palindromic-substring/) | [English](https://leetcode.com/problems/longest-palindromic-substring/)

**难度**：Medium
**标签**：字符串 | 双指针 | 动态规划

---

## 题目描述

给你一个字符串 `s`，找到 `s` 中最长的回文子串。

**示例 1**：
```
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```

**示例 2**：
```
输入：s = "cbbd"
输出："bb"
```

---

## 解题思路

### 方法一：中心扩展法（推荐）

**思路**：
- 回文串是关于中心对称的
- 枚举所有可能的中心点
- 从中心向两边扩展，找到最长的回文串

**中心点分类**：
- 奇数长度：中心是单个字符（如 "aba"，中心是 'b'）
- 偶数长度：中心是两个字符之间（如 "abba"，中心是 'b' 和 'b' 之间）

**复杂度**：
- 时间：O(n²)，n 个中心点，每个扩展 O(n)
- 空间：O(1)

**步骤**：
1. 遍历每个字符作为中心
2. 对每个中心，分别处理奇数和偶数长度
3. 从中心向两边扩展，记录最长的回文串

---

## 代码实现

### 方法一：中心扩展法

#### Java

```java
class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() < 2) {
            return s;
        }

        int start = 0, maxLen = 1;

        for (int i = 0; i < s.length(); i++) {
            // 奇数长度，以 i 为中心
            int len1 = expandAroundCenter(s, i, i);
            // 偶数长度，以 i 和 i+1 为中心
            int len2 = expandAroundCenter(s, i, i + 1);

            int len = Math.max(len1, len2);
            if (len > maxLen) {
                maxLen = len;
                start = i - (len - 1) / 2;
            }
        }

        return s.substring(start, start + maxLen);
    }

    // 返回以 left 和 right 为中心的最长回文串长度
    private int expandAroundCenter(String s, int left, int right) {
        while (left >= 0 && right < s.length() &&
               s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        // 返回长度，不需要 -1 因为循环结束时已经多走了一步
        return right - left - 1;
    }
}
```

#### Python

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        if not s or len(s) < 2:
            return s

        start, max_len = 0, 1

        for i in range(len(s)):
            # 奇数长度
            len1 = self.expand_around_center(s, i, i)
            # 偶数长度
            len2 = self.expand_around_center(s, i, i + 1)

            length = max(len1, len2)
            if length > max_len:
                max_len = length
                start = i - (length - 1) // 2

        return s[start:start + max_len]

    def expand_around_center(self, s: str, left: int, right: int) -> int:
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        return right - left - 1
```

---

### 方法二：动态规划

**思路**：
- `dp[i][j]` 表示 `s[i..j]` 是否为回文串
- 如果 `s[i] == s[j]` 且 `dp[i+1][j-1]` 为真，则 `dp[i][j]` 为真

**复杂度**：
- 时间：O(n²)
- 空间：O(n²)

#### Java

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n];

        int start = 0, maxLen = 1;

        // 单个字符都是回文
        for (int i = 0; i < n; i++) {
            dp[i][i] = true;
        }

        // 枚举长度
        for (int len = 2; len <= n; len++) {
            for (int i = 0; i <= n - len; i++) {
                int j = i + len - 1;

                if (s.charAt(i) == s.charAt(j)) {
                    if (len == 2) {
                        dp[i][j] = true;
                    } else {
                        dp[i][j] = dp[i + 1][j - 1];
                    }
                }

                if (dp[i][j] && len > maxLen) {
                    maxLen = len;
                    start = i;
                }
            }
        }

        return s.substring(start, start + maxLen);
    }
}
```

#### Python

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        n = len(s)
        dp = [[False] * n for _ in range(n)]

        start, max_len = 0, 1

        # 单个字符都是回文
        for i in range(n):
            dp[i][i] = True

        # 枚举长度
        for length in range(2, n + 1):
            for i in range(n - length + 1):
                j = i + length - 1

                if s[i] == s[j]:
                    if length == 2:
                        dp[i][j] = True
                    else:
                        dp[i][j] = dp[i + 1][j - 1]

                if dp[i][j] and length > max_len:
                    max_len = length
                    start = i

        return s[start:start + max_len]
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 中心扩展 | 奇数和偶数长度都要考虑 |
| 边界处理 | `left >= 0 && right < n` |
| DP 转移 | `s[i] == s[j] && dp[i+1][j-1]` |
| 长度计算 | `right - left - 1`（循环结束后多走了一步） |

---

## 方法对比

| 方法 | 时间 | 空间 | 说明 |
|------|------|------|------|
| 中心扩展 | O(n²) | O(1) | **推荐** |
| 动态规划 | O(n²) | O(n²) | 思路直观 |

---

## 变种/延伸

- **变种1**：最长回文子序列（不是子串，可以不连续）
- **变种2**：判断字符串是否可以重新排列成回文串
- **变种3**：最短回文串（在前面添加字符使其变成回文）
