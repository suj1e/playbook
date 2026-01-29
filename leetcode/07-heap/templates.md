# 堆代码模板

堆是一种特殊的完全二叉树，常用于解决 TopK 问题、优先队列等场景。Java 中使用 PriorityQueue，Python 中使用 heapq 模块。

---

## 1. 堆的初始化

### 1.1 小根堆（默认）

#### Java

```java
import java.util.*;

// 默认是小根堆
PriorityQueue<Integer> minHeap = new PriorityQueue<>();

// 带初始容量
PriorityQueue<Integer> minHeap = new PriorityQueue<>(16);

// 从数组构建
int[] arr = {3, 1, 4, 1, 5, 9};
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
for (int num : arr) {
    minHeap.offer(num);
}
```

#### Python

```python
import heapq

# 空堆
min_heap = []

# 从列表构建（会原地堆化）
arr = [3, 1, 4, 1, 5, 9]
heapq.heapify(arr)  # arr 变成了堆
```

---

### 1.2 大根堆

#### Java

```java
// 方法1：使用反向比较器
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());

// 方法2：自定义比较器
PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);

// 方法3：对于自定义对象
PriorityQueue<Node> maxHeap = new PriorityQueue<>((a, b) -> b.val - a.val);
```

#### Python

```python
# Python 只有小根堆，大根堆通过存储负数实现
import heapq

max_heap = []
# 插入时取负
heapq.heappush(max_heap, -num)
# 取出时再取负
max_num = -heapq.heappop(max_heap)
```

---

### 1.3 自定义比较器

#### Java

```java
// 根据对象的某个属性排序
PriorityQueue<int[]> heap = new PriorityQueue<>((a, b) -> a[0] - b[0]);  // 按第一个元素

// 或使用 Comparator
PriorityQueue<Node> heap = new PriorityQueue<>(new Comparator<Node>() {
    @Override
    public int compare(Node a, Node b) {
        if (a.val != b.val) {
            return a.val - b.val;  // 先按 val 升序
        }
        return b.index - a.index;  // val 相同按 index 降序
    }
});
```

#### Python

```python
# 使用元组，heapq 会按元组的顺序比较
heap = []
# (优先级, 数据) 元组
heapq.heappush(heap, (priority, data))
heapq.heappop(heap)  # 返回 (priority, data)

# 对于自定义对象，使用 functools.total_ordering
import heapq
from functools import total_ordering

@total_ordering
class Node:
    def __init__(self, val, index):
        self.val = val
        self.index = index

    def __lt__(self, other):
        # 先按 val 升序
        if self.val != other.val:
            return self.val < other.val
        # val 相同按 index 降序
        return self.index > other.index

# 使用
heap = []
heapq.heappush(heap, Node(5, 1))
heapq.heappush(heap, Node(3, 2))
node = heapq.heappop(heap)
```

---

## 2. 堆的基本操作

### 2.1 插入和删除

#### Java

```java
PriorityQueue<Integer> heap = new PriorityQueue<>();

// 插入
heap.offer(5);
heap.add(3);  // 等价于 offer

// 删除堆顶（最小元素）
int min = heap.poll();  // 返回并删除堆顶，队列为空时返回 null
int min = heap.remove();  // 队列为空时抛异常

// 只查看堆顶（不删除）
int min = heap.peek();  // 队列为空时返回 null
int min = heap.element();  // 队列为空时抛异常

// 获取堆大小
int size = heap.size();

// 判断是否为空
boolean empty = heap.isEmpty();

// 清空堆
heap.clear();
```

#### Python

```python
import heapq

heap = []

# 插入
heapq.heappush(heap, 5)

# 删除堆顶（最小元素）
min_val = heapq.heappop(heap)

# 只查看堆顶（不删除）
min_val = heap[0]  # 不检查堆是否为空

# 获取堆大小
size = len(heap)

# 判断是否为空
empty = len(heap) == 0

# 替换堆顶（先删除再插入，更高效）
heapq.heapreplace(heap, new_value)

# 弹出后插入
popped = heapq.heappushpop(heap, new_value)
```

---

## 3. TopK 问题模板

### 3.1 第 K 大元素（使用小根堆）

**思路**：维护一个大小为 K 的小根堆，堆中存储当前最大的 K 个元素，堆顶是第 K 大。

