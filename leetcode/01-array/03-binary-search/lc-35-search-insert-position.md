# 35. 搜索插入位置

[中文](https://leetcode.cn/problems/search-insert-position/) | [English](https://leetcode.com/problems/search-insert-position/)

**难度**：Easy
**标签**：数组 | 二分查找

---

## 题目描述

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。

**示例 1**：
```
输入: [1,3,5,6], 5
输出: 2
```

**示例 2**：
```
输入: [1,3,5,6], 2
输出: 1
```

**示例 3**：
```
输入: [1,3,5,6], 7
输出: 4
```

**示例 4**：
```
输入: [1,3,5,6], 0
输出: 0
```

---

## 解题思路

### 方法一：二分查找

**思路**：
- 标准二分查找的变体
- 最后 `left` 的位置就是插入位置
- 循环结束时，`left` 指向第一个大于 `target` 的位置

**为什么 `left` 是插入位置？**
- 循环结束时 `left = right + 1`
- `left` 左侧的元素都 `< target`
- `left` 右侧的元素都 `> target`
- 所以 `left` 就是正确的插入位置

**复杂度**：
- 时间：O(log n)
- 空间：O(1)

**步骤**：
1. 初始化 `left = 0`，`right = n - 1`
2. 当 `left <= right` 时循环：
   - 计算 `mid = left + (right - left) / 2`
   - 如果 `nums[mid] == target`：返回 `mid`
   - 如果 `nums[mid] < target`：`left = mid + 1`
   - 如果 `nums[mid] > target`：`right = mid - 1`
3. 返回 `left`（插入位置）

**图解**：
```
nums = [1,3,5,6], target = 2

left=0, right=3, mid=1
[1,3,5,6]
    ^
nums[1]=3 > 2, right=0

left=0, right=0, mid=0
[1,3,5,6]
 ^
nums[0]=1 < 2, left=1

left=1 > right=0, 结束
返回 left=1，插入位置是 1
```

---

## 代码实现

### Java

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0, right = nums.length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        return left;  // 插入位置
    }
}
```

### Python

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = left + (right - left) // 2

            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1

        return left  # 插入位置
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 循环结束条件 | `left > right` |
| 返回值 | 返回 `left`，即插入位置 |
| 找到目标 | 直接返回 `mid` |
| 时间复杂度 | O(log n) |

---

## 变种/延伸

- **变种1**：如果有重复元素，返回第一个等于 target 的位置
- **变种2**：如果有重复元素，返回最后一个等于 target 的位置
- **变种3**：返回应该插入的最左位置
