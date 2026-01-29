# 33. 搜索旋转排序数组

[中文](https://leetcode.cn/problems/search-in-rotated-sorted-array/) | [English](https://leetcode.com/problems/search-in-rotated-sorted-array/)

**难度**：Medium
**标签**：数组 | 二分查找

---

## 题目描述

整数数组 `nums` 按升序排列，数组中的值**互不相同**。

在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了**旋转**，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标从 0 开始计数）。

例如，`[0,1,2,4,5,6,7]` 在下标 `3` 处经旋转后可能变为 `[4,5,6,7,0,1,2]`。

给你**旋转后**的数组 `nums` 和一个整数 `target`，如果 `nums` 中存在这个目标值 `target`，则返回它的下标，否则返回 `-1`。

**示例 1**：
```
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```

**示例 2**：
```
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
```

**示例 3**：
```
输入：nums = [1], target = 0
输出：-1
```

---

## 解题思路

### 方法一：二分查找

**思路**：
- 旋转后的数组由两个有序部分组成
- 每次二分时，至少有一半是有序的
- 判断 `target` 在哪一半，然后继续二分

**关键判断**：
1. 如果 `nums[left] <= nums[mid]`：左半部分有序
   - 如果 `nums[left] <= target < nums[mid]`：在左半部分
   - 否则在右半部分
2. 否则：右半部分有序
   - 如果 `nums[mid] < target <= nums[right]`：在右半部分
   - 否则在左半部分

**复杂度**：
- 时间：O(log n)
- 空间：O(1)

**图解**：
```
nums = [4,5,6,7,0,1,2], target = 0

left=0, right=6, mid=3
[4,5,6,7,0,1,2]
         ^
nums[0]=4 <= nums[3]=7, 左半有序
target=0 不在 [4,7] 范围内，right=2

left=0, right=2, mid=1
[4,5,6,7,0,1,2]
      ^
nums[0]=4 <= nums[1]=5, 左半有序
target=0 不在 [4,5] 范围内，right=0

left=0, right=0, mid=0
[4,5,6,7,0,1,2]
 ^
nums[0]=4 != 0, right=-1

left > right, 未找到
```

---

## 代码实现

### Java

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (nums[mid] == target) {
                return mid;
            }

            // 判断哪半部分是有序的
            if (nums[left] <= nums[mid]) {
                // 左半部分有序
                if (nums[left] <= target && target < nums[mid]) {
                    right = mid - 1;  // 在左半部分
                } else {
                    left = mid + 1;   // 在右半部分
                }
            } else {
                // 右半部分有序
                if (nums[mid] < target && target <= nums[right]) {
                    left = mid + 1;   // 在右半部分
                } else {
                    right = mid - 1;  // 在左半部分
                }
            }
        }

        return -1;
    }
}
```

### Python

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = left + (right - left) // 2

            if nums[mid] == target:
                return mid

            # 判断哪半部分是有序的
            if nums[left] <= nums[mid]:
                # 左半部分有序
                if nums[left] <= target < nums[mid]:
                    right = mid - 1  # 在左半部分
                else:
                    left = mid + 1   # 在右半部分
            else:
                # 右半部分有序
                if nums[mid] < target <= nums[right]:
                    left = mid + 1   # 在右半部分
                else:
                    right = mid - 1  # 在左半部分

        return -1
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 有序判断 | `nums[left] <= nums[mid]` 判断左半是否有序 |
| 范围判断 | `<=` 和 `<` 的正确使用，避免遗漏边界 |
| 二分选择 | 根据有序半部分判断 target 在哪边 |
| 时间复杂度 | O(log n) |

---

## 变种/延伸

- **变种1**：数组中可能有重复元素
- **变种2**：寻找旋转点（最小元素的位置）
- **变种3**：在旋转数组中查找最小值
