# 堆专题

## 专题概述

堆是一种基于完全二叉树的数据结构，常用于获取最大值/最小值。本专题涵盖：

- **TopK 问题**：找出前 K 个最大/最小的元素
- **合并问题**：合并多个有序数组/链表

### 核心知识点

1. 大顶堆：堆顶是最大元素
2. 小顶堆：堆顶是最小元素
3. 堆的插入和删除操作：O(log n)
4. 堆化：O(n) 时间构建堆

---

## 学习路径

1. 先阅读 [templates.md](./templates.md) 熟悉堆操作模板
2. 按顺序完成各分类题目：
   - [ ] 01-top-k（TopK 问题）
   - [ ] 02-merge（合并问题）

---

## 题目分类

| 分类 | 题目数 | 难度 | 说明 |
|------|-------|------|------|
| 01-top-k | 2 | ⭐⭐⭐ | 寻找前 K 个元素 |
| 02-merge | 1 | ⭐⭐⭐ | 合并多个有序结构 |

---

## 进度追踪

### 01-top-k
- [ ] lc-215-kth-largest-element-in-an-array (Medium)
- [ ] lc-347-top-k-frequent-elements (Medium)

### 02-merge
- [ ] lc-23-merge-k-sorted-lists (Hard)

---

## 解题模板总结

### 最小堆（Java）
```java
// 创建最小堆
Queue<Integer> minHeap = new PriorityQueue<>();

// 插入元素
minHeap.offer(1);

// 获取最小值
int min = minHeap.peek();

// 删除最小值
int min = minHeap.poll();
```

### 最大堆（Java）
```java
// 创建最大堆
Queue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);

// 或者使用 Collections.reverseOrder()
Queue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
```

### 最小堆（Python）
```python
import heapq

# 创建最小堆
heap = []

# 插入元素
heapq.heappush(heap, 1)

# 获取最小值
min_val = heap[0]

# 删除最小值
min_val = heapq.heappop(heap)

# 列表转堆
heapq.heapify(nums)
```

### 最大堆（Python）
```python
# 技巧：存储负数
import heapq
max_heap = []
heapq.heappush(max_heap, -x)
max_val = -heapq.heappop(max_heap)
```

### TopK 模板（最小堆）
```java
// 找前 K 个最大的元素
Queue<Integer> minHeap = new PriorityQueue<>();
for (int num : nums) {
    minHeap.offer(num);
    if (minHeap.size() > k) {
        minHeap.poll();  // 维护堆大小为 K
    }
}
// 堆中就是前 K 个最大的元素
```

---

## 参考资源

- [LeetCode 堆标签](https://leetcode.cn/tag/heap/)
