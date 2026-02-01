# 215. Kth Largest Element in an Array / 数组中的第K个最大元素

[中文链接](https://leetcode.cn/problems/kth-largest-element-in-an-array/) | [English](https://leetcode.com/problems/kth-largest-element-in-an-array/)

**难度**：Medium
**标签**：数组 | 分治 | 快速选择 | 排序 | 堆（优先队列）
**相似题目**：[lc-973](./) | [lc-347](../../04-hash/01-frequency/lc-347-top-k-frequent-elements.md) | [lc-703](./)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

给定整数数组 `nums` 和整数 `k`，请返回数组中第 **k** 个最大的元素。

请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

你必须设计并实现时间复杂度为 O(n) 的算法解决此问题。

**示例 1**：
```
输入: [3,2,1,5,6,4], k = 2
输出: 5
```

**示例 2**：
```
输入: [3,2,3,1,2,4,5,5,6], k = 4
输出: 4
```

**约束条件**：
- 1 <= k <= nums.length <= 10⁵
- -10⁴ <= nums[i] <= 10⁴

---

## 解题思路

### 方法一：排序

**思路**：
排序后直接返回索引。

**复杂度**：
- 时间：O(n log n)
- 空间：O(1) 或 O(n)

---

### 方法二：堆（推荐）

**思路**：
维护大小为 k 的小根堆，堆顶就是第 k 大的元素。

**复杂度**：
- 时间：O(n log k)
- 空间：O(k)

**步骤**：
1. 创建小根堆
2. 遍历数组，维护堆大小为 k
3. 堆顶就是第 k 大元素

---

### 方法三：快速选择

**思路**：
类似快速排序的 partition，每次确定一个元素的最终位置。

**复杂度**：
- 时间：O(n) 平均，O(n²) 最坏
- 空间：O(1)

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：排序 O(n log n)
    public int findKthLargest(int[] nums, int k) {
        Arrays.sort(nums);
        return nums[nums.length - k];
    }

    // 方法二：小根堆 O(n log k)
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(k);

        for (int num : nums) {
            if (minHeap.size() < k) {
                minHeap.offer(num);
            } else if (num > minHeap.peek()) {
                minHeap.poll();
                minHeap.offer(num);
            }
        }

        return minHeap.peek();
    }

    // 方法三：快速选择 O(n)
    public int findKthLargest(int[] nums, int k) {
        int target = nums.length - k;  // 第 k 大 = 第 n-k+1 小
        int left = 0, right = nums.length - 1;

        while (left <= right) {
            int pivotIndex = partition(nums, left, right);

            if (pivotIndex == target) {
                return nums[pivotIndex];
            } else if (pivotIndex < target) {
                left = pivotIndex + 1;
            } else {
                right = pivotIndex - 1;
            }
        }

        return -1;  // 不会执行
    }

    private int partition(int[] nums, int left, int right) {
        int pivot = nums[right];
        int i = left;

        for (int j = left; j < right; j++) {
            if (nums[j] <= pivot) {
                swap(nums, i, j);
                i++;
            }
        }

        swap(nums, i, right);
        return i;
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

### Python

```python
import heapq
from typing import List

class Solution:
    # 方法一：排序 O(n log n)
    def findKthLargest(self, nums: List[int], k: int) -> int:
        nums.sort()
        return nums[-k]

    # 方法二：小根堆 O(n log k)
    def findKthLargest(self, nums: List[int], k: int) -> int:
        return heapq.nlargest(k, nums)[-1]

    # 方法二详细版
    def findKthLargest(self, nums: List[int], k: int) -> int:
        min_heap = []

        for num in nums:
            if len(min_heap) < k:
                heapq.heappush(min_heap, num)
            elif num > min_heap[0]:
                heapq.heapreplace(min_heap, num)

        return min_heap[0]

    # 方法三：快速选择 O(n)
    def findKthLargest(self, nums: List[int], k: int) -> int:
        target = len(nums) - k  # 第 k 大 = 第 n-k+1 小
        left, right = 0, len(nums) - 1

        while left <= right:
            pivot_index = self._partition(nums, left, right)

            if pivot_index == target:
                return nums[pivot_index]
            elif pivot_index < target:
                left = pivot_index + 1
            else:
                right = pivot_index - 1

        return -1  # 不会执行

    def _partition(self, nums, left, right):
        pivot = nums[right]
        i = left

        for j in range(left, right):
            if nums[j] <= pivot:
                nums[i], nums[j] = nums[j], nums[i]
                i += 1

        nums[i], nums[right] = nums[right], nums[i]
        return i
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 小根堆 | 堆顶是前 k 个中最小的，即第 k 大 |
| 堆大小 | 维护为 k，超出时弹出堆顶 |
| 快速选择 | 类似快排，但只递归一边 |
| 目标索引 | 第 k 大 = 第 n-k+1 小 |
| 时间复杂度 | 堆 O(n log k)，快速选择 O(n) 平均 |

---

## 图解

```
数组: [3, 2, 1, 5, 6, 4], k = 2

小根堆法:
维护大小为 2 的小根堆

3: heap = [3]
2: 2 < 3, heap = [2, 3]
1: 1 < 2, 1 < 3, 不替换
5: 5 > 2, 弹出 2, heap = [3, 5]
6: 6 > 3, 弹出 3, heap = [5, 6]
4: 4 < 5, 不替换

堆顶 = 5 (第 2 大)

---

快速选择法:
找第 2 大 = 找第 6-2+1 = 第 5 小

partition: [3,2,1,5,6,4]
pivot = 4, i = 0

j=0: 3 <= 4, swap(0,0), i=1, [3,2,1,5,6,4]
j=1: 2 <= 4, swap(1,1), i=2, [3,2,1,5,6,4]
j=2: 1 <= 4, swap(2,2), i=3, [3,2,1,5,6,4]
j=3: 5 > 4, 跳过
j=4: 6 > 4, 跳过

swap(3,5), [3,2,1,4,6,5]
pivot 在 index 3

target = 5, pivot = 3 < 5, 在右边找
left = 4, right = 5

partition: [3,2,1,4,6,5], left=4, right=5
pivot = 5, i = 4

j=4: 6 > 5, 跳过

swap(4,5), [3,2,1,4,5,6]
pivot 在 index 4

target = 5, pivot = 4 < 5, 在右边找
left = 5, right = 5

partition: [3,2,1,4,5,6], left=5, right=5
pivot = 6, i = 5
pivot 在 index 5

target = 5, pivot = 5 == 5, 找到!
返回 nums[5] = 6

验证: 排序后 [1,2,3,4,5,6]
第 2 大 = 5 ✗, 第 5 小 = 6 ✗

注意: 第 k 大的定义
排序后: [1,2,3,4,5,6]
索引:    0 1 2 3 4 5
第 1 大 = 6 (index 5)
第 2 大 = 5 (index 4)
...
第 k 大 = nums[n-k]

所以 target 应该是 n-k = 6-2 = 4
```

---

## 变种/延伸

### 变种1：前 K 个高频元素
- **题目**：[lc-347 前K个高频元素](../../04-hash/01-frequency/lc-347-top-k-frequent-elements.md)
- **变化**：返回频率最高的 k 个元素
- **解法**：堆 + 哈希表

### 变种2：数据流中的第 K 大元素
- **题目**：[lc-703 数据流中的第K大元素](./)
- **变化**：数据持续流入
- **解法**：维护固定大小的堆

### 变种3：最接近原点的 K 个点
- **题目**：[lc-973 最接近原点的 K 个点](./)
- **变化**：找距离原点最近的 k 个点
- **解法**：大根堆或快速选择

### 进阶思考

1. **为什么用小根堆而非大根堆？**
   - 小根堆维护 k 个最大的元素
   - 大根堆需要存储全部元素

2. **快速选择的最坏情况？**
   - 数组已经有序，每次只减少一个元素
   - O(n²) 时间
   - 可以随机选择 pivot 优化

3. **如何找第 k 小的元素？**
   - 小根堆：维护大小为 n-k+1 的小根堆
   - 快速选择：找第 k 小直接找

4. **如果 k = 1 怎么办？**
   - 直接遍历找最大值，O(n) 时间
   - 不需要用堆

5. **如何处理数据流？**
   - 使用固定大小的堆
   - 新元素来时与堆顶比较
   - 大于堆顶则替换
