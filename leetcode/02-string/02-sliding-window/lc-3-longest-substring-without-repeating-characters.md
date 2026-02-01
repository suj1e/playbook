# 3. Longest Substring Without Repeating Characters / 无重复字符的最长子串

[中文链接](https://leetcode.cn/problems/longest-substring-without-repeating-characters/) | [English](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

**难度**：Medium
**标签**：字符串 | 哈希表 | 滑动窗口
**相似题目**：[lc-159](./) | [lc-340](./) | [lc-90](../../04-hash/03-grouping/lc-49-group-anagrams.md)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

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

**约束条件**：
- 0 <= s.length <= 5 * 10⁴
- s 由英文字母、数字、符号和空格组成

---

## 解题思路

### 方法一：滑动窗口 + 哈希表（推荐）

**思路**：
维护一个滑动窗口 [left, right]，用哈希表记录每个字符最后出现的位置。当遇到重复字符时，移动 left 到重复字符的下一个位置。

**核心思想**：
- 窗口内始终是无重复字符的子串
- 遇到重复字符时，收缩窗口
- 用哈希表记录字符位置，O(1) 判断重复

**复杂度**：
- 时间：O(n) - 每个字符最多被访问两次
- 空间：O(min(m, n)) - m 是字符集大小，n 是字符串长度

**步骤**：
1. 初始化 left = 0，maxLen = 0
2. 用 HashMap 记录每个字符的最后索引
3. 遍历 right 从 0 到 n-1：
   - 如果 s[right] 在 map 中且索引 >= left：有重复
   - 更新 left = map.get(s[right]) + 1
   - 更新 map.put(s[right], right)
   - 更新 maxLen = max(maxLen, right - left + 1)

### 方法二：滑动窗口 + 数组优化

**思路**：
用数组代替 HashMap，适用于 ASCII 字符集。

**复杂度**：
- 时间：O(n)
- 空间：O(128) 或 O(256) - 固定大小

---

## 代码实现

### Java - 滑动窗口 + HashMap

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }

        HashMap<Character, Integer> map = new HashMap<>();
        int maxLen = 0;
        int left = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);

            // 如果字符已存在且在窗口内
            if (map.containsKey(c) && map.get(c) >= left) {
                left = map.get(c) + 1;  // 收缩窗口
            }

            map.put(c, right);  // 更新字符位置
            maxLen = Math.max(maxLen, right - left + 1);
        }

        return maxLen;
    }
}
```

### Java - 数组优化版

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }

        // ASCII 字符集，用数组代替 HashMap
        int[] index = new int[128];
        Arrays.fill(index, -1);

        int maxLen = 0;
        int left = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);

            // 如果字符已存在且在窗口内
            if (index[c] >= left) {
                left = index[c] + 1;  // 收缩窗口
            }

            index[c] = right;  // 更新字符位置
            maxLen = Math.max(maxLen, right - left + 1);
        }

        return maxLen;
    }
}
```

### Python - 滑动窗口

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if not s:
            return 0

        char_index = {}
        max_len = 0
        left = 0

        for right, char in enumerate(s):
            # 如果字符已存在且在窗口内
            if char in char_index and char_index[char] >= left:
                left = char_index[char] + 1

            char_index[char] = right
            max_len = max(max_len, right - left + 1)

        return max_len
```

### Python - 简洁版

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        char_index = {}
        left = max_len = 0

        for right, char in enumerate(s):
            if char in char_index and char_index[char] >= left:
                left = char_index[char] + 1
            char_index[char] = right
            max_len = max(max_len, right - left + 1)

        return max_len
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 滑动窗口 | [left, right] 区间内无重复字符 |
| HashMap | 记录字符最后出现的位置 |
| left 更新条件 | index[c] >= left（确保在窗口内） |
| maxLen 更新 | 每次循环都计算当前窗口长度 |
| 时间复杂度 | O(n)，每个字符最多访问两次 |

---

## 图解

```
s = "abcabcbb"

初始化: left = 0, maxLen = 0, map = {}

right=0, char='a':
  map中没有'a'
  map = {'a': 0}
  maxLen = max(0, 0-0+1) = 1
  窗口: [a]bcabcbb

