# 字符串代码模板

字符串处理在力扣中非常常见，掌握这些模板可以提高解题效率。

---

## 1. 基础操作模板

### 1.1 字符串反转

#### Java
```java
// 使用 char 数组
char[] chars = s.toCharArray();
int left = 0, right = chars.length - 1;
while (left < right) {
    char temp = chars[left];
    chars[left++] = chars[right];
    chars[right--] = temp;
}
String reversed = new String(chars);

// 使用 StringBuilder（推荐）
String reversed = new StringBuilder(s).reverse().toString();
```

#### Python
```python
# 切片反转（最简洁）
reversed = s[::-1]

# 使用 reversed()
reversed = ''.join(reversed(s))

# 手动反转
chars = list(s)
left, right = 0, len(s) - 1
while left < right:
    chars[left], chars[right] = chars[right], chars[left]
    left += 1
    right -= 1
reversed = ''.join(chars)
```

---

### 1.2 字符串遍历

#### Java
```java
// 索引遍历
for (int i = 0; i < s.length(); i++) {
    char c = s.charAt(i);
}

// 转 char 数组遍历
for (char c : s.toCharArray()) {
    // 处理 c
}

// 转列表遍历（需要先转数组再转列表）
List<Character> list = new ArrayList<>();
for (char c : s.toCharArray()) {
    list.add(c);
}
```

#### Python
```python
# 直接遍历
for c in s:
    # 处理 c

# 带索引遍历
for i, c in enumerate(s):
    # 处理

# 转列表
chars = list(s)
```

---

## 2. 双指针模板

### 2.1 相向双指针（回文判断）

#### Java
```java
int left = 0, right = s.length() - 1;
while (left < right) {
    if (s.charAt(left) != s.charAt(right)) {
        return false;
    }
    left++;
    right--;
}
return true;
```

#### Python
```python
left, right = 0, len(s) - 1
while left < right:
    if s[left] != s[right]:
        return False
    left += 1
    right -= 1
return True
```

---

### 2.2 同向双指针（删除指定字符）

#### Java
```java
// 原地修改字符串（需要 char 数组）
char[] chars = s.toCharArray();
int slow = 0;
for (int fast = 0; fast < chars.length; fast++) {
    if (chars[fast] != target) {  // 保留条件
        chars[slow++] = chars[fast];
    }
}
return new String(chars, 0, slow);
```

#### Python
```python
# 使用列表推导式（Pythonic）
result = [c for c in s if c != target]
return ''.join(result)

# 手动实现
chars = list(s)
slow = 0
for fast in range(len(chars)):
    if chars[fast] != target:  # 保留条件
        chars[slow] = chars[fast]
        slow += 1
return ''.join(chars[:slow])
```

---

## 3. KMP 算法模板

### 3.1 构建 next 数组（前缀表）

**next[i]** 表示模式串 `[0, i]` 的最长相等前后缀长度。

#### Java
```java
/**
 * 构建 next 数组
 * @param pattern 模式串
 * @return next 数组
 */
private int[] buildNext(String pattern) {
    int n = pattern.length();
    int[] next = new int[n];

    // i 从 1 开始，next[0] = 0
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
```

#### Python
```python
def build_next(pattern: str) -> list:
    """构建 next 数组"""
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

### 3.2 KMP 匹配

#### Java
```java
/**
 * KMP 字符串匹配
 * @param text 文本串
 * @param pattern 模式串
 * @return 第一次出现的位置，不存在返回 -1
 */
public int strStr(String text, String pattern) {
    if (pattern.isEmpty()) return 0;

    int[] next = buildNext(pattern);

    for (int i = 0, j = 0; i < text.length(); i++) {
        // 不匹配，回退 j
        while (j > 0 && text.charAt(i) != pattern.charAt(j)) {
            j = next[j - 1];
        }

        // 匹配，j 前进
        if (text.charAt(i) == pattern.charAt(j)) {
            j++;
        }

        // 完全匹配
        if (j == pattern.length()) {
            return i - j + 1;
        }
    }

    return -1;
}
```

#### Python
```python
def str_str(text: str, pattern: str) -> int:
    """KMP 字符串匹配"""
    if not pattern:
        return 0

    next_arr = build_next(pattern)
    j = 0

    for i in range(len(text)):
        # 不匹配，回退 j
        while j > 0 and text[i] != pattern[j]:
            j = next_arr[j - 1]

        # 匹配，j 前进
        if text[i] == pattern[j]:
            j += 1

        # 完全匹配
        if j == len(pattern):
            return i - j + 1

    return -1
