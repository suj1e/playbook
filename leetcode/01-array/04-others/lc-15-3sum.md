# 15. 三数之和

[中文](https://leetcode.cn/problems/3sum/) | [English](https://leetcode.com/problems/3sum/)

**难度**：Medium
**标签**：数组 | 双指针 | 排序

---

## 题目描述

给你一个整数数组 `nums`，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k`，同时还满足 `nums[i] + nums[j] + nums[k] == 0`。

请你返回所有和为 `0` 且不重复的三元组。

**注意**：答案中不可以包含重复的三元组。

**示例 1**：
```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = -1 + 0 + 1 = 0
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0
不同的三元组是 [-1,0,1] 和 [-1,-1,2]
注意，输出的顺序和三元组的顺序并不重要
```

**示例 2**：
```
输入：nums = [0,0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0
```

---

## 解题思路

### 方法一：排序 + 双指针

**思路**：
1. 先排序数组（便于去重和使用双指针）
2. 固定第一个数 `nums[i]`
3. 用双指针 `left` 和 `right` 找另外两个数
4. 去重：跳过重复的元素

**复杂度**：
- 时间：O(n²)
- 空间：O(1)（不考虑排序的空间）

**步骤**：
1. 排序数组
2. 遍历 `i` 从 0 到 n-3：
   - 如果 `nums[i] > 0`：后面的数都 > 0，不可能和为 0，break
   - 跳过重复的 `nums[i]`
   - `left = i + 1`，`right = n - 1`
   - 双指针查找和为 `-nums[i]` 的两个数：
     - `sum = nums[left] + nums[right]`
     - 如果 `sum == -nums[i]`：找到答案
     - 如果 `sum < -nums[i]`：`left++`
     - 如果 `sum > -nums[i]`：`right--`
     - 跳过重复元素

**图解**：
```
nums = [-1,0,1,2,-1,-4]
排序后: [-4,-1,-1,0,1,2]

i=0, nums[i]=-4
left=1, right=5, sum=-1+2=1 > -4, right--
...
sum=1 > -4, right=1, left>=right, 结束

i=1, nums[i]=-1
left=2, right=5, sum=-1+2=1 == 1, 找到 [-1,-1,2]
left=3, right=4, sum=0+1=1 == 1, 找到 [-1,0,1]
...
```

---

## 代码实现

### Java

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);

        for (int i = 0; i < nums.length - 2; i++) {
            // 如果最小的数都大于 0，不可能和为 0
            if (nums[i] > 0) break;

            // 跳过重复的第一个数
            if (i > 0 && nums[i] == nums[i - 1]) continue;

            int left = i + 1, right = nums.length - 1;
            int target = -nums[i];

            while (left < right) {
                int sum = nums[left] + nums[right];

                if (sum == target) {
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));

                    // 跳过重复的 left
                    while (left < right && nums[left] == nums[left + 1]) {
                        left++;
                    }
                    // 跳过重复的 right
                    while (left < right && nums[right] == nums[right - 1]) {
                        right--;
                    }

                    left++;
                    right--;
                } else if (sum < target) {
                    left++;
                } else {
                    right--;
                }
            }
        }

        return result;
    }
}
```

### Python

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        result = []
        nums.sort()

        for i in range(len(nums) - 2):
            # 如果最小的数都大于 0，不可能和为 0
            if nums[i] > 0:
                break

            # 跳过重复的第一个数
            if i > 0 and nums[i] == nums[i - 1]:
                continue

            left, right = i + 1, len(nums) - 1
            target = -nums[i]

            while left < right:
                total = nums[left] + nums[right]

                if total == target:
                    result.append([nums[i], nums[left], nums[right]])

                    # 跳过重复的 left
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    # 跳过重复的 right
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1

                    left += 1
                    right -= 1
                elif total < target:
                    left += 1
                else:
                    right -= 1

        return result
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 排序 | 便于去重和使用双指针 |
| 固定一个数 | 将三数之和转为两数之和 |
| 双指针 | 在有序数组中查找两数之和 |
| 去重 | 跳过重复元素 |
| 提前终止 | `nums[i] > 0` 时 break |

---

## 去重策略

1. **第一个数去重**：`if (i > 0 && nums[i] == nums[i - 1]) continue`
2. **第二个数去重**：找到答案后，跳过相同的 `left`
3. **第三个数去重**：找到答案后，跳过相同的 `right`

---

## 变种/延伸

- **变种1**：最接近 target 的三数之和
- **变种2**：四数之和（固定两个数，再用双指针）
- **变种3**：三数之和小于 target 的组合数
