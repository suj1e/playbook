# 19. 删除链表的倒数第 N 个节点

[中文](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/) | [English](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

**难度**：Medium
**标签**：链表 | 双指针

---

## 题目描述

给你一个链表，删除链表的倒数第 `n` 个节点，并且返回链表的头节点。

**示例 1**：
```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**示例 2**：
```
输入：head = [1], n = 1
输出：[]
```

**示例 3**：
```
输入：head = [1,2], n = 1
输出：[1]
```

---

## 解题思路

### 方法一：双指针（推荐）

**思路**：
- 使用快慢指针，`fast` 先走 `n` 步
- 然后 `fast` 和 `slow` 一起走，直到 `fast` 到达末尾
- 此时 `slow` 的下一个节点就是要删除的节点

**关键**：使用虚拟头节点，统一处理删除头节点的情况

**复杂度**：
- 时间：O(n)
- 空间：O(1)

**步骤**：
1. 创建虚拟头节点
2. `fast` 先走 `n + 1` 步（这样 `slow` 会停在要删除节点的前一个位置）
3. `fast` 和 `slow` 一起走，直到 `fast` 到达末尾
4. 删除 `slow.next`
5. 返回 `dummy.next`

**图解**：
```
初始状态（n = 2）：
dummy -> 1 -> 2 -> 3 -> 4 -> 5 -> null
 fast
 slow

fast 先走 3 步（n + 1）：
dummy -> 1 -> 2 -> 3 -> 4 -> 5 -> null
              fast
 slow

一起走，直到 fast 到达末尾：
dummy -> 1 -> 2 -> 3 -> 4 -> 5 -> null
                          fast
         slow

删除 slow.next：
dummy -> 1 -> 2 -> 3 ------> 5 -> null
```

---

### 方法二：两次遍历

**思路**：
- 第一次遍历获取链表长度
- 计算要删除的正数第几个节点
- 第二次遍历删除该节点

**复杂度**：
- 时间：O(n)
- 空间：O(1)

---

## 代码实现

### 方法一：双指针（一次遍历）

#### Java

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // 虚拟头节点
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode fast = dummy, slow = dummy;

        // fast 先走 n + 1 步
        for (int i = 0; i <= n; i++) {
            fast = fast.next;
        }

        // fast 和 slow 一起走
        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }

        // 删除 slow.next
        slow.next = slow.next.next;

        return dummy.next;
    }
}
```

#### Python

```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        # 虚拟头节点
        dummy = ListNode(0)
        dummy.next = head

        fast = slow = dummy

        # fast 先走 n + 1 步
        for _ in range(n + 1):
            fast = fast.next

        # fast 和 slow 一起走
        while fast:
            fast = fast.next
            slow = slow.next

        # 删除 slow.next
        slow.next = slow.next.next

        return dummy.next
```

---

### 方法二：两次遍历

#### Java

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // 虚拟头节点
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        // 第一次遍历：获取长度
        int length = 0;
        ListNode curr = head;
        while (curr != null) {
            length++;
            curr = curr.next;
        }

        // 计算要删除的位置（从头开始）
        int target = length - n;

        // 第二次遍历：找到要删除节点的前一个节点
        curr = dummy;
        for (int i = 0; i < target; i++) {
            curr = curr.next;
        }

        // 删除
        curr.next = curr.next.next;

        return dummy.next;
    }
}
```

#### Python

```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        # 虚拟头节点
        dummy = ListNode(0)
        dummy.next = head

        # 第一次遍历：获取长度
        length = 0
        curr = head
        while curr:
            length += 1
            curr = curr.next

        # 计算要删除的位置（从头开始）
        target = length - n

        # 第二次遍历：找到要删除节点的前一个节点
        curr = dummy
        for _ in range(target):
            curr = curr.next

        # 删除
        curr.next = curr.next.next

        return dummy.next
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 虚拟头节点 | 统一处理删除头节点的情况 |
| fast 先走 n+1 步 | 让 slow 停在要删除节点的前一个位置 |
| 一次遍历 | 双指针方法只需一次遍历 |
| 两次遍历 | 更直观，但需要遍历两次 |

---

## 为什么 fast 先走 n+1 步？

如果 `fast` 先走 `n` 步，那么当 `fast` 到达末尾时，`slow` 会停在要删除的节点上，无法获取其前驱节点。

先走 `n + 1` 步后：
- 当 `fast` 到达末尾（`null`）时
- `slow` 正好停在要删除节点的前一个位置
- 可以直接执行 `slow.next = slow.next.next`

---

## 方法对比

| 方法 | 时间 | 遍历次数 | 说明 |
|------|------|---------|------|
| 双指针 | O(n) | 1 次 | **推荐** |
| 两次遍历 | O(n) | 2 次 | 思路直观 |

---

## 变种/延伸

- **变种1**：删除链表的中间节点
- **变种2**：交换链表的相邻节点
- **变种3**：旋转链表
