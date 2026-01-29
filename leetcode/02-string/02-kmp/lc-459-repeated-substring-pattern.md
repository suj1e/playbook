# 459. 重复的子字符串

[中文](https://leetcode.cn/problems/repeated-substring-pattern/) | [English](https://leetcode.com/problems/repeated-substring-pattern/)

**难度**：Medium
**标签**：字符串 | KMP算法

---

## 题目描述

给定一个非空的字符串 `s`，检查是否可以通过由它的一个子串重复多次构成。

**示例 1**：
```
输入：s = "abab"
输出：true
解释：它可以由子串 "ab" 重复两次构成。
```

**示例 2**：
```
输入：s = "aba"
输出：false
```

**示例 3**：
```
输入：s = "abcabcabcabc"
输出：true
解释：它可以由子串 "abc" 重复四次构成。
```

---

## 解题思路

### 方法一：KMP 优化（推荐）

**思路**：
- 使用 KMP 算法构建 next 数组
- 如果字符串由重复子串构成，那么：
  - `len % (len - next[len-1]) == 0`
  - 且 `next[len-1] != 0`

**原理**：
- `next[len-1]` 表示整个字符串的最长相等前后缀长度
- 如果字符串由重复子串构成，去掉最长相等前后缀后，剩下的部分就是一个周期
- 周期长度 = `len - next[len-1]`
- 如果 `len` 是周期的整数倍，则由重复子串构成

**复杂度**：
- 时间：O(n)
- 空间：O(n)

**图解**：
```
s = "ababab"
next = [0,0,1,2,3,4]

next[len-1] = next[5] = 4
len = 6
len - next[len-1] = 6 - 4 = 2

6 % 2 == 0，所以由重复子串构成

最长相等前后缀：
前缀 "abab"
后缀 "abab"
中间剩下的 "ab" 就是最小重复单元
```

---

### 方法二：字符串匹配技巧

**思路**：
- 将字符串复制一份：`s + s`
- 去掉首尾字符，避免匹配到自己
- 如果原字符串是重复子串，那么一定能在 `s + s` 中找到

**原理**：
- 如果 `s = "abcabc"`，那么 `s + s = "abcabcabcabc"`
- 去掉首尾后是 `"bcabcabcab"`
- 在其中一定能找到完整的 `"abcabc"`

**复杂度**：
- 时间：O(n)（取决于字符串匹配的实现）
- 空间：O(n)

---

### 方法三：枚举子串长度

**思路**：
- 枚举可能的子串长度（必须是 n 的因数）
- 检查该长度的子串是否能构成整个字符串

**复杂度**：
- 时间：O(n × √n)
- 空间：O(n)

---

## 代码实现

### 方法一：KMP 优化

#### Java

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int n = s.length();
        int[] next = buildNext(s);

        // 最长相等前后缀长度
        int maxPrefixSuffix = next[n - 1];

        // 如果没有相等前后缀，不能由重复子串构成
        if (maxPrefixSuffix == 0) {
            return false;
        }

        // 检查是否是整数倍
        int period = n - maxPrefixSuffix;
        return n % period == 0;
    }

    private int[] buildNext(String s) {
        int n = s.length();
        int[] next = new int[n];

        for (int i = 1, j = 0; i < n; i++) {
            while (j > 0 && s.charAt(i) != s.charAt(j)) {
                j = next[j - 1];
            }
            if (s.charAt(i) == s.charAt(j)) {
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
    def repeatedSubstringPattern(self, s: str) -> bool:
        n = len(s)
        next_arr = self.build_next(s)

        # 最长相等前后缀长度
        max_prefix_suffix = next_arr[-1]

        # 如果没有相等前后缀，不能由重复子串构成
        if max_prefix_suffix == 0:
            return False

        # 检查是否是整数倍
        period = n - max_prefix_suffix
        return n % period == 0

    def build_next(self, s: str) -> list:
        n = len(s)
        next_arr = [0] * n

        j = 0
        for i in range(1, n):
            while j > 0 and s[i] != s[j]:
                j = next_arr[j - 1]
            if s[i] == s[j]:
                j += 1
            next_arr[i] = j

        return next_arr
```

---

### 方法二：字符串匹配技巧

#### Java

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        String doubled = s + s;
        // 去掉首尾字符，避免匹配到自己
        String trimmed = doubled.substring(1, doubled.length() - 1);

        return trimmed.contains(s);
    }
}
```

#### Python

```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        doubled = s + s
        # 去掉首尾字符，避免匹配到自己
        trimmed = doubled[1:-1]

        return s in trimmed
```

---

### 方法三：枚举子串长度

#### Java

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int n = s.length();

        // 枚举可能的子串长度
        for (int len = 1; len <= n / 2; len++) {
            if (n % len != 0) continue;  // 必须是因数

            String pattern = s.substring(0, len);
            boolean match = true;

            // 检查是否能构成整个字符串
            for (int i = len; i < n && match; i += len) {
                if (!s.substring(i, i + len).equals(pattern)) {
                    match = false;
                }
            }

            if (match) return true;
        }

        return false;
    }
}
```

#### Python

```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        n = len(s)

        # 枚举可能的子串长度
        for length in range(1, n // 2 + 1):
            if n % length != 0:
                continue  # 必须是因数

            pattern = s[:length]
            if pattern * (n // length) == s:
                return True

        return False
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| next[len-1] | 最长相等前后缀长度 |
| 周期长度 | `len - next[len-1]` |
| 整除判断 | `len % period == 0` |
| 字符串技巧 | `(s+s)[1:-1]` 中查找 s |

---

## 方法对比

| 方法 | 时间 | 空间 | 说明 |
|------|------|------|------|
| KMP | O(n) | O(n) | **推荐** |
| 字符串技巧 | O(n) | O(n) | 代码简洁 |
| 枚举 | O(n × √n) | O(n) | 思路直观 |

---

## 变种/延伸

- **变种1**：找出最小的重复子串
- **变种2**：找出所有可能的重复子串
- **变种3**：给定一个重复单元，构造字符串
