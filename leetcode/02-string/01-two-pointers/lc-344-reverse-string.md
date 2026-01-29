# 344. 反转字符串

[中文](https://leetcode.cn/problems/reverse-string/) | [English](https://leetcode.com/problems/reverse-string/)

**难度**：Easy
**标签**：双指针 | 字符串

---

## 题目描述

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `s` 的形式给出。

不要给另外的数组分配额外的空间，你必须**原地修改输入数组**、使用 O(1) 的额外空间解决这一问题。

**示例 1**：
```
输入：s = ["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```

**示例 2**：
```
输入：s = ["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]
```

---

## 解题思路

### 方法一：双指针（相向指针）

**思路**：
- 使用左右双指针，从两端向中间遍历
- 交换 `s[left]` 和 `s[right]`
- 直到 `left >= right`

**复杂度**：
- 时间：O(n/2) = O(n)
- 空间：O(1)

---

## 代码实现

### Java

```java
class Solution {
    public void reverseString(char[] s) {
        int left = 0, right = s.length - 1;

        while (left < right) {
            // 交换
            char temp = s[left];
            s[left] = s[right];
            s[right] = temp;

            // 移动指针
            left++;
            right--;
        }
    }
}
```

### Python

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left, right = 0, len(s) - 1

        while left < right:
            # 交换
            s[left], s[right] = s[right], s[left]

            # 移动指针
            left += 1
            right -= 1
```

---

### 方法二：Python 简洁写法

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        s.reverse()  # 原地反转

        # 或者
        # s[:] = s[::-1]  # 注意要用 s[:] 赋值才是原地修改
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 原地修改 | 不使用额外空间，直接修改输入 |
| 双指针 | 左右相向而行，交换元素 |
| 循环条件 | `left < right`，不需要等于 |
| Python 注意 | `s[::-1]` 会创建新数组，要用 `s[:] = s[::-1]` |

---

## 变种/延伸

- **变种1**：反转字符串 II（每隔 k 个字符反转）
- **变种2**：反转字符串中的单词 III
- **变种3**：仅反转字母
