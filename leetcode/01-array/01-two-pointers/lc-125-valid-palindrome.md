# 125. 验证回文串

[中文](https://leetcode.cn/problems/valid-palindrome/) | [English](https://leetcode.com/problems/valid-palindrome/)

**难度**：Easy
**标签**：双指针 | 字符串

---

## 题目描述

如果在将所有大写字符转换为小写字符、并移除所有非字母数字字符之后，短语正着读和反着读都一样，则可以认为该短语是一个回文串。

字母和数字都属于字母数字字符。

给你一个字符串 `s`，如果它是回文串，返回 `true`；否则，返回 `false`。

**示例 1**：
```
输入: s = "A man, a plan, a canal: Panama"
输出: true
解释："amanaplanacanalpanama" 是回文串
```

**示例 2**：
```
输入: s = "race a car"
输出: false
```

---

## 解题思路

### 方法一：双指针（相向指针）

**思路**：
- 使用左右双指针，从两端向中间遍历
- 跳过非字母数字字符
- 比较左右指针指向的字符（转换为小写）

**复杂度**：
- 时间：O(n)
- 空间：O(1)

**步骤**：
1. 初始化 `left = 0`，`right = s.length() - 1`
2. 当 `left < right` 时循环：
   - 跳过左边非字母数字字符
   - 跳过右边非字母数字字符
   - 比较两字符（转小写），不相等返回 `false`
   - `left++`，`right--`
3. 循环结束，返回 `true`

---

## 代码实现

### Java

```java
class Solution {
    public boolean isPalindrome(String s) {
        int left = 0, right = s.length() - 1;

        while (left < right) {
            // 跳过非字母数字字符
            while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
                left++;
            }
            while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
                right--;
            }

            // 比较（转小写）
            if (Character.toLowerCase(s.charAt(left)) !=
                Character.toLowerCase(s.charAt(right))) {
                return false;
            }

            left++;
            right--;
        }

        return true;
    }
}
```

### Python

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left, right = 0, len(s) - 1

        while left < right:
            # 跳过非字母数字字符
            while left < right and not s[left].isalnum():
                left += 1
            while left < right and not s[right].isalnum():
                right -= 1

            # 比较（转小写）
            if s[left].lower() != s[right].lower():
                return False

            left += 1
            right -= 1

        return True
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 字符判断 | Java 用 `Character.isLetterOrDigit()`，Python 用 `str.isalnum()` |
| 大小写转换 | Java 用 `Character.toLowerCase()`，Python 用 `str.lower()` |
| 边界条件 | 内层 while 也要检查 `left < right` |
| 时间复杂度 | O(n)，只遍历一次 |

---

## 变种/延伸

- **变种1**：只考虑字母，不考虑数字
- **变种2**：允许删除最多一个字符后成为回文串
- **变种3**：判断字符串是否可以重新排列成回文串
