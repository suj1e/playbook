# 209. 长度最小的子数组

[中文](https://leetcode.cn/problems/minimum-size-subarray-sum/) | [English](https://leetcode.com/problems/minimum-size-subarray-sum/)

**难度**：Medium
**标签**：数组 | 滑动窗口 | 前缀和

---

## 题目描述

给定一个含有 `n` 个正整数的数组和一个正整数 `target`。

找出该数组中满足其和 ≥ `target` 的长度最小的**连续子数组** `[numsl, numsl+1, ..., numsr-1, numsr]`，并返回其长度。如果不存在符合条件的子数组，返回 `0`。

**示例 1**：
```
输入：target = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```

**示例 2**：
```
输入：target = 4, nums = [1,4,4]
输出：1
```

**示例 3**：
```
输入：target = 11, nums = [1,1,1,1,1,1,1,1]
输出：0
```

---

## 解题思路

### 方法一：滑动窗口（可变大小）

**思路**：
- 使用滑动窗口维护一个和 ≥ target 的窗口
- 窗口右边界不断扩展，加入元素
- 当窗口和 ≥ target 时，尝试收缩左边界，找到最小长度

**复杂度**：
- 时间：O(n)，每个元素最多被访问两次
- 空间：O(1)

**步骤**：
1. 初始化 `left = 0`，`minLen = ∞`，`windowSum = 0`
2. `right` 从 0 到 n-1 遍历：
   - 将 `nums[right]` 加入窗口
   - 当 `windowSum ≥ target` 时：
     - 更新 `minLen`
     - 收缩左边界，直到 `windowSum < target`
3. 返回 `minLen`（如果未更新过返回 0）

**图解**：
```
target = 7, nums = [2,3,1,2,4,3]

right=0: sum=2 < 7
right=1: sum=5 < 7
right=2: sum=6 < 7
right=3: sum=8 >= 7, 更新minLen=4, 收缩
         left=1: sum=6 < 7
right=4: sum=10 >= 7, 更新minLen=4, 收缩
         left=2: sum=7 >= 7, 更新minLen=3, 收缩
         left=3: sum=6 < 7
right=5: sum=9 >= 7, 更新minLen=3, 收缩
         left=4: sum=7 >= 7, 更新minLen=2, 收缩
         left=5: sum=3 < 7

最终 minLen = 2
```

---

## 代码实现

### Java

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int minLen = Integer.MAX_VALUE;
        int windowSum = 0;

        for (int right = 0; right < nums.length; right++) {
            // 进入窗口
            windowSum += nums[right];

            // 窗口和 >= target，尝试收缩
            while (windowSum >= target) {
                // 更新最小长度
                minLen = Math.min(minLen, right - left + 1);

                // 离开窗口
                windowSum -= nums[left];
                left++;
            }
        }

        return minLen == Integer.MAX_VALUE ? 0 : minLen;
    }
}
```

### Python

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        left = 0
        min_len = float('inf')
        window_sum = 0

        for right in range(len(nums)):
            # 进入窗口
            window_sum += nums[right]

            # 窗口和 >= target，尝试收缩
            while window_sum >= target:
                # 更新最小长度
                min_len = min(min_len, right - left + 1)

                # 离开窗口
                window_sum -= nums[left]
                left += 1

        return 0 if min_len == float('inf') else min_len
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 窗口扩张 | `right` 向右移动，不断加入元素 |
| 窗口收缩 | 当 `sum >= target` 时，`left` 向右移动 |
| 更新时机 | 每次收缩后更新最小长度 |
| 初始值 | `minLen` 设为 `MAX_VALUE` 或 `inf` |

---

## 变种/延伸

- **变种1**：求和等于 target 的最短子数组
- **变种2**：求和 ≤ target 的最长子数组
- **变种3**：使用前缀和 + 二分的方法（O(n log n)）
