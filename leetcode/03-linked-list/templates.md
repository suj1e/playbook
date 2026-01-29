# 链表代码模板

链表是力扣中常见的数据结构，掌握这些模板可以提高解题效率。

---

## 1. 链表定义

### 1.1 节点定义

```
单向链表节点:
┌───┬───┐
│val│next│
└───┴───┘

双向链表节点:
┌────┬────┬────┐
│prev│val │next│
└────┴────┴────┘
```

#### Java
```java
// 单向链表
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}

// 双向链表
public class DoublyListNode {
    int val;
    DoublyListNode prev;
    DoublyListNode next;
    DoublyListNode(int x) { val = x; }
}
```

#### Python
```python
# 单向链表
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

# 双向链表
class DoublyListNode:
    def __init__(self, val=0, prev=None, next=None):
        self.val = val
        self.prev = prev
        self.next = next
```

---

## 2. 反转链表

### 2.1 迭代反转

```
原链表: 1 -> 2 -> 3 -> null

步骤1:  1 -> 2 -> 3 -> null
        ↑
       prev

步骤2:  1 <- 2    3 -> null
              ↑   ↑
            prev curr

步骤3:  1 <- 2 <- 3    null
                    ↑   ↑
                  prev curr
```

#### Java
```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null, curr = head;

    while (curr != null) {
        ListNode next = curr.next;  // 保存下一个节点
        curr.next = prev;           // 反转指针
        prev = curr;                // prev 前进
        curr = next;                // curr 前进
    }

    return prev;  // prev 是新的头节点
}
```

#### Python
```python
def reverse_list(head: ListNode) -> ListNode:
    prev, curr = None, head

    while curr:
        next_node = curr.next  # 保存下一个节点
        curr.next = prev       # 反转指针
        prev = curr            # prev 前进
        curr = next_node       # curr 前进

    return prev  # prev 是新的头节点
```

---

## 3. 快慢指针

### 3.1 找中点

```
链表: 1 -> 2 -> 3 -> 4 -> 5
      ^         ^
     slow     fast (步进为2)

第一轮: slow=1, fast=1
第二轮: slow=2, fast=3
第三轮: slow=3, fast=5 (结束)

返回: slow 指向 3 (中点)
```

#### Java
```java
public ListNode findMiddle(ListNode head) {
    ListNode slow = head, fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    return slow;  // 奇数返回中点，偶数返回第二个中点
}
```

#### Python
```python
def find_middle(head: ListNode) -> ListNode:
    slow = fast = head

    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

    return slow  # 奇数返回中点，偶数返回第二个中点
```

---

### 3.2 找倒数第 k 个节点

```
链表: 1 -> 2 -> 3 -> 4 -> 5, k = 2

第一步: fast 先走 k 步
       1 -> 2 -> 3 -> 4 -> 5
       ↑              ↑
      head          fast

第二步: fast 和 slow 一起走
       1 -> 2 -> 3 -> 4 -> 5
            ↑         ↑
          slow      fast

fast 到达末尾，slow 指向倒数第 2 个节点 (4)
```

#### Java
```java
public ListNode findKthFromEnd(ListNode head, int k) {
    ListNode fast = head, slow = head;

    // fast 先走 k 步
    for (int i = 0; i < k; i++) {
        fast = fast.next;
    }

    // fast 和 slow 一起走，直到 fast 到达末尾
    while (fast != null) {
        slow = slow.next;
        fast = fast.next;
    }

    return slow;
}
```

#### Python
```python
def find_kth_from_end(head: ListNode, k: int) -> ListNode:
    fast = slow = head

    # fast 先走 k 步
    for _ in range(k):
        fast = fast.next

    # fast 和 slow 一起走
    while fast:
        slow = slow.next
        fast = fast.next

    return slow
```

---

## 4. 环检测

### 4.1 判断是否有环

```
无环链表: 1 -> 2 -> 3 -> null
                    fast 到达 null

有环链表: 1 -> 2 -> 3
              ^    |
              |    v
              5 <- 4

fast 和 slow 最终会在环内相遇
```

#### Java
```java
public boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            return true;  // 相遇，有环
        }
    }

    return false;  // fast 到达末尾，无环
}
```

#### Python
```python
def has_cycle(head: ListNode) -> bool:
    slow = fast = head

    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:
            return True  # 相遇，有环

    return False  # fast 到达末尾，无环
```

---

## 5. 合并两个有序链表

### 5.1 迭代合并

```
l1: 1 -> 4 -> 7
l2: 2 -> 3 -> 6

合并: 1 -> 2 -> 3 -> 4 -> 6 -> 7

过程: 比较 l1.val 和 l2.val
     选小的加入结果链表
```

#### Java
```java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    ListNode dummy = new ListNode(0);  // 虚拟头节点
    ListNode curr = dummy;

    while (list1 != null && list2 != null) {
        if (list1.val <= list2.val) {
            curr.next = list1;
            list1 = list1.next;
        } else {
            curr.next = list2;
            list2 = list2.next;
        }
        curr = curr.next;
    }

    // 连接剩余部分
    curr.next = (list1 != null) ? list1 : list2;

    return dummy.next;
}
```

#### Python
```python
def merge_two_lists(list1: ListNode, list2: ListNode) -> ListNode:
    dummy = ListNode(0)  # 虚拟头节点
    curr = dummy

    while list1 and list2:
        if list1.val <= list2.val:
            curr.next = list1
            list1 = list1.next
        else:
            curr.next = list2
            list2 = list2.next
        curr = curr.next

    # 连接剩余部分
    curr.next = list1 or list2

    return dummy.next
```

---

## 6. 复杂度总结

| 操作 | 时间复杂度 | 空间复杂度 |
|------|-----------|-----------|
| 遍历 | O(n) | O(1) |
| 反转（迭代） | O(n) | O(1) |
| 反转（递归） | O(n) | O(n) |
| 快慢指针 | O(n) | O(1) |
| 合并有序链表 | O(n + m) | O(1) |
| 环检测 | O(n) | O(1) |