```

---

## 4. 子序列 DP 模板

### 4.1 最长公共子序列 (LCS)

**状态定义**：`dp[i][j]` 表示 `text1[0..i-1]` 和 `text2[0..j-1]` 的 LCS 长度

#### Java
```java
public int longestCommonSubsequence(String text1, String text2) {
    int m = text1.length(), n = text2.length();
    int[][] dp = new int[m + 1][n + 1];

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }

    return dp[m][n];
}
```

#### Python
```python
def longest_common_subsequence(text1: str, text2: str) -> int:
    m, n = len(text1), len(text2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i - 1] == text2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

    return dp[m][n]
```

---

### 4.2 编辑距离

**状态定义**：`dp[i][j]` 表示将 `word1[0..i-1]` 转换为 `word2[0..j-1]` 的最小操作数

#### Java
```java
public int minDistance(String word1, String word2) {
    int m = word1.length(), n = word2.length();
    int[][] dp = new int[m + 1][n + 1];

    // 初始化边界
    for (int i = 0; i <= m; i++) dp[i][0] = i;
    for (int j = 0; j <= n; j++) dp[0][j] = j;

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = Math.min(dp[i - 1][j - 1],    // 替换
                               Math.min(dp[i - 1][j],    // 删除
                                        dp[i][j - 1])) + 1;  // 插入
            }
        }
    }

    return dp[m][n];
}
```

#### Python
```python
def min_distance(word1: str, word2: str) -> int:
    m, n = len(word1), len(word2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    # 初始化边界
    for i in range(m + 1):
        dp[i][0] = i
    for j in range(n + 1):
        dp[0][j] = j

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if word1[i - 1] == word2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = min(dp[i - 1][j - 1],    # 替换
                           dp[i - 1][j],           # 删除
                           dp[i][j - 1]) + 1       # 插入

    return dp[m][n]
```

---

## 5. 常用技巧

### 5.1 判断回文串

#### Java
```java
// 简单回文
boolean isPalindrome(String s) {
    int left = 0, right = s.length() - 1;
    while (left < right) {
        if (s.charAt(left++) != s.charAt(right--)) {
            return false;
        }
    }
    return true;
}

// 忽略非字母数字，忽略大小写
boolean isPalindromeIgnoreCase(String s) {
    int left = 0, right = s.length() - 1;
    while (left < right) {
        while (left < right && !Character.isLetterOrDigit(s.charAt(left))) left++;
        while (left < right && !Character.isLetterOrDigit(s.charAt(right))) right--;
        if (Character.toLowerCase(s.charAt(left)) !=
            Character.toLowerCase(s.charAt(right))) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```

#### Python
```python
# 简单回文
def is_palindrome(s: str) -> bool:
    return s == s[::-1]

# 双指针版本
def is_palindrome(s: str) -> bool:
    left, right = 0, len(s) - 1
    while left < right:
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1
    return True

# 忽略非字母数字，忽略大小写
def is_palindrome_ignore(s: str) -> bool:
    left, right = 0, len(s) - 1
    while left < right:
        while left < right and not s[left].isalnum():
            left += 1
        while left < right and not s[right].isalnum():
            right -= 1
        if s[left].lower() != s[right].lower():
            return False
        left += 1
        right -= 1
    return True
```

---

### 5.2 字符串匹配（暴力）

#### Java
```java
public int strStrBruteForce(String haystack, String needle) {
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
```

#### Python
```python
def str_str_brute_force(haystack: str, needle: str) -> int:
    m, n = len(haystack), len(needle)

    for i in range(m - n + 1):
        for j in range(n):
            if haystack[i + j] != needle[j]:
                break
        else:
            return i  # 内层循环正常结束

    return -1
```

---

## 6. 复杂度总结

| 操作 | 时间复杂度 | 空间复杂度 |
|------|-----------|-----------|
| 字符串反转 | O(n) | O(n)（需要 char 数组） |
| 双指针遍历 | O(n) | O(1) |
| KMP 匹配 | O(m + n) | O(n)（next 数组） |
| 暴力匹配 | O(m × n) | O(1) |
| LCS DP | O(m × n) | O(m × n) |
| 编辑距离 DP | O(m × n) | O(m × n) |
