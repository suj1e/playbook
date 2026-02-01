# 23. Merge k Sorted Lists / 合并 K 个升序链表

[中文链接](https://leetcode.cn/problems/merge-k-sorted-lists/) | [English](https://leetcode.com/problems/merge-k-sorted-lists/)

**难度**：Hard
**标签**：链表 | 堆（优先队列） | 分治
**相似题目**：[lc-21](../../03-linked-list/01-basic/lc-21-merge-two-sorted-lists.md) | [lc-264](./)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

**示例 1**：
```
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到：
1->1->2->3->4->4->5->6
```

**示例 2**：
```
输入：lists = []
输出：[]
```

**示例 3**：
```
输入：lists = [[]]
输出：[]
```

**约束条件**：
- k == lists.length
- 0 <= k <= 10⁴
- 0 <= lists[i].length <= 500
- -10⁴ <= lists[i][j] <= 10⁴
- lists[i] 按 升序 排列
- lists[i].length 的总和不超过 10⁴

---

## 解题思路

### 方法一：最小堆（优先队列）推荐

**思路**：
维护一个大小为 k 的最小堆，堆中存储每个链表的头节点。每次取出堆顶元素（最小的），加入结果链表，然后将其后继节点加入堆。

**核心思想**：
- 利用最小堆的性质，每次 O(log k) 时间获取最小元素
- 总共 N 个节点，时间复杂度 O(N log k)
- 空间复杂度 O(k) 存储堆

**复杂度**：
- 时间：O(N log k)，N 是所有节点总数
- 空间：O(k) - 堆的大小

**步骤**：
1. 创建最小堆（优先队列）
2. 将所有链表的头节点加入堆
3. 循环取出堆顶元素，加入结果链表
4. 将取出的节点的后继加入堆（如果存在）

### 方法二：分治合并

**思路**：
将 k 个链表两两配对合并，类似归并排序。

**复杂度**：
- 时间：O(N log k)
- 空间：O(log k) - 递归栈

### 方法三：逐一合并

**思路**：
依次将每个链表合并到结果链表。

**复杂度**：
- 时间：O(N × k) - 每次合并需要遍历整个结果链表
- 空间：O(1)

---

## 代码实现

### Java - 最小堆

```java
/**
 * Definition for singly-linked list.
 */
class ListNode {
    int val;
    ListNode next;
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}

class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) {
            return null;
        }

        // 最小堆（优先队列）
        PriorityQueue<ListNode> pq = new PriorityQueue<>((a, b) -> a.val - b.val);

        // 将所有链表头节点加入堆
        for (ListNode head : lists) {
            if (head != null) {
                pq.offer(head);
            }
        }

        // 虚拟头节点
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;

        // 依次取出最小节点
        while (!pq.isEmpty()) {
            ListNode node = pq.poll();
            curr.next = node;
            curr = curr.next;

            // 将后继节点加入堆
            if (node.next != null) {
                pq.offer(node.next);
            }
        }

        return dummy.next;
    }
}
```

### Java - 分治合并

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) {
            return null;
        }
        return divide(lists, 0, lists.length - 1);
    }

    // 分治：合并 [left, right] 范围内的链表
    private ListNode divide(ListNode[] lists, int left, int right) {
        if (left == right) {
            return lists[left];
        }
        if (left > right) {
            return null;
        }

        int mid = left + (right - left) / 2;
        ListNode l1 = divide(lists, left, mid);
        ListNode l2 = divide(lists, mid + 1, right);
        return mergeTwoLists(l1, l2);
    }

    // 合并两个有序链表
    private ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;

        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                curr.next = l1;
                l1 = l1.next;
            } else {
                curr.next = l2;
                l2 = l2.next;
            }
            curr = curr.next;
        }

        curr.next = (l1 != null) ? l1 : l2;
        return dummy.next;
    }
}
```

### Python - 最小堆

```python
from typing import List, Optional
import heapq

# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        if not lists:
            return None

        # 最小堆
        min_heap = []

        # 将所有链表头节点加入堆
        # 注意：需要用 (val, index, node) 来避免比较 node 时出错
        for i, head in enumerate(lists):
            if head:
                heapq.heappush(min_heap, (head.val, i, head))

        # 虚拟头节点
        dummy = ListNode(0)
        curr = dummy

        while min_heap:
            val, i, node = heapq.heappop(min_heap)
            curr.next = node
            curr = curr.next

            # 将后继节点加入堆
            if node.next:
                heapq.heappush(min_heap, (node.next.val, i, node.next))

        return dummy.next
