# 167. 两数之和 II - 输入有序数组

[中文](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/) | [English](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

**难度**：Easy
**标签**：数组 | 双指针
**相似题目**：[lc-1](../04-others/lc-1-two-sum.md)

---

## 题目描述

给你一个下标从 1 开始的整数数组 `numbers`，该数组已按**非递减顺序排列**，请你从数组中找出满足相加之和等于目标数 `target` 的两个数。

你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。

返回这两个数的索引（下标从 1 开始）。

**示例 1**：
```
输入：numbers = [2,7,11,15], target = 9
输出：[1,2]
解释：2 与 7 之和等于目标数 9。因此 index1 = 1, index2 = 2。
```

**示例 2**：
```
输入：numbers = [2,3,4], target = 6
输出：[1,3]
```

---

## 解题思路

### 方法一：双指针（相向指针）

**思路**：
- 数组已排序，使用相向双指针
- `left` 指向开头，`right` 指向末尾
- 计算两数之和：
  - 如果等于 `target`：找到答案
  - 如果小于 `target`：需要更大的数，`left++`
  - 如果大于 `target`：需要更小的数，`right--`

**为什么这样做有效**：
- 数组有序，如果 `sum < target`，增大 `left` 会让 `sum` 变大
- 如果 `sum > target`，减小 `right` 会让 `sum` 变小
- 这样可以保证不遗漏可能的解

**复杂度**：
- 时间：O(n)
- 空间：O(1)

**图解**：
```
初始状态：
[2, 7, 11, 15] target = 9
 L           R

sum = 2 + 15 = 17 > 9, R--
[2, 7, 11, 15]
 L        R

sum = 2 + 11 = 13 > 9, R--
[2, 7, 11, 15]
 L     R

sum = 2 + 7 = 9 == target, 找到答案！
```

---

## 代码实现

### Java

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0, right = numbers.length - 1;

        while (left < right) {
            int sum = numbers[left] + numbers[right];

            if (sum == target) {
                // 题目要求下标从 1 开始
                return new int[]{left + 1, right + 1};
            } else if (sum < target) {
                left++;  // 需要更大的和
            } else {
                right--; // 需要更小的和
            }
        }

        return new int[]{-1, -1};  // 题目保证有解，这里不会执行
    }
}
```

### Python

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left, right = 0, len(numbers) - 1

        while left < right:
            total = numbers[left] + numbers[right]

            if total == target:
                # 题目要求下标从 1 开始
                return [left + 1, right + 1]
            elif total < target:
                left += 1  # 需要更大的和
            else:
                right -= 1  # 需要更小的和

        return [-1, -1]  # 题目保证有解
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 相向双指针 | 利用数组已排序的特性 |
| 指针移动方向 | `sum < target` 时 `left++`，`sum > target` 时 `right--` |
| 下标从 1 开始 | 返回结果时需要 `+1` |
| while 条件 | `left < right`，不能等于（同一元素不能重复使用） |

---

## 变种/延伸

- **变种1**：如果数组无序，需要先排序或使用哈希表
- **变种2**：如果可能有多个答案，需要返回所有可能
- **变种3**：如果要求最接近 target 的两数之和
