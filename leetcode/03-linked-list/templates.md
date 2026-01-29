# 链表代码模板

链表是力扣中常见的数据结构，掌握这些模板可以提高解题效率。

---

## 1. 链表定义

### Java

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

### Python

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

## 2. 遍历链表

### Java

```java
// while 循环遍历
ListNode curr = head;
while (curr != null) {
    // 处理 curr.val
    curr = curr.next;
}

// for 循环遍历
for (ListNode curr = head; curr != null; curr = curr.next) {
    // 处理 curr.val
}
```

### Python

```python
# while 循环遍历
curr = head
while curr:
    # 处理 curr.val
    curr = curr.next
```

---

## 3. 反转链表

### 3.1 迭代反转

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

### 3.2 递归反转

#### Java

```java
public ListNode reverseList(ListNode head) {
    // 递归终止条件
    if (head == null || head.next == null) {
        return head;
    }

    // 递归反转后面的链表
    ListNode newHead = reverseList(head.next);

    // 反转当前节点
    head.next.next = head;
    head.next = null;

    return newHead;
}
```

#### Python

```python
def reverse_list(head: ListNode) -> ListNode:
    # 递归终止条件
    if not head or not head.next:
        return head

    # 递归反转后面的链表
    new_head = reverse_list(head.next)

    # 反转当前节点
    head.next.next = head
    head.next = None

    return new_head
```

---

## 4. 快慢指针

### 4.1 找中点

#### Java

```java
public ListNode findMiddle(ListNode head) {
    ListNode slow = head, fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    return slow;  // 奇数个节点返回中点，偶数个返回第二个中点
}
```

#### Python

```python
def find_middle(head: ListNode) -> ListNode:
    slow = fast = head

    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

    return slow  # 奇数个节点返回中点，偶数个返回第二个中点
```

---

### 4.2 找倒数第 k 个节点

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

## 5. 虚拟头节点

**用途**：简化边界处理，统一头节点和非头节点的操作

### Java

```java
public ListNode dummyHeadExample(ListNode head) {
    // 创建虚拟头节点
    ListNode dummy = new ListNode(0);
    dummy.next = head;

    ListNode curr = dummy;
    while (curr.next != null) {
        // 处理 curr.next
        curr = curr.next;
    }

    return dummy.next;  // 返回真正的头节点
}
```

### Python

```python
def dummy_head_example(head: ListNode) -> ListNode:
    # 创建虚拟头节点
    dummy = ListNode(0)
    dummy.next = head

    curr = dummy
    while curr.next:
        # 处理 curr.next
        curr = curr.next

    return dummy.next  # 返回真正的头节点
```

---

## 6. 合并两个有序链表

### 6.1 迭代合并

#### Java

```java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    ListNode dummy = new ListNode(0);
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
    dummy = ListNode(0)
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

### 6.2 递归合并

#### Java

```java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    if (list1 == null) return list2;
    if (list2 == null) return list1;

    if (list1.val <= list2.val) {
        list1.next = mergeTwoLists(list1.next, list2);
        return list1;
    } else {
        list2.next = mergeTwoLists(list1, list2.next);
        return list2;
    }
}
```

#### Python

```python
def merge_two_lists(list1: ListNode, list2: ListNode) -> ListNode:
    if not list1:
        return list2
    if not list2:
        return list1

    if list1.val <= list2.val:
        list1.next = merge_two_lists(list1.next, list2)
        return list1
    else:
        list2.next = merge_two_lists(list1, list2.next)
        return list2
```

---

## 7. 环检测

### 7.1 判断是否有环

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

### 7.2 找环的起点

#### Java

```java
public ListNode detectCycle(ListNode head) {
    ListNode slow = head, fast = head;

    // 第一阶段：找相遇点
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            // 第二阶段：找环的起点
            ListNode ptr1 = head;
            ListNode ptr2 = slow;
            while (ptr1 != ptr2) {
                ptr1 = ptr1.next;
                ptr2 = ptr2.next;
            }
            return ptr1;  // 环的起点
        }
    }

    return null;  // 无环
}
```

#### Python

```python
def detect_cycle(head: ListNode) -> ListNode:
    slow = fast = head

    # 第一阶段：找相遇点
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:
            # 第二阶段：找环的起点
            ptr1 = head
            ptr2 = slow
            while ptr1 is not ptr2:
                ptr1 = ptr1.next
                ptr2 = ptr2.next
            return ptr1  # 环的起点

    return None  # 无环
```

---

## 8. 常用技巧

### 8.1 删除节点

#### Java

```java
// 删除指定节点（已知前驱节点）
public void deleteNode(ListNode prev) {
    prev.next = prev.next.next;
}

// 删除指定节点（只知道要删除的节点本身）
public void deleteNode(ListNode node) {
    node.val = node.next.val;    // 复制下一个节点的值
    node.next = node.next.next;  // 跳过下一个节点
}
```

#### Python

```python
# 删除指定节点（已知前驱节点）
def delete_node(prev: ListNode):
    prev.next = prev.next.next

# 删除指定节点（只知道要删除的节点本身）
def delete_node(node: ListNode):
    node.val = node.next.val
    node.next = node.next.next
```

---

### 8.2 链表长度

#### Java

```java
public int getLength(ListNode head) {
    int length = 0;
    while (head != null) {
        length++;
        head = head.next;
    }
    return length;
}
```

#### Python

```python
def get_length(head: ListNode) -> int:
    length = 0
    while head:
        length += 1
        head = head.next
    return length
```

---

## 9. 复杂度总结

| 操作 | 时间复杂度 | 空间复杂度 |
|------|-----------|-----------|
| 遍历 | O(n) | O(1) |
| 反转（迭代） | O(n) | O(1) |
| 反转（递归） | O(n) | O(n)（递归栈） |
| 快慢指针 | O(n) | O(1) |
| 合并有序链表 | O(n + m) | O(1) |
| 环检测 | O(n) | O(1) |