#### Java

```java
public int findKthLargest(int[] nums, int k) {
    // 小根堆，堆顶是第 k 大
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
```

#### Python

```python
import heapq

def find_kth_largest(nums, k):
    # 小根堆，存储最大的 k 个元素
    min_heap = []

    for num in nums:
        if len(min_heap) < k:
            heapq.heappush(min_heap, num)
        elif num > min_heap[0]:
            heapq.heapreplace(min_heap, num)

    return min_heap[0]
```

---

### 3.2 第 K 小元素（使用大根堆）

#### Java

```java
public int findKthSmallest(int[] nums, int k) {
    // 大根堆，堆顶是第 k 小
    PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());

    for (int num : nums) {
        if (maxHeap.size() < k) {
            maxHeap.offer(num);
        } else if (num < maxHeap.peek()) {
            maxHeap.poll();
            maxHeap.offer(num);
        }
    }

    return maxHeap.peek();
}
```

#### Python

```python
import heapq

def find_kth_smallest(nums, k):
    # 大根堆（存储负数）
    max_heap = []

    for num in nums:
        if len(max_heap) < k:
            heapq.heappush(max_heap, -num)
        elif num < -max_heap[0]:
            heapq.heapreplace(max_heap, -num)

    return -max_heap[0]
```

---

### 3.3 前 K 个高频元素

#### Java

```java
public List<Integer> topKFrequent(int[] nums, int k) {
    // 1. 统计频率
    Map<Integer, Integer> freqMap = new HashMap<>();
    for (int num : nums) {
        freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
    }

    // 2. 使用小根堆，按频率排序
    PriorityQueue<Integer> minHeap = new PriorityQueue<>(
        (a, b) -> freqMap.get(a) - freqMap.get(b)
    );

    for (int num : freqMap.keySet()) {
        if (minHeap.size() < k) {
            minHeap.offer(num);
        } else if (freqMap.get(num) > freqMap.get(minHeap.peek())) {
            minHeap.poll();
            minHeap.offer(num);
        }
    }

    // 3. 收集结果
    List<Integer> result = new ArrayList<>();
    while (!minHeap.isEmpty()) {
        result.add(minHeap.poll());
    }
    return result;
}
```

#### Python

```python
import heapq
from collections import Counter

def top_k_frequent(nums, k):
    # 1. 统计频率
    freq_map = Counter(nums)

    # 2. 使用小根堆，按频率排序
    min_heap = []
    for num, freq in freq_map.items():
        if len(min_heap) < k:
            heapq.heappush(min_heap, (freq, num))
        elif freq > min_heap[0][0]:
            heapq.heapreplace(min_heap, (freq, num))

    # 3. 收集结果
    return [num for freq, num in min_heap]
```

---

## 4. 合并 K 个有序链表

### Java

```java
public ListNode mergeKLists(ListNode[] lists) {
    // 最小堆，按节点值排序
    PriorityQueue<ListNode> minHeap = new PriorityQueue<>((a, b) -> a.val - b.val);

    // 将所有链表的头节点入堆
    for (ListNode head : lists) {
        if (head != null) {
            minHeap.offer(head);
        }
    }

    // 虚拟头节点
    ListNode dummy = new ListNode(0);
    ListNode curr = dummy;

    while (!minHeap.isEmpty()) {
        // 取出最小节点
        ListNode node = minHeap.poll();
        curr.next = node;
        curr = curr.next;

        // 将该节点的下一个节点入堆
        if (node.next != null) {
            minHeap.offer(node.next);
        }
    }

    return dummy.next;
}
```

### Python

```python
import heapq

def merge_k_lists(lists):
    # 最小堆，存储 (val, index, node)
    # 添加 index 是为了防止比较节点时出错
    min_heap = []

    for i, head in enumerate(lists):
        if head:
            heapq.heappush(min_heap, (head.val, i, head))

    dummy = ListNode(0)
    curr = dummy

    while min_heap:
        val, i, node = heapq.heappop(min_heap)
        curr.next = node
        curr = curr.next

        if node.next:
            heapq.heappush(min_heap, (node.next.val, i, node.next))

    return dummy.next
```

---

## 5. 堆排序

### Java

