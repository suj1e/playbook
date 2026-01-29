# 392. 判断子序列

[中文](https://leetcode.cn/problems/is-subsequence/) | [English](https://leetcode.com/problems/is-subsequence/)

**难度**：Easy
**标签**：字符串 | 双指针

---

## 题目描述

给定字符串 `s` 和 `t`，判断 `s` 是否为 `t` 的子序列。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。

**示例 1**：
```
输入：s = "abc", t = "ahbgdc"
输出：true
```

**示例 2**：
```
输入：s = "axc", t = "ahbgdc"
输出：false
```

---

## 解题思路

### 方法一：双指针（推荐）

**思路**：
- 使用两个指针，`i` 指向 `s`，`j` 指向 `t`
- 遍历 `t`，如果 `t[j] == s[i]`，则 `i++`
- 最后检查 `i` 是否到达 `s` 的末尾

**复杂度**：
- 时间：O(n)，n 是 t 的长度
- 空间：O(1)

**步骤**：
1. 初始化 `i = 0`，`j = 0`
2. 遍历 `t`：
   - 如果 `s[i] == t[j]`，则 `i++`
   - `j++`
3. 返回 `i == s.length()`

---

### 方法二：DP（预处理）

**思路**：
- 预处理 `t`，记录每个位置之后各个字符第一次出现的位置
- 然后在 `s` 中跳转查找

**适用场景**：需要多次查询不同的 `s`

**复杂度**：
- 预处理：O(n × 26)
- 查询：O(m)，m 是 s 的长度
- 空间：O(n × 26)

---

## 代码实现

### 方法一：双指针

#### Java

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int i = 0, j = 0;

        while (i < s.length() && j < t.length()) {
            if (s.charAt(i) == t.charAt(j)) {
                i++;
            }
            j++;
        }

        return i == s.length();
    }
}
```

#### Python

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        i = j = 0

        while i < len(s) and j < len(t):
            if s[i] == t[j]:
                i += 1
            j += 1

        return i == len(s)
```

---

### 方法二：Python 简洁写法

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        it = iter(t)
        return all(c in it for c in s)
```

**解释**：`c in it` 会从迭代器中查找字符 `c`，找到后迭代器停留在该位置，下次查找继续往后。

---

### 方法三：DP 预处理

#### Java

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        // 预处理：dp[i][c] 表示 t 中位置 i 之后字符 c 第一次出现的位置
        int n = t.length();
        int[][] dp = new int[n + 1][26];

        // 初始化最后一行为 -1（表示找不到）
        Arrays.fill(dp[n], -1);

        // 从后往前填充
        for (int i = n - 1; i >= 0; i--) {
            // 复制下一行
            dp[i] = dp[i + 1].clone();
            // 更新当前字符
            dp[i][t.charAt(i) - 'a'] = i;
        }

        // 在 s 中跳转查找
        int pos = 0;
        for (char c : s.toCharArray()) {
            pos = dp[pos][c - 'a'];
            if (pos == -1) {
                return false;
            }
            pos++;  // 移动到下一位置
        }

        return true;
    }
}
```

#### Python

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        # 预处理：记录每个位置之后各字符首次出现的位置
        n = len(t)
        # dp[i][c] = position，26 个小写字母
        dp = [[-1] * 26 for _ in range(n + 1)]

        # 从后往前填充
        for i in range(n - 1, -1, -1):
            dp[i] = dp[i + 1][:]  # 复制下一行
            dp[i][ord(t[i]) - ord('a')] = i

        # 在 s 中跳转查找
        pos = 0
        for c in s:
            pos = dp[pos][ord(c) - ord('a')]
            if pos == -1:
                return False
            pos += 1

        return True
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 双指针 | `i` 只在匹配时前进，`j` 始终前进 |
| 子序列定义 | 不需要连续，保持相对顺序即可 |
| DP 预处理 | 适合多次查询的场景 |
| Python 技巧 | `iter()` 的妙用 |

---

## 图解

```
s = "abc", t = "ahbgdc"

i=0, j=0: s[0]='a' == t[0]='a', i++
i=1, j=1: s[1]='b' != t[1]='h'
i=1, j=2: s[1]='b' == t[2]='b', i++
i=2, j=3: s[2]='c' != t[3]='g'
i=2, j=4: s[2]='c' != t[4]='d'
i=2, j=5: s[2]='c' == t[5]='c', i++

i=3 == s.length()，返回 true
```

---

## 变种/延伸

- **变种1**：判断是否是子数组（需要连续）
- **变种2**：给定多个 s，判断每个是否是 t 的子序列
- **变种3**：找出 t 中有多少个不同的子序列等于 s