```

### Python - 分治合并

```python
from typing import List, Optional

class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        if not lists:
            return None
        return self._divide(lists, 0, len(lists) - 1)

    def _divide(self, lists: List[Optional[ListNode]], left: int, right: int) -> Optional[ListNode]:
        if left == right:
            return lists[left]
        if left > right:
            return None

        mid = left + (right - left) // 2
        l1 = self._divide(lists, left, mid)
        l2 = self._divide(lists, mid + 1, right)
        return self._merge_two_lists(l1, l2)

    def _merge_two_lists(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(0)
        curr = dummy

        while l1 and l2:
            if l1.val <= l2.val:
                curr.next = l1
                l1 = l1.next
            else:
                curr.next = l2
                l2 = l2.next
            curr = curr.next

        curr.next = l1 if l1 else l2
        return dummy.next
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 最小堆 | 每次 O(log k) 取最小元素 |
| 虚拟头节点 | 简化链表合并操作 |
| 堆元组 | Python 中用 (val, index, node) 避免比较 |
| 分治思想 | 两两合并，类似归并排序 |
| 空换头 | 每次取出一个节点后，将其后继加入堆 |

---

## 图解

```
输入: lists = [[1,4,5], [1,3,4], [2,6]]

初始堆构建:
        1(0)  <- 链表0的头
       /   \
    1(1)   2(2)  <- 链表1和链表2的头

第1轮: 取出 1(0)，加入结果
      将 4(0) 加入堆
        1(1)
       /   \
    2(2)   4(0)

结果: 1 ->

第2轮: 取出 1(1)，加入结果
      将 3(1) 加入堆
        2(2)
       /   \
    3(1)   4(0)

结果: 1 -> 1 ->

第3轮: 取出 2(2)，加入结果
      将 6(2) 加入堆
        3(1)
       /   \
    6(2)   4(0)

结果: 1 -> 1 -> 2 ->

第4轮: 取出 3(1)，加入结果
      3(1) 没有后继
        4(0)
       /
    6(2)

结果: 1 -> 1 -> 2 -> 3 ->

第5轮: 取出 4(0)，加入结果
      将 5(0) 加入堆
        5(0)
       /
    6(2)

结果: 1 -> 1 -> 2 -> 3 -> 4 ->

第6轮: 取出 5(0)，加入结果
      5(0) 没有后继
        6(2)

结果: 1 -> 1 -> 2 -> 3 -> 4 -> 5 ->

第7轮: 取出 6(2)，加入结果
      6(2) 没有后继
      堆为空

结果: 1 -> 1 -> 2 -> 3 -> 4 -> 5 -> 6

---

分治合并示意:

[[1,4,5], [1,3,4], [2,6]]

第一轮两两合并:
merge([1,4,5], [1,3,4]) = [1,1,3,4,4,5]
merge([2,6], null) = [2,6]

第二轮合并:
merge([1,1,3,4,4,5], [2,6]) = [1,1,2,3,4,4,5,6]
```

---

## 变种/延伸

### 变种1：合并两个有序链表
- **题目**：[lc-21 合并两个有序链表](../../03-linked-list/01-basic/lc-21-merge-two-sorted-lists.md)
- **变化**：只有 2 个链表
- **解法**：简单的双指针合并

### 变种2：丑数 II
- **题目**：[lc-264 丑数 II](./)
- **变化**：合并多个有序序列
- **解法**：类似最小堆思路

### 进阶思考

1. **为什么用堆而不是逐个合并？**
   - 逐个合并：O(N × k)
   - 堆：O(N log k)
   - 当 k 较大时，堆明显更快

2. **Python 中为什么要加 index？**
   - 堆比较元素时，如果 val 相同会比较 node
   - ListNode 不可比较，会报错
   - 加 index 确保不会比较到 node

3. **分治为什么更快？**
   - 分治：每次合并长度相近的链表
   - 逐个合并：每次都遍历累积的结果链表
   - 分治减少了重复遍历

4. **如何处理空链表？**
   - 初始化时跳过 null 链表
   - 堆中只存非空节点

5. **空间复杂度？**
   - 堆：O(k)
   - 分治：O(log k) 递归栈
   - 逐个合并：O(1)

6. **如果链表数量极大？**
   - 堆和分治都可以处理
   - 堆需要 O(k) 空间
   - 分治空间更优
