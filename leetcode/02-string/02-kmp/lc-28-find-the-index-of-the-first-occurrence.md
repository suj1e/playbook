# 28. 找出字符串中第一个匹配项的下标

[中文](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/) | [English](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)

**难度**：Medium
**标签**：字符串 | 双指针 | KMP算法

---

## 题目描述

给你两个字符串 `haystack` 和 `needle`，请你在 `haystack` 字符串中找出 `needle` 字符串出现的第一个位置（下标从 0 开始）。如果不存在，则返回 `-1`。

**示例 1**：
```
输入：haystack = "sadbutsad", needle = "sad"
输出：0
解释："sad" 在下标 0 和 6 处出现。第一个出现的位置是下标 0。
```

**示例 2**：
```
输入：haystack = "leetcode", needle = "leeto"
输出：-1
解释："leeto" 没有在 "leetcode" 中出现。
```

---

## 解题思路

### 方法一：KMP 算法（推荐）

**思路**：
- KMP 算法通过预处理模式串，构建 `next` 数组（前缀表）
- 匹配失败时，利用 `next` 数组跳过已匹配的部分
- 时间复杂度 O(m + n)，优于暴力匹配的 O(m × n)

**next 数组含义**：
- `next[i]` 表示 `needle[0..i]` 的最长相等前后缀长度
- 例如：`needle = "aabaaf"`
  - `next[0] = 0`（单个字符没有前后缀）
  - `next[1] = 1`（"aa" 的最长相等前后缀是 "a"）
  - `next[2] = 0`（"aab" 没有相等前后缀）
  - ...

**复杂度**：
- 时间：O(m + n)，m 是 haystack 长度，n 是 needle 长度
- 空间：O(n)，存储 next 数组

---

### 方法二：暴力匹配

**思路**：
- 遍历 haystack 的每个位置作为起点
- 检查从该位置开始的子串是否等于 needle

**复杂度**：
- 时间：O(m × n)
- 空间：O(1)

---

## 代码实现

### 方法一：KMP 算法

#### Java

```java
class Solution {
    public int strStr(String haystack, String needle) {
        if (needle.isEmpty()) return 0;

        int[] next = buildNext(needle);

        for (int i = 0, j = 0; i < haystack.length(); i++) {
            // 不匹配，回退 j
            while (j > 0 && haystack.charAt(i) != needle.charAt(j)) {
                j = next[j - 1];
            }

            // 匹配，j 前进
            if (haystack.charAt(i) == needle.charAt(j)) {
                j++;
            }

            // 完全匹配
            if (j == needle.length()) {
                return i - j + 1;
            }
        }

        return -1;
    }

    // 构建 next 数组
    private int[] buildNext(String pattern) {
        int n = pattern.length();
        int[] next = new int[n];

        for (int i = 1, j = 0; i < n; i++) {
            // 不匹配，回退 j
            while (j > 0 && pattern.charAt(i) != pattern.charAt(j)) {
                j = next[j - 1];
            }

            // 匹配，j 前进
            if (pattern.charAt(i) == pattern.charAt(j)) {
                j++;
            }

            next[i] = j;
        }

        return next;
    }
}
```

#### Python

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if not needle:
            return 0

        next_arr = self.build_next(needle)
        j = 0

        for i in range(len(haystack)):
            # 不匹配，回退 j
            while j > 0 and haystack[i] != needle[j]:
                j = next_arr[j - 1]

            # 匹配，j 前进
            if haystack[i] == needle[j]:
                j += 1

            # 完全匹配
            if j == len(needle):
                return i - j + 1

        return -1

    def build_next(self, pattern: str) -> list:
        n = len(pattern)
        next_arr = [0] * n

        j = 0
        for i in range(1, n):
            # 不匹配，回退 j
            while j > 0 and pattern[i] != pattern[j]:
                j = next_arr[j - 1]

            # 匹配，j 前进
            if pattern[i] == pattern[j]:
                j += 1

            next_arr[i] = j

        return next_arr
```

---

### 方法二：暴力匹配

#### Java

```java
class Solution {
    public int strStr(String haystack, String needle) {
        int m = haystack.length(), n = needle.length();

        for (int i = 0; i <= m - n; i++) {
            int j;
            for (j = 0; j < n; j++) {
                if (haystack.charAt(i + j) != needle.charAt(j)) {
                    break;
                }
            }
            if (j == n) {
                return i;
            }
        }

        return -1;
    }
}
```

#### Python

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        m, n = len(haystack), len(needle)

        for i in range(m - n + 1):
            for j in range(n):
                if haystack[i + j] != needle[j]:
                    break
            else:
                return i

        return -1
```

---

### 方法三：Python 简洁写法

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        return haystack.find(needle)
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| next 数组 | 记录模式串的最长相等前后缀 |
| 回退 j | `j = next[j - 1]`，利用已匹配的信息 |
| 前缀定义 | 不包含最后一个字符的子串 |
| 后缀定义 | 不包含第一个字符的子串 |

---

## next 数组构建图解

```
pattern = "aabaaf"

i=1, j=0: pattern[1]='a' == pattern[0]='a'
         next[1] = 1, j = 1

i=2, j=1: pattern[2]='b' != pattern[1]='a'
         j = next[0] = 0
         pattern[2]='b' != pattern[0]='a'
         next[2] = 0

i=3, j=0: pattern[3]='a' == pattern[0]='a'
         next[3] = 1, j = 1

i=4, j=1: pattern[4]='a' == pattern[1]='a'
         next[4] = 2, j = 2

i=5, j=2: pattern[5]='f' != pattern[2]='b'
         j = next[1] = 1
         pattern[5]='f' != pattern[1]='a'
         j = next[0] = 0
         pattern[5]='f' != pattern[0]='a'
         next[5] = 0

next = [0, 1, 0, 1, 2, 0]
```

---

## 方法对比

| 方法 | 时间 | 空间 | 说明 |
|------|------|------|------|
| KMP | O(m + n) | O(n) | **推荐**，适合大规模数据 |
| 暴力 | O(m × n) | O(1) | 代码简单，适合小数据 |

---

## 变种/延伸

- **变种1**：找出所有匹配的位置
- **变种2**：返回匹配的次数
- **变种3**：支持通配符的字符串匹配
