# 11. 盛最多水的容器

[中文](https://leetcode.cn/problems/container-with-most-water/) | [English](https://leetcode.com/problems/container-with-most-water/)

**难度**：Medium
**标签**：数组 | 双指针 | 贪心

---

## 题目描述

给定一个长度为 `n` 的整数数组 `height`。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])`。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

**示例 1**：
```
输入：[1,8,6,2,5,4,8,3,7]
输出：49
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

**示例 2**：
```
输入：height = [1,1]
输出：1
```

---

## 解题思路

### 方法一：双指针（相向指针）+ 贪心

**思路**：
- 容积 = `min(height[left], height[right]) * (right - left)`
- 使用相向双指针，从两端向中间移动
- **关键决策**：每次移动**较矮的那一边**

**为什么移动较矮的一边？**
```
当前容积由较矮的边决定：
- 如果移动较高的边，高度只能不变或更小，宽度也减小
- 如果移动较矮的边，可能遇到更高的边，虽然宽度减小，但高度可能增加
```

**复杂度**：
- 时间：O(n)
- 空间：O(1)

**图解**：
```
初始：
     |
|    |
|    |     |
|    |  |  |
|  | |  |  |
1  8  6  2  5 ...
L           R

min(1,5) * 4 = 4，左边更矮，L++

     |
|    |
|    |     |
|    |  |  |
|  | |  |  |
1  8  6  2  5 ...
   L        R

min(8,5) * 3 = 15，右边更矮，R++
...
```

---

## 代码实现

### Java

```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0, right = height.length - 1;
        int maxArea = 0;

        while (left < right) {
            // 计算当前容积
            int h = Math.min(height[left], height[right]);
            int width = right - left;
            int area = h * width;
            maxArea = Math.max(maxArea, area);

            // 移动较矮的一边
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }

        return maxArea;
    }
}
```

### Python

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        left, right = 0, len(height) - 1
        max_area = 0

        while left < right:
            # 计算当前容积
            h = min(height[left], height[right])
            width = right - left
            area = h * width
            max_area = max(max_area, area)

            # 移动较矮的一边
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1

        return max_area
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 容积公式 | `min(h[left], h[right]) * (right - left)` |
| 指针移动策略 | 总是移动较矮的那一边 |
| 贪心思想 | 移动较矮的一边才有机会获得更大的容积 |
| 边界条件 | `left < right`，不能等于 |

---

## 变种/延伸

- **变种1**：如果有多个相同最大值，返回任意一个
- **变种2**：如果要求返回构成最大容器的两个索引
- **变种3**：接雨水问题（lc-42），更难的版本
