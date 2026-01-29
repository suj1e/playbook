# 83. 删除排序链表中的重复元素

[中文](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/) | [English](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)

**难度**：Easy
**标签**：链表

---

## 题目描述

给定一个已排序的链表的头 `head`，**删除所有重复的元素**，使每个元素只出现一次。返回已排序的链表。

**示例 1**：
```
输入：head = [1,1,2]
输出：[1,2]
```

**示例 2**：
```
输入：head = [1,1,2,3,3]
输出：[1,2,3]
```

---

## 解题思路

### 方法一：直接遍历（推荐）

**思路**：
- 由于链表已排序，重复元素必然相邻
- 遍历链表，如果当前节点与下一个节点值相同，删除下一个节点
- 继续检查，直到下一个节点值不同

**复杂度**：
- 时间：O(n)
- 空间：O(1)

---

## 代码实现

### 方法一：直接遍历

#### Java

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode curr = head;

        while (curr != null && curr.next != null) {
            if (curr.val == curr.next.val) {
                // 删除下一个节点
                curr.next = curr.next.next;
            } else {
                // 值不同，移动到下一个节点
                curr = curr.next;
            }
        }

        return head;
    }
}
```

#### Python

```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        curr = head

        while curr and curr.next:
            if curr.val == curr.next.val:
                # 删除下一个节点
                curr.next = curr.next.next
            else:
                # 值不同，移动到下一个节点
                curr = curr.next

        return head
```

---

### 方法二：虚拟头节点（不必要）

#### Java

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null) return null;

        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode curr = head;

        while (curr != null && curr.next != null) {
            if (curr.val == curr.next.val) {
                curr.next = curr.next.next;
            } else {
                curr = curr.next;
            }
        }

        return dummy.next;
    }
}
```

#### Python

```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if not head:
            return None

        dummy = ListNode(0)
        dummy.next = head
        curr = head

        while curr and curr.next:
            if curr.val == curr.next.val:
                curr.next = curr.next.next
            else:
                curr = curr.next

        return dummy.next
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 有序链表 | 重复元素相邻，便于判断 |
| 删除操作 | 直接修改 next 指针 |
| 不移动 curr | 删除重复时保持 curr 不变 |
| 时间复杂度 | O(n)，只遍历一次 |

---

## 图解

```
初始：1 -> 1 -> 2 -> 3 -> 3
      curr

步骤1：curr.val = 1, curr.next.val = 1，删除 curr.next
      1 ------> 2 -> 3 -> 3
      curr

步骤2：curr.val = 1, curr.next.val = 2，不同，移动 curr
      1 -> 2 -> 3 -> 3
           curr

步骤3：curr.val = 2, curr.next.val = 3，不同，移动 curr
      1 -> 2 -> 3 -> 3
                curr

步骤4：curr.val = 3, curr.next.val = 3，删除 curr.next
      1 -> 2 -> 3 ------>
                curr

最终：1 -> 2 -> 3
```

---

## 与 "删除所有重复节点" 的区别

本题（LC 83）：保留重复元素的一个
```
输入：1 -> 1 -> 2 -> 3 -> 3
输出：1 -> 2 -> 3
```

LC 82：删除所有重复元素
```
输入：1 -> 1 -> 2 -> 3 -> 3
输出：2
```

---

## 变种/延伸

- **变种1**：删除所有重复元素（LC 82）
- **变种2**：删除无序链表中的重复元素
- **变种3**：删除链表中值为 target 的所有节点
