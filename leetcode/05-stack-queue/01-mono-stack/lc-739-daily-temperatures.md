# 739. Daily Temperatures / 每日温度

[中文链接](https://leetcode.cn/problems/daily-temperatures/) | [English](https://leetcode.com/problems/daily-temperatures/)

**难度**：Medium
**标签**：栈 | 数组 | 单调栈
**相似题目**：[lc-496](./lc-496-next-greater-element-i.md) | [lc-503](./) | [lc-901](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给定一个整数数组 `temperatures` ，表示每天的温度，返回一个数组 `answer` ，其中 `answer[i]` 是指对于第 i 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 0 来代替。

**示例 1**：
```
输入: temperatures = [73,74,75,71,69,72,76,73]
输出: [1,1,4,2,1,1,0,0]
```

**示例 2**：
```
输入: temperatures = [30,40,50,60]
输出: [1,1,1,0]
```

**示例 3**：
```
输入: temperatures = [30,60,90]
输出: [1,1,0]
```

**约束条件**：
- 1 <= temperatures.length <= 10⁵
- 30 <= temperatures[i] <= 100

---

## 解题思路

### 方法一：暴力枚举

**思路**：
对于每个元素，向后找第一个比它大的元素。

**复杂度**：
- 时间：O(n²)
- 空间：O(1)

---

### 方法二：单调栈（推荐）

**思路**：
使用单调递减栈（存储索引），栈中元素对应的温度从底到顶递减。

**核心思想**：
- 当遇到更高温度时，弹出栈中所有比当前温度小的元素
- 这些被弹出的元素找到了它们的"下一个更高温度"

**复杂度**：
- 时间：O(n)
- 空间：O(n)

**步骤**：
1. 创建单调递减栈（存储索引）
2. 遍历温度数组
3. 当前温度 > 栈顶温度时：
   - 弹出栈顶索引，计算天数差
   - 继续比较，直到栈为空或当前温度 <= 栈顶温度
4. 当前索引入栈

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：暴力 O(n²)
    public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] answer = new int[n];

        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (temperatures[j] > temperatures[i]) {
                    answer[i] = j - i;
                    break;
                }
            }
        }
        return answer;
    }

    // 方法二：单调栈 O(n)
    public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] answer = new int[n];
        Deque<Integer> stack = new ArrayDeque<>();  // 单调递减栈（存索引）

        for (int i = 0; i < n; i++) {
            // 当前温度高于栈顶，栈顶元素找到答案
            while (!stack.isEmpty() && temperatures[i] > temperatures[stack.peek()]) {
                int idx = stack.pop();
                answer[idx] = i - idx;
            }
            stack.push(i);
        }

        return answer;
    }
}
```

### Python

```python
from typing import List

class Solution:
    # 方法一：暴力 O(n²)
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        n = len(temperatures)
        answer = [0] * n

        for i in range(n):
            for j in range(i + 1, n):
                if temperatures[j] > temperatures[i]:
                    answer[i] = j - i
                    break

        return answer

    # 方法二：单调栈 O(n)
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        n = len(temperatures)
        answer = [0] * n
        stack = []  # 单调递减栈（存索引）

        for i, temp in enumerate(temperatures):
            # 当前温度高于栈顶
            while stack and temp > temperatures[stack[-1]]:
                idx = stack.pop()
                answer[idx] = i - idx
            stack.append(i)

        return answer
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 单调递减栈 | 栈中元素对应的温度从底到顶递减 |
| 存储索引 | 栈中存储索引而非温度值，方便计算天数差 |
| 弹出时机 | 当前温度 > 栈顶温度时，栈顶找到答案 |
| 批量处理 | 一次 while 可以处理多个栈顶元素 |
| 时间复杂度 | 每个元素最多入栈出栈各一次，O(n) |

---

## 图解

```
温度: [73, 74, 75, 71, 69, 72, 76, 73]
索引:   0    1    2    3    4    5    6    7

单调递减栈（存索引）:

i=0, temp=73:
stack: [0]           (73)

i=1, temp=74:
74 > 73, 弹出 0, answer[0] = 1 - 0 = 1
stack: [1]           (74)

i=2, temp=75:
75 > 74, 弹出 1, answer[1] = 2 - 1 = 1
stack: [2]           (75)

i=3, temp=71:
71 < 75, 直接入栈
stack: [2, 3]        (75, 71)

i=4, temp=69:
69 < 71, 直接入栈
stack: [2, 3, 4]     (75, 71, 69)

i=5, temp=72:
72 > 69, 弹出 4, answer[4] = 5 - 4 = 1
72 > 71, 弹出 3, answer[3] = 5 - 3 = 2
72 < 75, 停止
stack: [2, 5]        (75, 72)

i=6, temp=76:
76 > 72, 弹出 5, answer[5] = 6 - 5 = 1
76 > 75, 弹出 2, answer[2] = 6 - 2 = 4
stack: [6]           (76)

i=7, temp=73:
73 < 76, 直接入栈
stack: [6, 7]        (76, 73)

最终栈中元素没有更高温度，保持 0

answer: [1, 1, 4, 2, 1, 1, 0, 0]
```

---

## 变种/延伸

### 变种1：下一个更大元素 I
- **题目**：[lc-496 下一个更大元素 I](./lc-496-next-greater-element-i.md)
- **变化**：给定两个数组，在第二个数组中找第一个数组每个元素的下一个更大元素
- **解法**：单调栈 + 哈希表

### 变种2：下一个更大元素 II（循环数组）
- **题目**：[lc-503 下一个更大元素 II](./)
- **变化**：数组是循环的
- **解法**：遍历 2n 次，对 n 取模

### 变种3：柱状图中最大的矩形
- **题目**：[lc-84 柱状图中最大的矩形](./lc-84-largest-rectangle-in-histogram.md)
- **变化**：找最大矩形面积
- **解法**：单调递增栈

### 变种4：股票价格跨度
- **题目**：[lc-901 股票价格跨度](./)
- **变化**：连续小于或等于当前价格的天数
- **解法**：单调栈优化

### 进阶思考

1. **为什么是单调递减栈？**
   - 我们要找"下一个更高温度"
   - 递减栈保证：新元素 > 栈顶时，栈顶找到了答案

2. **如果栈中存温度值而非索引会怎样？**
   - 无法计算天数差（需要索引）
   - 且重复温度无法正确处理

3. **如何处理循环数组的问题？**
   - 遍历 2n 次
   - 索引 i % n 取模
   - 或者复制一份数组拼接在后面

4. **从后往前遍历可以吗？**
   - 可以，使用单调递增栈
   - 思路类似，只是遍历方向相反

5. **如何找到上一个更高温度？**
   - 从后往前遍历，使用单调递增栈
   - 或者正向遍历时，栈底元素就是"上一个更高温度"
