# 438. 找到字符串中所有字母异位词

[中文](https://leetcode.cn/problems/find-all-anagrams-in-a-string/) | [English](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

**难度**：Medium
**标签**：字符串 | 滑动窗口 | 哈希表

---

## 题目描述

给定两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的**异位词**的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

**异位词**指由相同字母重新排列形成的字符串（包括相同的字符串）。

**示例 1**：
```
输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
```

**示例 2**：
```
输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
```

---

## 解题思路

### 方法一：滑动窗口 + 数组计数

**思路**：
- 使用固定大小为 `len(p)` 的滑动窗口
- 用数组记录每个字母的频率差
- 当窗口内字母频率与 `p` 完全一致时，记录起始位置

**复杂度**：
- 时间：O(n)
- 空间：O(1)，只有 26 个字母

**步骤**：
1. 初始化计数数组 `count`，记录 `p` 中每个字母的频率
2. 遍历 `s`，维护固定大小的窗口：
   - 进入窗口的字符：`count[c]--`
   - 离开窗口的字符：`count[c]++`
   - 检查所有计数是否为 0
3. 如果全为 0，说明是异位词，记录起始位置

---

## 代码实现

### 方法一：数组计数（优化版）

#### Java

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();

        if (s.length() < p.length()) {
            return result;
        }

        int[] count = new int[26];
        int windowSize = p.length();

        // 记录 p 中每个字母的频率
        for (char c : p.toCharArray()) {
            count[c - 'a']++;
        }

        // 滑动窗口
        for (int i = 0; i < s.length(); i++) {
            // 进入窗口
            count[s.charAt(i) - 'a']--;

            // 离开窗口
            if (i >= windowSize) {
                count[s.charAt(i - windowSize) - 'a']++;
            }

            // 检查是否为异位词
            if (i >= windowSize - 1 && isAnagram(count)) {
                result.add(i - windowSize + 1);
            }
        }

        return result;
    }

    private boolean isAnagram(int[] count) {
        for (int c : count) {
            if (c != 0) return false;
        }
        return true;
    }
}
```

#### Python

```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        result = []

        if len(s) < len(p):
            return result

        count = [0] * 26
        window_size = len(p)

        # 记录 p 中每个字母的频率
        for c in p:
            count[ord(c) - ord('a')] += 1

        # 滑动窗口
        for i in range(len(s)):
            # 进入窗口
            count[ord(s[i]) - ord('a')] -= 1

            # 离开窗口
            if i >= window_size:
                count[ord(s[i - window_size]) - ord('a')] += 1

            # 检查是否为异位词
            if i >= window_size - 1 and all(c == 0 for c in count):
                result.append(i - window_size + 1)

        return result
```

---

### 方法二：滑动窗口优化（记录不匹配数量）

**优化思路**：维护一个变量记录有多少个字母的计数不为 0，避免每次遍历 26 个字母。

#### Java

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();

        if (s.length() < p.length()) {
            return result;
        }

        int[] count = new int[26];
        int windowSize = p.length();
        int mismatch = 0;  // 记录有多少个字母计数不为0

        // 初始化计数
        for (char c : p.toCharArray()) {
            if (++count[c - 'a'] == 1) {
                mismatch++;
            }
        }

        // 滑动窗口
        for (int i = 0; i < s.length(); i++) {
            // 进入窗口
            int idx1 = s.charAt(i) - 'a';
            if (--count[idx1] == 0) {
                mismatch--;
            } else if (count[idx1] == -1) {
                mismatch++;
            }

            // 离开窗口
            if (i >= windowSize) {
                int idx2 = s.charAt(i - windowSize) - 'a';
                if (++count[idx2] == 0) {
                    mismatch--;
                } else if (count[idx2] == 1) {
                    mismatch++;
                }
            }

            // 检查是否为异位词
            if (i >= windowSize - 1 && mismatch == 0) {
                result.add(i - windowSize + 1);
            }
        }

        return result;
    }
}
```

#### Python

```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        result = []

        if len(s) < len(p):
            return result

        count = [0] * 26
        window_size = len(p)
        mismatch = 0  # 记录有多少个字母计数不为0

        # 初始化计数
        for c in p:
            count[ord(c) - ord('a')] += 1
            if count[ord(c) - ord('a')] == 1:
                mismatch += 1

        # 滑动窗口
        for i in range(len(s)):
            # 进入窗口
            idx1 = ord(s[i]) - ord('a')
            count[idx1] -= 1
            if count[idx1] == 0:
                mismatch -= 1
            elif count[idx1] == -1:
                mismatch += 1

            # 离开窗口
            if i >= window_size:
                idx2 = ord(s[i - window_size]) - ord('a')
                count[idx2] += 1
                if count[idx2] == 0:
                    mismatch -= 1
                elif count[idx2] == 1:
                    mismatch += 1

            # 检查是否为异位词
            if i >= window_size - 1 and mismatch == 0:
                result.append(i - window_size + 1)

        return result
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 固定窗口 | 窗口大小固定为 `len(p)` |
| 计数数组 | 用 26 长度数组记录字母频率 |
| 优化方法 | 用 `mismatch` 变量避免遍历数组 |
| 时间复杂度 | O(n) |

---

## 变种/延伸

- **变种1**：找出所有异位词的子串本身
- **变种2**：判断两个字符串是否互为异位词
- **变种3**：扩展到 Unicode 字符
