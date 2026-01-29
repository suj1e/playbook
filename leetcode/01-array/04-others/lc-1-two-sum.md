# 1. 两数之和

[中文](https://leetcode.cn/problems/two-sum/) | [English](https://leetcode.com/problems/two-sum/)

**难度**：Easy
**标签**：数组 | 哈希表

---

## 题目描述

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出**和为目标值** `target` 的那**两个**整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

**示例 1**：
```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2**：
```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3**：
```
输入：nums = [3,3], target = 6
输出：[0,1]
```

---

## 解题思路

### 方法一：哈希表

**思路**：
- 使用哈希表存储每个元素及其索引
- 遍历数组，对于每个元素 `nums[i]`：
  - 计算 `complement = target - nums[i]`
  - 检查 `complement` 是否已在哈希表中
  - 如果存在，返回答案
  - 否则，将当前元素加入哈希表

**为什么这样做有效？**
- 哈希表提供 O(1) 查找
- 边遍历边检查，避免了重复使用同一元素

**复杂度**：
- 时间：O(n)
- 空间：O(n)

**图解**：
```
nums = [2,7,11,15], target = 9

i=0: nums[0]=2, complement=7
     7 不在 map 中
     map = {2: 0}

i=1: nums[1]=7, complement=2
     2 在 map 中，索引为 0
     找到答案！返回 [0, 1]
```

---

## 代码实现

### Java

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];

            if (map.containsKey(complement)) {
                return new int[]{map.get(complement), i};
            }

            map.put(nums[i], i);
        }

        return new int[]{-1, -1};  // 题目保证有解
    }
}
```

### Python

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        num_to_index = {}

        for i, num in enumerate(nums):
            complement = target - num

            if complement in num_to_index:
                return [num_to_index[complement], i]

            num_to_index[num] = i

        return [-1, -1]  # 题目保证有解
```

---

### 方法二：暴力枚举（不推荐）

**复杂度**：
- 时间：O(n²)
- 空间：O(1)

#### Java

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }
        return new int[]{-1, -1};
    }
}
```

#### Python

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                if nums[i] + nums[j] == target:
                    return [i, j]
        return [-1, -1]
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 哈希表 | 存储已遍历元素及其索引 |
| 查找顺序 | 先查找 complement，再存入当前元素 |
| 避免重复 | 边遍历边检查，不会重复使用同一元素 |
| 时间复杂度 | O(n) |

---

## 方法对比

| 方法 | 时间 | 空间 | 说明 |
|------|------|------|------|
| 哈希表 | O(n) | O(n) | **推荐** |
| 暴力枚举 | O(n²) | O(1) | 不推荐 |

---

## 变种/延伸

- **变种1**：返回所有可能的组合（不限于两个数）
- **变种2**：数组已排序时，可以使用双指针（见 lc-167）
- **变种3**：找出三个数之和等于 target
