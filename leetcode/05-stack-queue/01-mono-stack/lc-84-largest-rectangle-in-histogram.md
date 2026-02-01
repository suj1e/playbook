# 84. Largest Rectangle in Histogram / 柱状图中最大的矩形

[中文链接](https://leetcode.cn/problems/largest-rectangle-in-histogram/) | [English](https://leetcode.com/problems/largest-rectangle-in-histogram/)

**难度**：Hard
**标签**：栈 | 数组 | 单调栈
**相似题目**：[lc-85](./) | [lc-42](./) | [lc-221](./)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

**示例 1**：
```
输入：heights = [2,1,5,6,2,3]
输出：10
解释：最大的矩形为图中红色区域，面积为 10
```

**示例 2**：
```
输入：heights = [2,4]
输出：4
```

**约束条件**：
- 1 <= heights.length <= 10⁵
- 0 <= heights[i] <= 10⁴

---

## 解题思路

### 方法一：暴力枚举

**思路**：
枚举每个柱子作为高度，向左右扩展直到遇到更矮的柱子。

**复杂度**：
- 时间：O(n²)
- 空间：O(1)

---

### 方法二：单调栈（推荐）

**思路**：
使用单调递增栈，找到每个柱子能向左右扩展的最大范围。

**核心思想**：
- 栈中保持递增序列
- 当遇到更矮的柱子时，栈顶柱子的"右边界"确定了
- 柱子能扩展到的右边界就是当前柱子的位置
- 柱子能扩展到的左边界是栈中下一个元素的位置

**复杂度**：
- 时间：O(n)
- 空间：O(n)

**步骤**：
1. 在数组末尾添加 0，确保所有柱子都会被处理
2. 使用单调递增栈（存储索引）
3. 当前高度 < 栈顶高度时：
   - 弹出栈顶，计算以该高度为矩形的面积
   - 宽度 = 当前索引 - 新栈顶索引 - 1
4. 当前索引入栈

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：暴力 O(n²)
    public int largestRectangleArea(int[] heights) {
        int maxArea = 0;
        int n = heights.length;

        for (int i = 0; i < n; i++) {
            int height = heights[i];
            int left = i, right = i;

            // 向左扩展
            while (left >= 0 && heights[left] >= height) {
                left--;
            }

            // 向右扩展
            while (right < n && heights[right] >= height) {
                right++;
            }

            int width = right - left - 1;
            maxArea = Math.max(maxArea, height * width);
        }

        return maxArea;
    }

    // 方法二：单调栈 O(n)
    public int largestRectangleArea(int[] heights) {
        // 添加哨兵 0，确保所有柱子都会被处理
        int[] newHeights = new int[heights.length + 1];
        System.arraycopy(heights, 0, newHeights, 0, heights.length);
        newHeights[heights.length] = 0;

        Deque<Integer> stack = new ArrayDeque<>();  // 单调递增栈
        int maxArea = 0;

        for (int i = 0; i < newHeights.length; i++) {
            // 当前柱子更矮，处理栈中更高的柱子
            while (!stack.isEmpty() && newHeights[i] < newHeights[stack.peek()]) {
                int height = newHeights[stack.pop()];
                // 宽度 = 当前位置 - 新栈顶位置 - 1
                // 如果栈为空，说明可以扩展到最左边，宽度为 i
                int width = stack.isEmpty() ? i : i - stack.peek() - 1;
                maxArea = Math.max(maxArea, height * width);
            }
            stack.push(i);
        }

        return maxArea;
    }
}
```

### Python

```python
from typing import List

