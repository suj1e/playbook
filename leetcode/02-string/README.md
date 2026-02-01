# 字符串专题

## 专题概述

字符串是力扣中常见的数据结构，本专题涵盖：

- **双指针**：处理字符串的修改、反转、回文判断等
- **KMP 算法**：高效的字符串匹配算法
- **子序列问题**：最长公共子序列、编辑距离等

### 核心知识点

1. 字符串的不可变性（Java/Python）
2. 双指针在字符串中的应用
3. KMP 算法的 next 数组构建
4. 动态规划处理子序列问题

---

## 学习路径

1. 先阅读 [templates.md](./templates.md) 熟悉字符串操作模板
2. 按顺序完成各分类题目：
   - [x] 01-two-pointers（双指针，基础）
   - [x] 02-sliding-window（滑动窗口）
   - [x] 02-kmp（KMP 算法，进阶）
   - [x] 03-subsequence（子序列问题，DP）

---

## 题目分类

| 分类 | 题目数 | 难度 | 说明 |
|------|-------|------|------|
| 01-two-pointers | 2 | ⭐⭐ | 双指针基础应用 |
| 02-sliding-window | 1 | ⭐⭐⭐⭐ | 滑动窗口经典题 |
| 02-kmp | 2 | ⭐⭐⭐ | KMP 字符串匹配 |
| 03-subsequence | 2 | ⭐⭐⭐ | 子序列 DP 问题 |

---

## 进度追踪

### 01-two-pointers
- [x] lc-344-reverse-string (Easy)
- [x] lc-5-longest-palindromic-substring (Medium)

### 02-sliding-window
- [x] lc-3-longest-substring-without-repeating-characters (Medium)

### 02-kmp
- [x] lc-28-find-the-index-of-the-first-occurrence (Medium)
- [x] lc-459-repeated-substring-pattern (Medium)

### 03-subsequence
- [x] lc-392-is-subsequence (Easy)
- [x] lc-516-longest-palindromic-subsequence (Medium)

---

## 解题模板总结

### 字符串反转模板
```java
int left = 0, right = s.length() - 1;
char[] chars = s.toCharArray();
while (left < right) {
    char temp = chars[left];
    chars[left++] = chars[right];
    chars[right--] = temp;
}
return new String(chars);
```

### KMP 模板
```java
// 构建 next 数组
int[] next = new int[pattern.length()];
for (int i = 1, j = 0; i < pattern.length(); i++) {
    while (j > 0 && pattern.charAt(i) != pattern.charAt(j)) {
        j = next[j - 1];
    }
    if (pattern.charAt(i) == pattern.charAt(j)) {
        j++;
    }
    next[i] = j;
}

// 匹配
for (int i = 0, j = 0; i < text.length(); i++) {
    while (j > 0 && text.charAt(i) != pattern.charAt(j)) {
        j = next[j - 1];
    }
    if (text.charAt(i) == pattern.charAt(j)) {
        j++;
    }
    if (j == pattern.length()) {
        return i - j + 1; // 找到匹配
    }
}
```

---

## 参考资源

- [LeetCode 字符串标签](https://leetcode.cn/tag/string/)
- [KMP 算法详解](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/solution/)
