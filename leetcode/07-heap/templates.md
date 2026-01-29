# 堆代码模板

堆是一种特殊的完全二叉树，常用于 TopK 问题。

---

## 1. 堆的初始化

### 1.1 小根堆/大根堆

```
小根堆（堆顶最小）:    大根堆（堆顶最大）:
      1                    9
     / \                  / \
    3   5                7   5
   / \                  /
  7   9                3

特点: 父节点 <= 子节点   特点: 父节点 >= 子节点
```

#### Java
```java
// 小根堆（默认）
PriorityQueue<Integer> minHeap = new PriorityQueue<>();

// 大根堆
PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);

// 自定义比较器
PriorityQueue<int[]> heap = new PriorityQueue<>((a, b) -> a[0] - b[0]);
```

#### Python
```python
import heapq

# 小根堆
min_heap = []
heapq.heappush(min_heap, 5)
heapq.heappop(min_heap)

# 大根堆（存储负数）
max_heap = []
heapq.heappush(max_heap, -5)
max_val = -heapq.heappop(max_heap)
```

---

## 2. TopK 问题

### 2.1 第 K 大元素（小根堆）

**适用场景**：找第 K 大/小的元素

```
数组: [3, 2, 1, 5, 6, 4], K = 3

维护大小为 K 的小根堆:
i=0, num=3: heap=[3]
i=1, num=2: heap=[2,3]  (2<3, 直接加入)
i=2, num=1: heap=[1,3,2]  (1<2, 直接加入)
i=3, num=5: 5>heap[0]=1, 弹出1, 加入5
           heap=[2,3,5]
i=4, num=6: 6>heap[0]=2, 弹出2, 加入6
           heap=[3,5,6]
i=5, num=4: 4>heap[0]=3, 弹出3, 加入4
           heap=[4,5,6]

返回: heap[0] = 4 (第3大)
```

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
    min_heap = []

    for num in nums:
        if len(min_heap) < k:
            heapq.heappush(min_heap, num)
        elif num > min_heap[0]:
            heapq.heapreplace(min_heap, num)

    return min_heap[0]
```

---

## 3. 合并 K 个有序链表

### 3.1 堆合并

```
输入: [1->4->5, 1->3->4, 2->6]

堆初始化: [(1,0,list1), (1,1,list2), (2,2,list3)]
           (值, 链表索引, 节点)

过程:
1. 弹出 (1,0,list1), 加入 list1 的下一个 4
2. 弹出 (1,1,list2), 加入 list2 的下一个 3
3. 弹出 (2,2,list3), 加入 list3 的下一个 6
4. 弹出 (3,1,list2), list2 到达末尾
5. 弹出 (4,0,list1), 加入 list1 的下一个 5
6. 弹出 (4,1,list1), list1 到达末尾
7. 弹出 (5,1,list1), list1 到达末尾
8. 弹出 (6,2,list3), list3 到达末尾

结果: 1 -> 1 -> 2 -> 3 -> 4 -> 4 -> 5 -> 6
```

#### Java
```java
public ListNode mergeKLists(ListNode[] lists) {
    PriorityQueue<ListNode> minHeap = new PriorityQueue<>((a, b) -> a.val - b.val);

    // 将所有链表的头节点入堆
    for (ListNode head : lists) {
        if (head != null) {
            minHeap.offer(head);
        }
    }

    ListNode dummy = new ListNode(0);
    ListNode curr = dummy;

    while (!minHeap.isEmpty()) {
        ListNode node = minHeap.poll();
        curr.next = node;
        curr = curr.next;

        if (node.next != null) {
            minHeap.offer(node.next);
        }
    }

    return dummy.next;
}
```

#### Python
```python
import heapq

def merge_k_lists(lists):
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

## 4. 数据流中位数

### 4.1 双堆维护

**适用场景**：动态数据流求中位数

```
数据流: [5, 2, 3, 1, 4]

maxHeap(存较小的一半)  minHeap(存较大的一半)

5:  [5]                  []
2:  [2]                  [5]
3:  [2,3]                [5]  (平衡: 3 > maxHeap.top=2, 移到 minHeap)
                        [2]  [3,5]
1:  [2,1]                [3,5]
                        [1,2]  [3,5]
4:  [1,2]                [3,4,5]  (4 > maxHeap.top=2, 加入 minHeap)

中位数: size 不同，取 maxHeap.top = 2
      size 相同，取 (maxHeap.top + minHeap.top) / 2
```

#### Java
```java
class MedianFinder {
    private PriorityQueue<Integer> maxHeap;  // 存较小的一半
    private PriorityQueue<Integer> minHeap;  // 存较大的一半

    public MedianFinder() {
        maxHeap = new PriorityQueue<>((a, b) -> b - a);
        minHeap = new PriorityQueue<>();
    }

    public void addNum(int num) {
        maxHeap.offer(num);
        minHeap.offer(maxHeap.poll());  // 平衡

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

#### Python
```python
import heapq

class MedianFinder:
    def __init__(self):
        self.max_heap = []  # 存较小的一半（用负数）
        self.min_heap = []  # 存较大的一半

    def addNum(self, num):
        heapq.heappush(self.max_heap, -num)
        heapq.heappush(self.min_heap, -self.max_heap[0])
        heapq.heappop(self.max_heap)

        if len(self.min_heap) > len(self.max_heap):
            heapq.heappush(self.max_heap, -self.min_heap[0])
            heapq.heappop(self.min_heap)

    def findMedian(self):
        if len(self.max_heap) > len(self.min_heap):
            return -self.max_heap[0]
        return (-self.max_heap[0] + self.min_heap[0]) / 2
```

---

## 5. 复杂度总结

| 操作 | 时间复杂度 | 空间复杂度 |
|------|-----------|-----------|
| 插入 | O(log n) | O(1) |
| 删除堆顶 | O(log n) | O(1) |
| 查看堆顶 | O(1) | O(1) |
| 构建堆 | O(n) | O(n) |
| TopK | O(n log k) | O(k) |
