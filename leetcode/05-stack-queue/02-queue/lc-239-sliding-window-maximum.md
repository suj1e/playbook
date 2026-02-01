# 239. Sliding Window Maximum / 滑动窗口最大值

[中文链接](https://leetcode.cn/problems/sliding-window-maximum/) | [English](https://leetcode.com/problems/sliding-window-maximum/)

**难度**：Hard
**标签**：队列 | 数组 | 滑动窗口 | 单调队列
**相似题目**：[lc-42](./) | [lc-862](./) | [lc-2398](./)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

返回 滑动窗口中的最大值 。

**示例 1**：
```
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**示例 2**：
```
输入：nums = [1], k = 1
输出：[1]
```

**约束条件**：
- 1 <= nums.length <= 10⁵
- -10⁴ <= nums[i] <= 10⁴
- 1 <= k <= nums.length

**进阶**：你能在线性时间复杂度内解决此题吗？

---

## 解题思路

### 方法一：暴力枚举

**思路**：
每个窗口遍历找最大值。

**复杂度**：
- 时间：O(n × k)
- 空间：O(1)

---

### 方法二：单调队列（推荐）

**思路**：
使用单调递减队列，队列头部始终是当前窗口的最大值。

**核心思想**：
- 队列存储索引（而非值）
- 队列中对应的值单调递减
- 移除超出窗口的索引
- 移除比当前值小的索引（它们不可能是最大值了）

**复杂度**：
- 时间：O(n)
- 空间：O(k)

**步骤**：
1. 遍历数组
2. 移除队首超出窗口的索引
3. 移除队尾比当前值小的索引
4. 当前索引入队
5. 窗口形成后，记录队首值（最大值）

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：暴力 O(n × k)
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] result = new int[n - k + 1];

        for (int i = 0; i <= n - k; i++) {
            int max = nums[i];
            for (int j = i + 1; j < i + k; j++) {
                max = Math.max(max, nums[j]);
            }
            result[i] = max;
        }

        return result;
    }

    // 方法二：单调队列 O(n)
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] result = new int[n - k + 1];
        Deque<Integer> deque = new ArrayDeque<>();  // 单调递减队列（存索引）

        for (int i = 0; i < n; i++) {
            // 1. 移除超出窗口的索引
            while (!deque.isEmpty() && deque.peekFirst() < i - k + 1) {
                deque.pollFirst();
            }

            // 2. 移除比当前值小的索引
            while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
                deque.pollLast();
            }

            // 3. 当前索引入队
            deque.offerLast(i);

            // 4. 记录窗口最大值
            if (i >= k - 1) {
                result[i - k + 1] = nums[deque.peekFirst()];
            }
        }

        return result;
    }
}
```

### Python

```python
from typing import List
from collections import deque

class Solution:
    # 方法一：暴力 O(n × k)
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        result = []

        for i in range(n - k + 1):
            result.append(max(nums[i:i + k]))

        return result

    # 方法二：单调队列 O(n)
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        result = []
        dq = deque()  # 单调递减队列（存索引）

        for i, num in enumerate(nums):
            # 1. 移除超出窗口的索引
            while dq and dq[0] < i - k + 1:
                dq.popleft()

            # 2. 移除比当前值小的索引
            while dq and nums[dq[-1]] < num:
                dq.pop()

            # 3. 当前索引入队
            dq.append(i)

            # 4. 记录窗口最大值
            if i >= k - 1:
                result.append(nums[dq[0]])

        return result
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 单调递减队列 | 队列头部始终是当前窗口的最大值 |
| 存储索引 | 存储索引而非值，方便判断是否超出窗口 |
| 移除超出窗口 | 索引 < i - k + 1 的元素需要移除 |
| 移除较小值 | 比当前值小的元素不可能是最大值 |
| 队首即最大值 | 窗口形成后，队首就是最大值 |

---

## 图解

```
nums = [1, 3, -1, -3, 5, 3, 6, 7], k = 3

单调递减队列（存索引）:

i=0, num=1:
  移除超出: 无
  移除较小: 无
  入队: dq=[0]
  窗口未形成

i=1, num=3:
  移除超出: 无
  移除较小: 3>1, 移除0
  入队: dq=[1]
  窗口未形成

i=2, num=-1:
  移除超出: 无
  移除较小: -1<3, 不移除
  入队: dq=[1,2]
  窗口形成, result=[3]

i=3, num=-3:
  移除超出: 1>=0, 不移除
  移除较小: -3<-1, 不移除
  入队: dq=[1,2,3]
  result=[3,3]

i=4, num=5:
  移除超出: 1<2, 移除1
  移除较小: 5>-3, 移除3; 5>-1, 移除2
  入队: dq=[4]
  result=[3,3,5]

i=5, num=3:
  移除超出: 4>=3, 不移除
  移除较小: 3<5, 不移除
  入队: dq=[4,5]
  result=[3,3,5,5]

i=6, num=6:
  移除超出: 4>=4, 不移除
  移除较小: 6>3, 移除5; 6>5, 移除4
  入队: dq=[6]
  result=[3,3,5,5,6]

i=7, num=7:
  移除超出: 6<5, 移除6
  移除较小: 7>?, 无
  入队: dq=[7]
  result=[3,3,5,5,6,7]

结果: [3, 3, 5, 5, 6, 7]
```

---

## 变种/延伸

### 变种1：滑动窗口最小值
- **变化**：找窗口中的最小值
- **解法**：使用单调递增队列

### 变种2：滑动窗口中位数
- **题目**：[lc-480 滑动窗口中位数](./)
- **变化**：找窗口的中位数
- **解法**：双堆（大根堆 + 小根堆）

### 变种3：和至少为 K 的最短子数组
- **题目**：[lc-862 和至少为 K 的最短子数组](./)
- **变化**：找和至少为 K 的最短子数组
- **解法**：单调队列 + 前缀和

### 进阶思考

1. **为什么是单调递减队列？**
   - 我们要找最大值
   - 比当前值小的元素不可能是最大值了
   - 所以要移除它们

2. **为什么存储索引而非值？**
   - 需要判断元素是否超出窗口
   - 存储索引可以直接比较 i - k + 1

3. **如果 k = 1 怎么办？**
   - 每个元素就是一个窗口
   - 结果就是数组本身
   - 代码仍能正确处理

4. **如何找滑动窗口的中位数？**
   - 单调队列不适用
   - 可以用两个堆（大根堆存较小的一半，小根堆存较大的一半）

5. **如果数组很大，无法一次加载？**
   - 可以分块处理
   - 或者使用流式处理的方式
