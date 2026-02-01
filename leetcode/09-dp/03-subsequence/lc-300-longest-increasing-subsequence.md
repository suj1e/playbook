# 300. Longest Increasing Subsequence / 最长递增子序列

[中文链接](https://leetcode.cn/problems/longest-increasing-subsequence/) | [English](https://leetcode.com/problems/longest-increasing-subsequence/)

**难度**：Medium
**标签**：数组 | 二分查找 | 动态规划
**相似题目**：[lc-354](./) | [lc-673](./) | [lc[718](./)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

给你一个整数数组 `nums` ，找到其中最长严格递增子序列的长度。

子序列 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。

**示例 1**：
```
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，长度为 4。
```

**示例 2**：
```
输入：nums = [0,1,0,3,2,3]
输出：4
解释：最长递增子序列是 [0,1,2,3]，长度为 4。
```

**示例 3**：
```
输入：nums = [7,7,7,7,7,7,7]
输出：1
```

**约束条件**：
- 1 <= nums.length <= 2500
- -10⁴ <= nums[i] <= 10⁴

**进阶**：你能将算法的时间复杂度降低到 O(n log n) 吗?

---

## 解题思路

### 方法一：动态规划 O(n²)

**思路**：
dp[i] = 以 nums[i] 结尾的 LIS 长度

**复杂度**：
- 时间：O(n²)
- 空间：O(n)

**步骤**：
1. dp[i] 初始化为 1（自己构成长度 1）
2. 对于每个 i，向前找所有 j < i
3. 如果 nums[j] < nums[i]，dp[i] = max(dp[i], dp[j] + 1)

---

### 方法二：二分查找 O(n log n)

**思路**：
维护 tails 数组，tails[i] 表示长度为 i+1 的 LIS 的最小末尾元素。

**复杂度**：
- 时间：O(n log n)
- 空间：O(n)

**步骤**：
1. 遍历 nums
2. 用二分查找找到 nums[i] 在 tails 中的位置
3. 替换或追加到 tails

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：DP O(n²)
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];
        Arrays.fill(dp, 1);

        int maxLen = 1;
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            maxLen = Math.max(maxLen, dp[i]);
        }

        return maxLen;
    }

    // 方法二：二分查找 O(n log n)
    public int lengthOfLIS(int[] nums) {
        List<Integer> tails = new ArrayList<>();

        for (int num : nums) {
            // 二分查找第一个 >= num 的位置
            int left = 0, right = tails.size();
            while (left < right) {
                int mid = left + (right - left) / 2;
                if (tails.get(mid) < num) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }

            if (left == tails.size()) {
                tails.add(num);  // 追加
            } else {
                tails.set(left, num);  // 替换
            }
        }

        return tails.size();
    }
}
```

### Python

```python
from typing import List
import bisect

class Solution:
    # 方法一：DP O(n²)
    def lengthOfLIS(self, nums: List[int]) -> int:
        n = len(nums)
        dp = [1] * n

        for i in range(1, n):
            for j in range(i):
                if nums[j] < nums[i]:
                    dp[i] = max(dp[i], dp[j] + 1)

        return max(dp) if dp else 0

    # 方法二：二分查找 O(n log n)
    def lengthOfLIS(self, nums: List[int]) -> int:
        tails = []

        for num in nums:
            # 二分查找第一个 >= num 的位置
            pos = bisect.bisect_left(tails, num)
            if pos == len(tails):
                tails.append(num)
            else:
                tails[pos] = num

        return len(tails)
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 子序列 | 不要求连续，保持相对顺序 |
| dp[i] 定义 | 以 i 结尾的 LIS 长度 |
| 状态转移 | dp[i] = max(dp[j] + 1) for j < i and nums[j] < nums[i] |
| tails 数组 | tails[i] = 长度 i+1 的 LIS 最小末尾 |
| 二分查找 | 用 bisect_left 找第一个 >= num 的位置 |

---

## 图解

```
nums = [10, 9, 2, 5, 3, 7, 101, 18]

方法一：DP
dp = [1, 1, 1, 1, 1, 1, 1, 1]

i=1, num=9:  j=0: 10>9, 不更新
dp = [1, 1, 1, 1, 1, 1, 1, 1]

i=2, num=2:  j=0: 10>2, j=1: 9>2, 不更新
dp = [1, 1, 1, 1, 1, 1, 1, 1]

i=3, num=5:  j=0: 10>5, j=1: 9>5, j=2: 2<5, dp[3]=dp[2]+1=2
dp = [1, 1, 1, 2, 1, 1, 1, 1]

i=4, num=3:  j=2: 2<3, dp[4]=dp[2]+1=2
dp = [1, 1, 1, 2, 2, 1, 1, 1]

i=5, num=7:  j=3: 5<7, dp[5]=dp[3]+1=3
                j=4: 3<7, dp[5]=max(3, dp[4]+1=3) = 3
dp = [1, 1, 1, 2, 2, 3, 1, 1]

i=6, num=101: j=0..5: 都<101, dp[6]=dp[5]+1=4
dp = [1, 1, 1, 2, 2, 3, 4, 1]

i=7, num=18:  j=0..5: 都<18, 但 dp 最大是 3, dp[7]=dp[5]+1=4
dp = [1, 1, 1, 2, 2, 3, 4, 4]

maxLen = 4

---

方法二：二分查找
tails = []

num=10: tails = [10]
num=9:  tails = [9]     (9 < 10, 替换)
num=2:  tails = [2]     (2 < 9, 替换)
num=5:  tails = [2, 5]
num=3:  tails = [2, 3]  (3 < 5, 替换)
num=7:  tails = [2, 3, 7]
num=101: tails = [2, 3, 7, 101]
num=18:  tails = [2, 3, 7, 18] (18 < 101, 替换)

len(tails) = 4
```

---

## 变种/延伸

### 变种1：俄罗斯套娃信封问题
- **题目**：[lc-354 俄罗斯套娃信封问题](./)
- **变化**：信封嵌套，找最大嵌套数量
- **解法**：类似 LIS，但条件是宽高都严格递增

### 变种2：最长连续递增序列
- **题目**：[lc-674 最长连续递增序列](./)
- **变化**：子数组（连续）而非子序列
- **解法**：直接遍历，O(n) 时间

### 变种3：摆动序列
- **题目**：[lc[376 摆动序列](./)
- **变化**：差值正负交替
- **解法：贪心/DP

### 进阶思考

1. **为什么 tails 维护的是最小末尾？**
   - 相同长度的 LIS，末尾越小越好
   - 给后续数字更多扩展空间
   - 贪心思想

2. **如何恢复具体的 LIS？**
   - 用 parent 数组记录每个元素的前驱
   - 从最大长度位置回溯

3. **如何求 LIS 的数量？**
   - DP 状态：dp[i] = 以 i 结尾的 LIS 数量
   - 需要考虑长度和数量两个维度

4. **二分查找为什么是 bisect_left？**
   - 找第一个 >= num 的位置
   - 保证严格递增（不能用等于）
   - bisect_right 会允许等于

5. **O(n log n) 一定比 O(n²) 快吗？**
   - 数据量大时优势明显
   - 小数据量时，二分的常数开销可能更大
   - 但 n = 2500 时，O(n log n) ≈ 33000，O(n²) ≈ 6,250,000