right=1, char='b':
  map中没有'b'
  map = {'a': 0, 'b': 1}
  maxLen = max(1, 1-0+1) = 2
  窗口: [a,b]cabcbb

right=2, char='c':
  map中没有'c'
  map = {'a': 0, 'b': 1, 'c': 2}
  maxLen = max(2, 2-0+1) = 3
  窗口: [a,b,c]abcbb

right=3, char='a':
  map中有'a'=0, 0 >= left=0
  left = 0 + 1 = 1
  map = {'a': 3, 'b': 1, 'c': 2}
  maxLen = max(3, 3-1+1) = 3
  窗口: a[b,c,a]bcbb

right=4, char='b':
  map中有'b'=1, 1 >= left=1
  left = 1 + 1 = 2
  map = {'a': 3, 'b': 4, 'c': 2}
  maxLen = max(3, 4-2+1) = 3
  窗口: ab[c,a,b]cbb

right=5, char='c':
  map中有'c'=2, 2 >= left=2
  left = 2 + 1 = 3
  map = {'a': 3, 'b': 4, 'c': 5}
  maxLen = max(3, 5-3+1) = 3
  窗口: abc[a,b,c]bb

right=6, char='b':
  map中有'b'=4, 4 >= left=3
  left = 4 + 1 = 5
  map = {'a': 3, 'b': 6, 'c': 5}
  maxLen = max(3, 6-5+1) = 2
  窗口: abcab[c,b]b

right=7, char='b':
  map中有'b'=6, 6 >= left=5
  left = 6 + 1 = 7
  map = {'a': 3, 'b': 7, 'c': 5}
  maxLen = max(2, 7-7+1) = 2
  窗口: abcabcb[b]

最终结果: maxLen = 3
最长无重复子串: "abc"

---

s = "pwwkew"

right=0, 'p': map={'p':0}, maxLen=1, 窗口[p]wwkew
right=1, 'w': map={'p':0,'w':1}, maxLen=2, 窗口[p,w]wkew
right=2, 'w': 重复! left=2, map={'p':0,'w':2}, maxLen=2, 窗口pw[w,kew]
right=3, 'k': map={...,'k':3}, maxLen=2, 窗口pw[w,k]ew
right=4, 'e': map={...,'e':4}, maxLen=3, 窗口pw[w,k,e]w
right=5, 'w': 重复! left=3, map={...,'w':5}, maxLen=3, 窗口pww[k,e,w]

最终: maxLen = 3
```

---

## 变种/延伸

### 变种1：至多包含两个不同字符的最长子串
- **题目**：[lc-159 至多包含两个不同字符的最长子串](./)
- **变化**：允许最多 2 个重复字符
- **解法**：类似的滑动窗口，用 HashMap 计数

### 变种2：至多包含 K 个不同字符的最长子串
- **题目**：[lc-340 至多包含 K 个不同字符的最长子串](./)
- **变化**：允许最多 K 个不同字符
- **解法**：滑动窗口 + HashMap 计数

### 变种3：找到字符串中所有字母异位词
- **题目**：[lc-438 找到字符串中所有字母异位词](../03-subsequence/lc-438-find-all-anagrams-in-a-string.md)
- **变化**：固定长度的滑动窗口
- **解法**：滑动窗口 + 计数

### 进阶思考

1. **为什么 left 更新条件要加 >= left？**
   - 确保重复字符在当前窗口内
   - 如果重复字符在窗口左侧，不需要移动 left

2. **如何理解滑动窗口？**
   - 窗口是满足条件的区间
   - 右边界扩展，左边界收缩
   - 始终维护窗口的有效性

3. **数组优化何时有效？**
   - 字符集较小时（ASCII）
   - Unicode 字符集不适合，还是用 HashMap

4. **如果要求返回子串而非长度？**
   - 记录最大长度时的 left 和 right
   - 返回 s.substring(left, right + 1)

5. **与暴力解法对比？**
   - 暴力：O(n³) - 检查所有子串
   - 滑动窗口：O(n) - 每个字符最多进出窗口一次

6. **如何处理空字符串？**
   - 直接返回 0
   - 代码中需要特殊处理或自然返回 0
