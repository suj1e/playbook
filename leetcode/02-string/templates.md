# 字符串代码模板

字符串处理在力扣中非常常见，掌握这些模板可以提高解题效率。

---

## 1. 双指针模板

### 1.1 回文判断

**适用场景**：判断回文串

```
字符串: "a b c b a"
         ^       ^
        left    right

比较: s[left] == s[right]
      a    ==    a  ✓

移动后: "a b c b a"
            ^ ^
          left right
```

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

## 2. KMP 算法模板

### 2.1 构建 next 数组

```
模式串: "a b a b c"
索引:    0 1 2 3 4

next:   [0,0,1,2,0]
含义:   最长相等前后缀长度

i=2, j=0: "ab" + "a" -> next[2]=1
i=3, j=1: "aba" + "b" -> next[3]=2
i=4, j=2: "abab" + "c" -> 不匹配, j=next[1]=0
```

#### Java
```java
private int[] buildNext(String pattern) {
    int n = pattern.length();
    int[] next = new int[n];

    for (int i = 1, j = 0; i < n; i++) {
        while (j > 0 && pattern.charAt(i) != pattern.charAt(j)) {
            j = next[j - 1];  // 回退 j
        }
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
    n = len(pattern)
    next_arr = [0] * n
    j = 0

    for i in range(1, n):
        while j > 0 and pattern[i] != pattern[j]:
            j = next_arr[j - 1]  # 回退 j
        if pattern[i] == pattern[j]:
            j += 1
        next_arr[i] = j

    return next_arr
```

---

### 2.2 KMP 匹配

```
文本串:   "a b a b a b c"
模式串:     "a b a b c"
匹配过程:   ↓ ↓ ↓ ↓ ✗
          回退: j = next[3] = 2
          继续匹配...
```

#### Java
```java
public int strStr(String text, String pattern) {
    if (pattern.isEmpty()) return 0;

    int[] next = buildNext(pattern);

    for (int i = 0, j = 0; i < text.length(); i++) {
        while (j > 0 && text.charAt(i) != pattern.charAt(j)) {
            j = next[j - 1];  // 不匹配时回退
        }
        if (text.charAt(i) == pattern.charAt(j)) {
            j++;  // 匹配则前进
        }
        if (j == pattern.length()) {
            return i - j + 1;  // 完全匹配
        }
    }
    return -1;
}
```

#### Python
```python
def str_str(text: str, pattern: str) -> int:
    if not pattern:
        return 0

    next_arr = build_next(pattern)
    j = 0

    for i in range(len(text)):
        while j > 0 and text[i] != pattern[j]:
            j = next_arr[j - 1]  # 不匹配时回退
        if text[i] == pattern[j]:
            j += 1
        if j == len(pattern):
            return i - j + 1  # 完全匹配

    return -1
```

---

## 3. 常用技巧

### 3.1 字符串反转

```
原字符串: "a b c d e"
反转后:   "e d c b a"

切片法: s[::-1] -> 反转
```

#### Java
```java
// 使用 StringBuilder
String reversed = new StringBuilder(s).reverse().toString();

// 手动反转（双指针）
char[] chars = s.toCharArray();
int left = 0, right = chars.length - 1;
while (left < right) {
    char temp = chars[left];
    chars[left++] = chars[right];
    chars[right--] = temp;
}
String reversed = new String(chars);
```

#### Python
```python
# 切片反转（最简洁）
reversed = s[::-1]

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

### 3.2 判断回文串（忽略非字母数字）

```
"A man, a plan, a canal: Panama"
  ^                       ^
过滤后: "amanaplanacanalpanama" -> 回文 ✓

步骤: 1. 跳过非字母数字
     2. 统一转为小写
     3. 双指针比较
```

#### Java
```java
int left = 0, right = s.length() - 1;
while (left < right) {
    // 跳过非字母数字
    while (left < right && !Character.isLetterOrDigit(s.charAt(left))) left++;
    while (left < right && !Character.isLetterOrDigit(s.charAt(right))) right--;

    // 统一小写后比较
    if (Character.toLowerCase(s.charAt(left)) !=
        Character.toLowerCase(s.charAt(right))) {
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
    # 跳过非字母数字
    while left < right and not s[left].isalnum():
        left += 1
    while left < right and not s[right].isalnum():
        right -= 1

    # 统一小写后比较
    if s[left].lower() != s[right].lower():
        return False
    left += 1
    right -= 1
return True
```

---

## 4. 复杂度总结

| 操作 | 时间复杂度 | 空间复杂度 |
|------|-----------|-----------|
| 字符串反转 | O(n) | O(n) |
| 双指针遍历 | O(n) | O(1) |
| KMP 匹配 | O(m + n) | O(n) |
| 暴力匹配 | O(m × n) | O(1) |
