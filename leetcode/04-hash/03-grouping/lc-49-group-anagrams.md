# 49. Group Anagrams / 字母异位词分组

[中文链接](https://leetcode.cn/problems/group-anagrams/) | [English](https://leetcode.com/problems/group-anagrams/)

**难度**：Medium
**标签**：数组 | 哈希表 | 字符串 | 排序
**相似题目**：[lc-242](../../../02-string/02-kmp/) | [lc-249](./) | [lc-451](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给你一个字符串数组，请你将 字母异位词 组合在一起。可以按任意顺序返回结果列表。

字母异位词 是由重新排列源单词的所有字母得到的一个新单词。

**示例 1**：
```
输入: strs = ["eat","tea","tan","ate","nat","bat"]
输出: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

**示例 2**：
```
输入: strs = [""]
输出: [[""]]
```

**示例 3**：
```
输入: strs = ["a"]
输出: [["a"]]
```

**约束条件**：
- 1 <= strs.length <= 10⁴
- 0 <= strs[i].length <= 100
- strs[i] 仅包含小写字母

---

## 解题思路

### 方法一：排序作为 key（推荐）

**思路**：
字母异位词排序后相同，用排序后的字符串作为哈希表的 key。

**复杂度**：
- 时间：O(n × k × log k)，n 是字符串数量，k 是字符串最大长度
- 空间：O(n × k)

**步骤**：
1. 遍历每个字符串
2. 对字符串排序作为 key
3. 将原字符串添加到对应 key 的列表中
4. 返回所有分组

---

### 方法二：计数作为 key

**思路**：
用每个字母的出现次数作为 key（避免排序）。

**复杂度**：
- 时间：O(n × k)
- 空间：O(n × k)

**步骤**：
1. 遍历每个字符串
2. 统计 26 个字母的出现次数，转为字符串作为 key
3. 将原字符串添加到对应 key 的列表中
4. 返回所有分组

---

### 方法三：质数乘积作为 key

**思路**：
将 26 个字母映射到 26 个质数，字母异位词的质数乘积相同。

**复杂度**：
- 时间：O(n × k)
- 空间：O(n × k)
- 注意：可能溢出

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：排序作为 key
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> groups = new HashMap<>();

        for (String s : strs) {
            // 排序后作为 key
            char[] chars = s.toCharArray();
            Arrays.sort(chars);
            String key = new String(chars);

            // 分组
            groups.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
        }

        return new ArrayList<>(groups.values());
    }

    // 方法二：计数作为 key
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> groups = new HashMap<>();

        for (String s : strs) {
            // 统计字母频次
            int[] count = new int[26];
            for (char c : s.toCharArray()) {
                count[c - 'a']++;
            }

            // 转为字符串作为 key
            StringBuilder key = new StringBuilder();
            for (int i = 0; i < 26; i++) {
                key.append('#');
                key.append(count[i]);
            }

            groups.computeIfAbsent(key.toString(), k -> new ArrayList<>()).add(s);
        }

        return new ArrayList<>(groups.values());
    }
}
```

### Python

```python
from typing import List
from collections import defaultdict

class Solution:
    # 方法一：排序作为 key
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        groups = defaultdict(list)

        for s in strs:
            # 排序后作为 key
            key = ''.join(sorted(s))
            groups[key].append(s)

        return list(groups.values())

    # 方法二：计数作为 key
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        groups = defaultdict(list)

        for s in strs:
            # 统计字母频次
            count = [0] * 26
            for c in s:
                count[ord(c) - ord('a')] += 1

            # 转为元组作为 key
            key = tuple(count)
            groups[key].append(s)

        return list(groups.values())
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 字母异位词特征 | 包含相同字母，只是顺序不同 |
| 排序 key | 异位词排序后相同，直观但需要排序 |
| 计数 key | 统计字母频次，避免排序开销 |
| # 分隔符 | 计数 key 中用 # 分隔，避免混淆（如 12 vs 1,2） |
| computeIfAbsent | Java 中简化 key 不存在时的处理 |
| 默认字典 | Python 中简化 key 不存在时的处理 |

---

## 图解

```
输入: ["eat","tea","tan","ate","nat","bat"]

方法一（排序）:
"eat"  -> "aet" -> groups["aet"] = ["eat"]
"tea"  -> "aet" -> groups["aet"] = ["eat", "tea"]
"tan"  -> "ant" -> groups["ant"] = ["tan"]
"ate"  -> "aet" -> groups["aet"] = ["eat", "tea", "ate"]
"nat"  -> "ant" -> groups["ant"] = ["tan", "nat"]
"bat"  -> "abt" -> groups["abt"] = ["bat"]

结果: [["bat"], ["nat","tan"], ["eat","tea","ate"]]

方法二（计数）:
"eat"  -> #1#0#0#0#1#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0#0
       (e:1, a:1, t:1，其余为0)

三种方法比较:
┌───────────┬────────────┬────────────┬──────────────┐
│   方法    │   时间      │   空间      │   优缺点      │
├───────────┼────────────┼────────────┼──────────────┤
│ 排序 key  │ O(n·k·logk)│   O(n·k)   │ 代码简单，排序开销│
│ 计数 key  │   O(n·k)   │   O(n·k)   │ 无排序，代码稍长 │
│ 质数乘积  │   O(n·k)   │   O(n·k)   │ 可能溢出      │
└───────────┴────────────┴────────────┴──────────────┘
```

---

## 变种/延伸

### 变种1：有效的字母异位词
- **题目**：[lc-242 有效的字母异位词](../../../02-string/02-kmp/)
- **变化**：判断两个字符串是否为字母异位词
- **解法**：排序比较 或 计数比较

### 变种2：字母异位词分组（返回索引）
- **变化**：返回的是索引而非字符串
- **解法**：在遍历时记录索引

### 变种3：分组后的字符串排序
- **变化**：每个分组内的字符串也需要排序
- **解法**：最后对每个组内排序

### 变种4：找所有字母异位词
- **题目**：[lc-438 找到字符串中所有字母异位词](../../../01-array/02-sliding-window/lc-438-find-all-anagrams.md)
- **变化**：在字符串 s 中找 p 的所有字母异位词子串
- **解法**：滑动窗口 + 计数

### 进阶思考

1. **为什么计数 key 中需要分隔符？**
   ```
   不加分隔符: [1,11] -> "111", [11,1] -> "111" (混淆!)
   加分隔符:   [1,11] -> "#1#11", [11,1] -> "#11#1" (清晰)
   ```

2. **如果字符串包含 Unicode 字符怎么办？**
   - 计数方法需要改为用 Map 而非固定数组
   - 或者使用排序方法更通用

3. **如何优化内存使用？**
   - 如果字符串很长，可以用哈希值（如 MD5）代替排序后的字符串
   - 但需要注意哈希冲突

4. **大数据场景如何优化？**
   - 可以分批处理
   - 或者使用 MapReduce 框架分布式处理

5. **质数乘积方法为什么可能溢出？**
   - 26 个质数的乘积可能超过 Long.MAX_VALUE
   - 只适合短字符串，实际应用中不推荐
