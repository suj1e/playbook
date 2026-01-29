# 142. 环形链表 II

[中文](https://leetcode.cn/problems/linked-list-cycle-ii/) | [English](https://leetcode.com/problems/linked-list-cycle-ii/)

**难度**：Medium
**标签**：链表 | 双指针

---

## 题目描述

给定一个链表的头节点 `head`，返回链表开始入环的第一个节点。如果链表无环，则返回 `null`。

不允许修改链表。

**示例 1**：
```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2**：
```
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
```

**示例 3**：
```
输入：head = [1], pos = -1
输出：返回 null
```

---

## 解题思路

### 方法一：快慢指针 + 数学推导（推荐）

**思路**：
1. 第一阶段：使用快慢指针判断是否有环，找到相遇点
2. 第二阶段：一个指针从头部开始，一个从相遇点开始，每次走一步，相遇点就是环的起点

**数学证明**：
```
设：
- 链表头到环起点的距离为 a
- 环起点到相遇点的距离为 b
- 相遇点到环起点的距离为 c
- 环的长度为 L = b + c

快指针走的距离：a + b + c + b = a + 2b + c
慢指针走的距离：a + b

快指针速度是慢指针的 2 倍：
a + 2b + c = 2(a + b)
a + 2b + c = 2a + 2b
c = a

所以：从头到环起点的距离 = 从相遇点到环起点的距离
```

**复杂度**：
- 时间：O(n)
- 空间：O(1)

**步骤**：
1. 初始化 `slow = fast = head`
2. 第一阶段：找相遇点
   - `slow` 每次走 1 步，`fast` 每次走 2 步
   - 如果 `fast` 到达 `null`，无环
   - 如果 `slow == fast`，找到相遇点
3. 第二阶段：找环的起点
   - `ptr1 = head`，`ptr2 = slow`（相遇点）
   - 每次各走 1 步，直到 `ptr1 == ptr2`
   - 返回 `ptr1`

**图解**：
```
第一阶段：
head -> a -> b -> c -> d -> e
                ^         |
                |         |
                f <-------+

slow: a -> b -> c -> d -> e -> f -> c
fast: a -> c -> e -> d -> f -> d -> f -> d

第二阶段（从相遇点 c 开始）：
ptr1: a -> b -> c
ptr2: c -> d -> e -> f -> c

在 c 点相遇，c 就是环的起点
```

---

### 方法二：哈希表

**思路**：
- 遍历链表，将访问过的节点存入集合
- 如果当前节点已在集合中，说明是环的起点
- 如果到达 `null`，无环

**复杂度**：
- 时间：O(n)
- 空间：O(n)

---

## 代码实现

### 方法一：快慢指针

#### Java

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        // 第一阶段：找相遇点
        ListNode slow = head, fast = head;

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
}
```

#### Python

```python
class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        # 第一阶段：找相遇点
        slow = fast = head

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

### 方法二：哈希表

#### Java

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        Set<ListNode> visited = new HashSet<>();

        while (head != null) {
            if (visited.contains(head)) {
                return head;  // 环的起点
            }
            visited.add(head);
            head = head.next;
        }

        return null;  // 无环
    }
}
```

#### Python

```python
class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        visited = set()

        while head:
            if head in visited:
                return head  # 环的起点
            visited.add(head)
            head = head.next

        return None  # 无环
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 两阶段法 | 先找相遇点，再找环的起点 |
| 数学推导 | 从头到环起点的距离 = 从相遇点到环起点的距离 |
| 边界处理 | 注意 `fast.next` 可能为 `null` |
| 时间复杂度 | O(n)，只遍历链表常数次 |

---

## 图解：快慢指针如何找到相遇点

```
假设环的长度是 L，head 到环起点距离是 a

第 1 步：
slow: head -> node1
fast: head -> node2

第 2 步：
slow: node1 -> node2
fast: node2 -> node4

...（继续移动）

最终 slow 和 fast 会在环内某点相遇
```

---

## 方法对比

| 方法 | 时间 | 空间 | 说明 |
|------|------|------|------|
| 快慢指针 | O(n) | O(1) | **推荐** |
| 哈希表 | O(n) | O(n) | 思路简单 |

---

## 变种/延伸

- **变种1**：判断链表是否有环（LC 141）
- **变种2**：找到环的长度
- **变种3**：两个链表是否相交（相交问题可以转为环问题）
