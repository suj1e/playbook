# 347. Top K Frequent Elements / 前 K 个高频元素

[中文链接](https://leetcode.cn/problems/top-k-frequent-elements/) | [English](https://leetcode.com/problems/top-k-frequent-elements/)

**难度**：Medium
**标签**：数组 | 哈希表 | 分治 | 桶排序 | 计数 | 堆（优先队列）
**相似题目**：[lc-215](../../../07-heap/01-top-k/lc-215-kth-largest-element-in-an-array.md) | [lc-692](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回其中出现频率前 `k` 高的元素。你可以按 任意顺序 返回答案。

**示例 1**：
```
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
```

**示例 2**：
```
输入: nums = [1], k = 1
输出: [1]
```

**约束条件**：
- 1 <= nums.length <= 10⁵
- k 的取值范围是 [1, 数组中不相同的元素的个数]
- 题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的

**进阶**：你所设计算法的时间复杂度 必须 优于 O(n log n) ，其中 n 是数组大小。

---

## 解题思路

### 方法一：哈希表 + 堆（推荐）

**思路**：
1. 用哈希表统计每个元素的频率
2. 用小根堆维护前 k 个高频元素
3. 遍历哈希表，堆大小超过 k 时弹出堆顶（最小频率）

**复杂度**：
- 时间：O(n log k)
- 空间：O(n)

**步骤**：
1. 遍历数组，用 HashMap 统计频率
2. 创建小根堆，按频率排序
3. 遍历 HashMap，维护堆大小为 k
4. 返回堆中所有元素

---

### 方法二：哈希表 + 桶排序

**思路**：
频率最大为 n，创建 n+1 个桶，桶的下标代表频率。

**复杂度**：
- 时间：O(n)
- 空间：O(n)

**步骤**：
1. 统计每个元素的频率
2. 创建 n+1 个桶，每个桶存储该频率的所有元素
3. 从高频率桶向低频率桶遍历，收集前 k 个元素

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：哈希表 + 小根堆
    public int[] topKFrequent(int[] nums, int k) {
        // 1. 统计频率
        Map<Integer, Integer> freqMap = new HashMap<>();
        for (int num : nums) {
            freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
        }

        // 2. 小根堆（按频率排序）
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(
            (a, b) -> freqMap.get(a) - freqMap.get(b)
        );

        // 3. 维护堆大小为 k
        for (int num : freqMap.keySet()) {
            minHeap.offer(num);
            if (minHeap.size() > k) {
                minHeap.poll();  // 弹出频率最小的
            }
        }

        // 4. 收集结果
        int[] result = new int[k];
        int i = 0;
        while (!minHeap.isEmpty()) {
            result[i++] = minHeap.poll();
        }
        return result;
    }

    // 方法二：哈希表 + 桶排序（O(n)）
    public int[] topKFrequent(int[] nums, int k) {
        // 1. 统计频率
        Map<Integer, Integer> freqMap = new HashMap<>();
        for (int num : nums) {
            freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
        }

        // 2. 创建桶（频率 -> 元素列表）
        List<Integer>[] buckets = new List[nums.length + 1];
        for (int i = 0; i <= nums.length; i++) {
            buckets[i] = new ArrayList<>();
        }

        for (int num : freqMap.keySet()) {
            int freq = freqMap.get(num);
            buckets[freq].add(num);
        }

        // 3. 从高到低收集前 k 个
        List<Integer> result = new ArrayList<>();
        for (int i = nums.length; i >= 0 && result.size() < k; i--) {
            result.addAll(buckets.get(i));
        }

        // 4. 转换为数组
        int[] arr = new int[k];
        for (int i = 0; i < k; i++) {
            arr[i] = result.get(i);
        }
        return arr;
    }
}
```

### Python

```python
from typing import List
from collections import Counter
import heapq

class Solution:
    # 方法一：哈希表 + 堆
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # 1. 统计频率
        count = Counter(nums)

        # 2. 小根堆（使用负数模拟大根堆，取前 k 个）
        # 或者直接用 nsmallest
        return heapq.nlargest(k, count.keys(), key=count.get)

    # 方法一详细版
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # 1. 统计频率
        count = Counter(nums)

        # 2. 小根堆（按频率）
        min_heap = []
        for num, freq in count.items():
            heapq.heappush(min_heap, (freq, num))
            if len(min_heap) > k:
                heapq.heappop(min_heap)  # 弹出频率最小的

        # 3. 收集结果
        return [num for freq, num in min_heap]

    # 方法二：桶排序（O(n)）
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # 1. 统计频率
        count = Counter(nums)

        # 2. 创建桶
        buckets = [[] for _ in range(len(nums) + 1)]
        for num, freq in count.items():
            buckets[freq].append(num)

        # 3. 从高到低收集
        result = []
        for i in range(len(nums), -1, -1):
            result.extend(buckets[i])
            if len(result) >= k:
                break

        return result[:k]
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 频率统计 | 使用 HashMap/Counter 统计元素出现次数 |
| 小根堆 | 维护大小为 k 的堆，堆顶是当前最小的频率 |
| 大根堆 | 可以用 n-largest 直接获取前 k 个 |
| 桶排序 | 利用频率最大为 n 的特点，创建 n+1 个桶 |
| 时间优化 | 桶排序可以达到 O(n) 时间复杂度 |
| 堆的大小 | 堆最大为 k，所以时间复杂度是 O(n log k) |

---

## 变种/延伸

### 变种1：第 K 个高频元素
- **题目**：[lc-215 数组中的第K个最大元素](../../../07-heap/01-top-k/lc-215-kth-largest-element-in-an-array.md)
- **变化**：不需要统计频率，直接找第 k 大
- **解法**：小根堆维护 k 个元素

### 变种2：前 K 个高频单词
- **题目**：[lc-692 前K个高频单词](./)
- **变化**：
  - 频率相同时，按字母顺序排列
  - 返回单词而非数字
- **解法**：堆排序时自定义比较器

### 变种3：数据流中的第 K 大元素
- **题目**：[lc-703 数据流中的第K大元素](./)
- **变化**：数据持续流入，动态查询
- **解法**：维护固定大小的小根堆

### 进阶思考

1. **如果 k 很小（如 k=1），如何优化？**
   - 直接遍历哈希表找最大值，O(n) 时间，O(1) 额外空间

2. **如果 k 很大（接近 n），应该用大根堆还是小根堆？**
   - k 接近 n 时，用大根堆可能更好
   - 或者求"后 n-k 个低频元素"

3. **如何验证桶排序的空间复杂度？**
   - 桶的数量是 n+1
   - 每个元素只存一次
   - 总空间 O(n)

4. **如果元素范围很小（如 1-100），可以怎么优化？**
   - 可以用计数排序，直接用数组统计频率
   - 空间复杂度可以降到 O(range)
