# 3. 无重复字符的最长子串

[中文](https://leetcode.cn/problems/longest-substring-without-repeating-characters/) | [English](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

**难度**：Medium
**标签**：字符串 | 滑动窗口 | 哈希表

---

## 题目描述

给定一个字符串 `s`，请你找出其中不含有重复字符的**最长子串**的长度。

**示例 1**：
```
输入: s = "abcabcbb"
输出: 3
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2**：
```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3**：
```
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

---

## 解题思路

### 方法一：滑动窗口 + 哈希集合

**思路**：
- 使用滑动窗口 `[left, right]` 维护无重复字符的子串
- 使用哈希集合记录窗口内的字符
- 当遇到重复字符时，收缩左边界直到重复字符被移除

**复杂度**：
- 时间：O(n)，每个字符最多被访问两次
- 空间：O(min(m, n))，m 为字符集大小

**步骤**：
1. 初始化 `left = 0`，`maxLen = 0`，哈希集合 `window`
2. `right` 从 0 到 n-1 遍历：
   - 如果 `s[right]` 已在窗口中：
     - 从左边不断移除字符，直到重复字符被移除
   - 将 `s[right]` 加入窗口
   - 更新 `maxLen`
3. 返回 `maxLen`

**图解**：
```
s = "abcabcbb"

right=0: 'a' 加入, window={a}, maxLen=1
right=1: 'b' 加入, window={a,b}, maxLen=2
right=2: 'c' 加入, window={a,b,c}, maxLen=3
right=3: 'a' 已存在, 移除 'a','b','c', window={a}, maxLen=3
right=4: 'b' 已存在, 移除 'a', window={b}, maxLen=3
right=5: 'c' 已存在, 移除 'b', window={c}, maxLen=3
right=6: 'b' 已存在, 移除 'c', window={b}, maxLen=3
right=7: 'b' 已存在, 移除 'b', window={}, maxLen=3

最终 maxLen = 3
```

---

## 代码实现

### Java

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Set<Character> window = new HashSet<>();
        int left = 0;
        int maxLen = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);

            // 如果字符已存在，收缩窗口
            while (window.contains(c)) {
                window.remove(s.charAt(left));
                left++;
            }

            // 加入当前字符
            window.add(c);

            // 更新最大长度
            maxLen = Math.max(maxLen, right - left + 1);
        }

        return maxLen;
    }
}
```

### Python

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        window = set()
        left = 0
        max_len = 0

        for right in range(len(s)):
            # 如果字符已存在，收缩窗口
            while s[right] in window:
                window.remove(s[left])
                left += 1

            # 加入当前字符
            window.add(s[right])

            # 更新最大长度
            max_len = max(max_len, right - left + 1)

        return max_len
```

---

### 方法二：滑动窗口优化（直接跳转）

**优化思路**：使用哈希表记录字符的最新位置，遇到重复时直接跳转 `left`。

#### Java

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> lastIndex = new HashMap<>();
        int left = 0;
        int maxLen = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);

            // 如果字符已出现过，且在窗口内
            if (lastIndex.containsKey(c) && lastIndex.get(c) >= left) {
                left = lastIndex.get(c) + 1;
            }

            // 更新字符位置
            lastIndex.put(c, right);

            // 更新最大长度
            maxLen = Math.max(maxLen, right - left + 1);
        }

        return maxLen;
    }
}
```

#### Python

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        last_index = {}
        left = 0
        max_len = 0

        for right, c in enumerate(s):
            # 如果字符已出现过，且在窗口内
            if c in last_index and last_index[c] >= left:
                left = last_index[c] + 1

            # 更新字符位置
            last_index[c] = right

            # 更新最大长度
            max_len = max(max_len, right - left + 1)

        return max_len
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 窗口维护 | 保证窗口内无重复字符 |
| 收缩条件 | 新字符已在窗口中 |
| 优化方法 | 记录字符位置，直接跳转 `left` |
| 时间复杂度 | O(n)，每个字符最多被访问两次 |

---

## 变种/延伸

- **变种1**：返回最长无重复子串本身
- **变种2**：至少有 k 个不同字符的最长子串
- **变种3**：至多有 k 个不同字符的最长子串
