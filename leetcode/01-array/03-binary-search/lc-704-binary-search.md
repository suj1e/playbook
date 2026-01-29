# 704. 二分查找

[中文](https://leetcode.cn/problems/binary-search/) | [English](https://leetcode.com/problems/binary-search/)

**难度**：Easy
**标签**：数组 | 二分查找

---

## 题目描述

给定一个 `n` 个元素有序的（升序）整型数组 `nums` 和一个目标值 `target`，写一个函数搜索 `nums` 中的 `target`，如果目标值存在返回下标，否则返回 `-1`。

**示例 1**：
```
输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4
```

**示例 2**：
```
输入: nums = [-1,0,3,5,9,12], target = 2
输出: -1
解释: 2 不存在 nums 中因此返回 -1
```

---

## 解题思路

### 方法一：标准二分查找

**思路**：
- 利用数组有序特性，每次排除一半元素
- `left` 和 `right` 指针不断向中间收缩
- 比较 `nums[mid]` 与 `target` 决定搜索方向

**复杂度**：
- 时间：O(log n)
- 空间：O(1)

**步骤**：
1. 初始化 `left = 0`，`right = n - 1`
2. 当 `left <= right` 时循环：
   - 计算 `mid = left + (right - left) / 2`（防止溢出）
   - 如果 `nums[mid] == target`：返回 `mid`
   - 如果 `nums[mid] < target`：`left = mid + 1`
   - 如果 `nums[mid] > target`：`right = mid - 1`
3. 未找到返回 `-1`

**图解**：
```
nums = [-1,0,3,5,9,12], target = 9

left=0, right=5, mid=2
[-1,0,3,5,9,12]
       ^
nums[2]=3 < 9, left=3

left=3, right=5, mid=4
[-1,0,3,5,9,12]
          ^
nums[4]=9 == 9, 找到！返回 4
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
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
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
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1

        return -1
```

---

### 方法二：递归版本

#### Java

```java
class Solution {
    public int search(int[] nums, int target) {
        return binarySearch(nums, 0, nums.length - 1, target);
    }

    private int binarySearch(int[] nums, int left, int right, int target) {
        if (left > right) {
            return -1;
        }

        int mid = left + (right - left) / 2;

        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            return binarySearch(nums, mid + 1, right, target);
        } else {
            return binarySearch(nums, left, mid - 1, target);
        }
    }
}
```

#### Python

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        def binary_search(left, right):
            if left > right:
                return -1

            mid = left + (right - left) // 2

            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                return binary_search(mid + 1, right)
            else:
                return binary_search(left, mid - 1)

        return binary_search(0, len(nums) - 1)
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 循环条件 | `left <= right`（包含等于） |
| mid 计算 | `left + (right - left) / 2` 防止溢出 |
| 边界更新 | `left = mid + 1`，`right = mid - 1` |
| 时间复杂度 | O(log n) |

---

## 变种/延伸

- **变种1**：查找第一个等于 target 的位置
- **变种2**：查找最后一个等于 target 的位置
- **变种3**：查找第一个大于等于 target 的位置
- **变种4**：查找最后一个小于等于 target 的位置