class Solution:
    # 方法一：暴力 O(n²)
    def largestRectangleArea(self, heights: List[int]) -> int:
        max_area = 0
        n = len(heights)

        for i in range(n):
            height = heights[i]
            left = right = i

            while left >= 0 and heights[left] >= height:
                left -= 1

            while right < n and heights[right] >= height:
                right += 1

            width = right - left - 1
            max_area = max(max_area, height * width)

        return max_area

    # 方法二：单调栈 O(n)
    def largestRectangleArea(self, heights: List[int]) -> int:
        # 添加哨兵 0
        heights = heights + [0]
        stack = []  # 单调递增栈
        max_area = 0

        for i, h in enumerate(heights):
            while stack and h < heights[stack[-1]]:
                height = heights[stack.pop()]
                width = i if not stack else i - stack[-1] - 1
                max_area = max(max_area, height * width)
            stack.append(i)

        return max_area
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 单调递增栈 | 栈中柱子高度递增，新柱子更矮时触发计算 |
| 哨兵 0 | 末尾添加 0，确保所有柱子都被处理 |
| 宽度计算 | right - left - 1，左右边界是不包含的 |
| 左边界 | 弹出后栈顶元素就是左边界 |
| 右边界 | 当前位置就是右边界 |
| 面积公式 | 高度 × (右边界 - 左边界 - 1) |

---

## 图解

```
heights = [2, 1, 5, 6, 2, 3]

添加哨兵: [2, 1, 5, 6, 2, 3, 0]

单调递增栈（存索引）:

i=0, h=2: stack=[0]
i=1, h=1: 1<2, 弹出0
  高度2, 左边界=-1, 右边界=1, 宽度=1, 面积=2
  stack=[1]
i=2, h=5: stack=[1,2]
i=3, h=6: stack=[1,2,3]
i=4, h=2: 2<6, 弹出3
  高度6, 左边界=2, 右边界=4, 宽度=1, 面积=6
  2<5, 弹出2
  高度5, 左边界=1, 右边界=4, 宽度=2, 面积=10 ← 最大
  stack=[1,4]
i=5, h=3: stack=[1,4,5]
i=6, h=0: 0<3, 弹出5
  高度3, 左边界=4, 右边界=6, 宽度=1, 面积=3
  0<2, 弹出4
  高度2, 左边界=1, 右边界=6, 宽度=4, 面积=8
  0<1, 弹出1
  高度1, 左边界=-1, 右边界=6, 宽度=6, 面积=6
  stack=[6]

最大面积: 10
对应矩形: 高度5, 宽度2 (索引2-3的两个柱子)
```

---

## 变种/延伸

### 变种1：最大矩形（二维）
- **题目**：[lc-85 最大矩形](./)
- **变化**：给定二维 0-1 矩阵，找只包含 1 的最大矩形
- **解法**：每一行作为柱状图，用本题方法

### 变种2：接雨水
- **题目**：[lc-42 接雨水](./)
- **变化**：计算能接多少雨水
- **解法**：单调递减栈

### 变种3：最大正方形
- **题目**：[lc-221 最大正方形](./)
- **变化**：找只包含 1 的最大正方形
- **解法**：动态规划

### 进阶思考

1. **为什么末尾添加 0？**
   - 确保所有柱子都会被弹出处理
   - 没有这个哨兵，最后留在栈中的柱子不会被计算

2. **为什么是单调递增栈？**
   - 我们要找每个柱子向左右能扩展多远
   - 遇到更矮的柱子时，说明栈顶柱子的"右边界"找到了

3. **宽度为什么是 right - left - 1？**
   - left 和 right 是边界，不包含在矩形内
   - 例如：heights = [2, 1, 2]，中间柱子高度为 1
   - 左边界 index = -1，右边界 index = 3
   - 宽度 = 3 - (-1) - 1 = 3，覆盖全部三个柱子

4. **如果所有柱子高度相同？**
   - 每次都会处理之前的柱子
   - 最后一个柱子（哨兵0触发）会计算整个矩形的面积

5. **如何同时返回最大矩形的左上角和右下角？**
   - 在计算最大面积时，记录 left 和 right
   - 返回 (left + 1, height_index, right - 1, height_index)
