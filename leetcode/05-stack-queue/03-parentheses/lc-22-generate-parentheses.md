# 22. Generate Parentheses / 括号生成

[中文链接](https://leetcode.cn/problems/generate-parentheses/) | [English](https://leetcode.com/problems/generate-parentheses/)

**难度**：Medium
**标签**：字符串 | 回溯算法
**相似题目**：[lc-20](./lc-20-valid-parentheses.md) | [lc-17](./) | [lc-301](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

数字 `n` 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

**示例 1**：
```
输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]
```

**示例 2**：
```
输入：n = 1
输出：["()"]
```

**约束条件**：
- 1 <= n <= 8

---

## 解题思路

### 方法一：回溯（推荐）

**思路**：
递归生成括号，关键保证生成的括号始终有效。

**有效括号的性质**：
1. 左括号数量 `<=` n
2. 右括号数量 `<=` 左括号数量

**复杂度**：
- 时间：O(4ⁿ/√n) - 卡塔兰数
- 空间：O(n)

**步骤**：
1. 使用回溯递归生成字符串
2. 添加左括号：如果左括号数量 < n
3. 添加右括号：如果右括号数量 < 左括号数量
4. 当字符串长度为 2n 时，加入结果

---

### 方法二：动态规划

**思路**：
用 f(n) 表示 n 对括号的所有有效组合。

**递推关系**：
```
f(n) = "(" + f(i) + ")" + f(n-1-i), 其中 0 <= i < n
```

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：回溯
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
        backtrack(result, new StringBuilder(), 0, 0, n);
        return result;
    }

    private void backtrack(List<String> result, StringBuilder sb, int open, int close, int n) {
        // 基础情况：生成 2n 个字符
        if (sb.length() == 2 * n) {
            result.add(sb.toString());
            return;
        }

        // 添加左括号（只要左括号数量 < n）
        if (open < n) {
            sb.append('(');
            backtrack(result, sb, open + 1, close, n);
            sb.deleteCharAt(sb.length() - 1);  // 回溯
        }

        // 添加右括号（只要右括号数量 < 左括号数量）
        if (close < open) {
            sb.append(')');
            backtrack(result, sb, open, close + 1, n);
            sb.deleteCharAt(sb.length() - 1);  // 回溯
        }
    }

    // 方法二：动态规划
    public List<String> generateParenthesis(int n) {
        List<List<String>> dp = new ArrayList<>();
        dp.add(Arrays.asList(""));  // dp[0] = [""]

        for (int i = 1; i <= n; i++) {
            List<String> current = new ArrayList<>();
            for (int j = 0; j < i; j++) {
                // "(" + dp[j] + ")" + dp[i-1-j]
                for (String left : dp.get(j)) {
                    for (String right : dp.get(i - 1 - j)) {
                        current.add("(" + left + ")" + right);
                    }
                }
            }
            dp.add(current);
        }

        return dp.get(n);
    }
}
```

### Python

```python
from typing import List

class Solution:
    # 方法一：回溯
    def generateParenthesis(self, n: int) -> List[str]:
        result = []

        def backtrack(s: str, open: int, close: int):
            if len(s) == 2 * n:
                result.append(s)
                return

            if open < n:
                backtrack(s + '(', open + 1, close)

            if close < open:
                backtrack(s + ')', open, close + 1)

        backtrack('', 0, 0)
        return result

    # 方法二：动态规划
    def generateParenthesis(self, n: int) -> List[str]:
        dp = [[] for _ in range(n + 1)]
        dp[0] = ['']

        for i in range(1, n + 1):
            for j in range(i):
                # "(" + dp[j] + ")" + dp[i-1-j]
                for left in dp[j]:
                    for right in dp[i - 1 - j]:
                        dp[i].append('(' + left + ')' + right)

        return dp[n]
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 左括号限制 | open < n 才能添加左括号 |
| 右括号限制 | close < open 才能添加右括号 |
| 回溯还原 | 每次递归返回后要撤销选择 |
| 卡塔兰数 | 结果数量是第 n 个卡塔兰数 |
| DP 思想 | 可以看作：() + 所有可能的内层和外层组合 |

---

## 图解

```
n = 2

回溯树:
                      ""
                    /     \
                 "("       (无效，close >= open)
                /   \
           "(("   "()"        ← 这两个都有效
           /  \      \
        "(()" "()()"  "()"  (无效，close >= open)
          |
         "(())"

结果: ["(())", "()()"]

---

n = 3 的所有组合:

1. ((())) - 三层嵌套
2. (()()) - 两层嵌套 + 内层并列
3. (())() - 两层嵌套 + 外层并列
4. ()(()) - 外层并列 + 两层嵌套
5. ()()() - 三层并列

卡塔兰数 C(3) = 5
```

---

## 卡塔兰数

第 n 个卡塔兰数表示 n 对括号的有效组合数量：

```
C(0) = 1
C(1) = 1
C(2) = 2
C(3) = 5
C(4) = 14
C(5) = 42
...

公式: C(n) = (2n)! / ((n+1)! * n!)
     = C(2n, n) / (n+1)
```

---

## 变种/延伸

### 变种1：有效的括号
- **题目**：[lc-20 有效的括号](./lc-20-valid-parentheses.md)
- **变化**：判断给定的括号字符串是否有效
- **解法**：栈

### 变种2：删除无效括号
- **题目**：[lc-301 删除无效括号](./)
- **变化**：删除最少括号使字符串有效
- **解法**：BFS/回溯

### 变种3：不同的括号生成
- **变化**：使用多种括号 ()、[]、{}
- **解法**：回溯时记录括号类型

### 进阶思考

1. **为什么 close < open 才能添加右括号？**
   - 保证每个右括号都有对应的左括号
   - 如果 close >= open，说明右括号比左括号多，无效

2. **如何证明时间复杂度是卡塔兰数？**
   - 每个有效括号序列对应唯一的生成路径
   - 有效括号的数量就是卡塔兰数

3. **如何用迭代方式实现？**
   - 动态规划就是迭代方式
   - 从 dp[0] 开始逐步构建 dp[n]

4. **如果有多对括号嵌套，如何处理？**
   - 回溯法自动处理
   - DP 的递推关系也考虑了嵌套

5. **如何打印所有可能的组合？**
   - 本题就是在生成所有组合
   - 可以调整遍历顺序改变输出顺序