```java
public int[] heapSort(int[] nums) {
    // 构建大根堆
    PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());

    for (int num : nums) {
        maxHeap.offer(num);
    }

    int[] result = new int[nums.length];
    int index = 0;

    while (!maxHeap.isEmpty()) {
        result[index++] = maxHeap.poll();
    }

    return result;
}
```

### Python

```python
import heapq

def heap_sort(nums):
    # 构建小根堆
    heapq.heapify(nums)

    result = []
    while nums:
        result.append(heapq.heappop(nums))

    return result
```

---

## 6. 数据流中位数

**思路**：维护两个堆，大根堆存储较小的一半，小根堆存储较大的一半。

### Java

```java
class MedianFinder {
    // 大根堆：存储较小的一半
    private PriorityQueue<Integer> maxHeap;
    // 小根堆：存储较大的一半
    private PriorityQueue<Integer> minHeap;

    public MedianFinder() {
        maxHeap = new PriorityQueue<>(Collections.reverseOrder());
        minHeap = new PriorityQueue<>();
    }

    public void addNum(int num) {
        // 先放入大根堆
        maxHeap.offer(num);

        // 平衡：大根堆的最大值移到小根堆
        minHeap.offer(maxHeap.poll());

        // 如果小根堆比大根堆多，移回去
        if (minHeap.size() > maxHeap.size()) {
            maxHeap.offer(minHeap.poll());
        }
    }

    public double findMedian() {
        if (maxHeap.size() > minHeap.size()) {
            return maxHeap.peek();
        }
        return (maxHeap.peek() + minHeap.peek()) / 2.0;
    }
}
```

### Python

```python
import heapq

class MedianFinder:
    def __init__(self):
        # 大根堆（存储负数）：存储较小的一半
        self.max_heap = []
        # 小根堆：存储较大的一半
        self.min_heap = []

    def addNum(self, num):
        # 先放入大根堆
        heapq.heappush(self.max_heap, -num)

        # 平衡：大根堆的最大值移到小根堆
        heapq.heappush(self.min_heap, -self.max_heap[0])
        heapq.heappop(self.max_heap)

        # 如果小根堆比大根堆多，移回去
        if len(self.min_heap) > len(self.max_heap):
            heapq.heappush(self.max_heap, -self.min_heap[0])
            heapq.heappop(self.min_heap)

    def findMedian(self):
        if len(self.max_heap) > len(self.min_heap):
            return -self.max_heap[0]
        return (-self.max_heap[0] + self.min_heap[0]) / 2
```

---

## 7. 复杂度总结

| 操作 | 时间复杂度 | 说明 |
|------|-----------|------|
| 插入（offer/push） | O(log n) | 需要向上调整 |
| 删除堆顶（poll/pop） | O(log n) | 需要向下调整 |
| 查看堆顶（peek） | O(1) | 直接返回 |
| 构建堆（heapify） | O(n) | 从数组构建 |
| 堆排序 | O(n log n) | n 次删除操作 |

---

## 8. 堆 vs 排序的选择

| 场景 | 推荐方法 | 原因 |
|------|---------|------|
| 找 Top 1 | 遍历一遍 | O(n) |
| 找 Top K（n 很大） | 堆 | O(n log K) |
| 找 Top K（n 较小） | 排序 | O(n log n)，代码简单 |
| 动态数据流 | 堆 | 需要频繁插入删除 |
| 已排序数组 | 二分查找 | O(log n) |

---

## 9. 常见陷阱

| 陷阱 | 说明 | 解决方案 |
|------|------|----------|
| 大小根堆混淆 | Java 默认小根堆，Python 只有小根堆 | 明确需求，必要时用反向比较器或负数 |
| 堆的容量控制 | TopK 问题需要控制堆大小为 K | 先判断 size 再决定是否替换 |
| 对象比较 | 自定义对象需要实现比较逻辑 | 使用 Comparator 或实现 Comparable |
| 堆顶访问 | 堆为空时访问会出错 | 访问前检查 isEmpty() 或 len() |
| 相等元素处理 | Python 比较元组时会比较后续元素 | 使用唯一索引作为元组的第二项 |
| 内存溢出 | 数据量过大时全部入堆可能溢出 | 考虑分批处理或使用其他算法 |
