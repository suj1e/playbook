# 206. 反转链表

[中文](https://leetcode.cn/problems/reverse-linked-list/) | [English](https://leetcode.com/problems/reverse-linked-list/)

**难度**：Easy
**标签**：链表 | 递归

---

## 题目描述

给你单链表的头节点 `head`，请你反转链表，并返回反转后的链表。

**示例 1**：
```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**示例 2**：
```
输入：head = [1,2]
输出：[2,1]
```

**示例 3**：
```
输入：head = []
输出：[]
```

---

## 解题思路

### 方法一：迭代（推荐）

**思路**：
- 使用三个指针：`prev`、`curr`、`next`
- 遍历链表，逐个反转指针方向
- `prev` 初始为 `null`，最终成为新的头节点

**复杂度**：
- 时间：O(n)
- 空间：O(1)

**步骤**：
1. 初始化 `prev = null`，`curr = head`
2. 遍历链表：
   - 保存 `next = curr.next`
   - 反转 `curr.next = prev`
   - 移动指针：`prev = curr`，`curr = next`
3. 返回 `prev`

**图解**：
```
初始状态：
null <- 1 -> 2 -> 3 -> null
 prev   curr

第一步：
null <- 1    2 -> 3 -> null
       prev  curr

第二步：
null <- 1 <- 2    3 -> null
            prev  curr

第三步：
null <- 1 <- 2 <- 3    null
                 prev  curr

返回 prev（新头节点）
```

---

### 方法二：递归

**思路**：
- 递归反转链表的后半部分
- 然后反转当前节点

**复杂度**：
- 时间：O(n)
- 空间：O(n)（递归栈）

---

## 代码实现

### 方法一：迭代

#### Java

```java
class Solution {
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
}
```

#### Python

```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        prev, curr = None, head

        while curr:
            next_node = curr.next  # 保存下一个节点
            curr.next = prev       # 反转指针
            prev = curr            # prev 前进
            curr = next_node       # curr 前进

        return prev  # prev 是新的头节点
```

---

### 方法二：递归

#### Java

```java
class Solution {
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
}
```

#### Python

```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        # 递归终止条件
        if not head or not head.next:
            return head

        # 递归反转后面的链表
        new_head = self.reverseList(head.next)

        # 反转当前节点
        head.next.next = head
        head.next = None

        return new_head
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 迭代方法 | 三个指针：prev、curr、next |
| 递归方法 | 先递归到末尾，再逐层返回反转 |
| 边界处理 | 空链表和单节点链表直接返回 |
| 新头节点 | 返回 prev（迭代）或 newHead（递归） |

---

## 方法对比

| 方法 | 时间 | 空间 | 说明 |
|------|------|------|------|
| 迭代 | O(n) | O(1) | **推荐** |
| 递归 | O(n) | O(n) | 代码简洁，有栈溢出风险 |

---

## 变种/延伸

- **变种1**：反转链表 II（指定区间反转）
- **变种2**：两两交换链表中的节点
- **变种3**：K 个一组翻转链表
