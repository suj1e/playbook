# 141. 环形链表

[中文](https://leetcode.cn/problems/linked-list-cycle/) | [English](https://leetcode.com/problems/linked-list-cycle/)

**难度**：Easy
**标签**：链表 | 双指针

---

## 题目描述

给你一个链表的头节点 `head`，判断链表中是否有环。

如果链表中存在环，则返回 `true`。否则，返回 `false`。

**示例 1**：
```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2**：
```
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3**：
```
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

---

## 解题思路

### 方法一：快慢指针（推荐）

**思路**：
- 使用快慢指针，`slow` 每次走 1 步，`fast` 每次走 2 步
- 如果链表有环，`fast` 最终会追上 `slow`
- 如果链表无环，`fast` 会到达 `null`

**为什么有效？**
- 有环时：`fast` 相对于 `slow` 每次追赶 1 步，最终一定会相遇
- 无环时：`fast` 会先到达末尾

**复杂度**：
- 时间：O(n)
- 空间：O(1)

**步骤**：
1. 初始化 `slow = fast = head`
2. 循环条件：`fast != null && fast.next != null`
3. 每次移动：`slow` 走 1 步，`fast` 走 2 步
4. 检查：如果 `slow == fast`，有环
5. 循环结束：`fast` 到达 `null`，无环

**图解**：
```
有环的情况：
1 -> 2 -> 3 -> 4
         ^     |
         |     |
         6 <- 5

步骤1: slow=1, fast=1
步骤2: slow=2, fast=3
步骤3: slow=3, fast=5
步骤4: slow=4, fast=3
步骤5: slow=5, fast=5（相遇！）

无环的情况：
1 -> 2 -> 3 -> 4 -> 5 -> null

步骤1: slow=1, fast=1
步骤2: slow=2, fast=3
步骤3: slow=3, fast=5
步骤4: slow=4, fast=null（结束，无环）
```

---

### 方法二：哈希表

**思路**：
- 遍历链表，将访问过的节点存入集合
- 如果当前节点已在集合中，说明有环
- 如果到达 `null`，无环

**复杂度**：
- 时间：O(n)
- 空间：O(n)

---

### 方法三：标记法（修改节点值）

**思路**：
- 遍历时修改节点值（如设为特殊值）
- 如果遇到已标记的节点，说明有环

**注意**：此方法会修改原链表

**复杂度**：
- 时间：O(n)
- 空间：O(1)

---

## 代码实现

### 方法一：快慢指针

#### Java

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        // 处理边界情况
        if (head == null || head.next == null) {
            return false;
        }

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
}
```

#### Python

```python
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        # 处理边界情况
        if not head or not head.next:
            return False

        slow = fast = head

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

            if slow is fast:
                return True  # 相遇，有环

        return False  # fast 到达末尾，无环
```

---

### 方法二：哈希表

#### Java

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        Set<ListNode> visited = new HashSet<>();

        while (head != null) {
            if (visited.contains(head)) {
                return true;  // 已经访问过，有环
            }
            visited.add(head);
            head = head.next;
        }

        return false;  // 到达 null，无环
    }
}
```

#### Python

```python
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        visited = set()

        while head:
            if head in visited:
                return True  # 已经访问过，有环
            visited.add(head)
            head = head.next

        return False  # 到达 null，无环
```

---

### 方法三：标记法

#### Java

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        while (head != null) {
            // 如果节点值已被标记，说明有环
            if (head.val == Integer.MAX_VALUE) {
                return true;
            }
            // 标记节点
            head.val = Integer.MAX_VALUE;
            head = head.next;
        }
        return false;
    }
}
```

#### Python

```python
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        while head:
            # 如果节点值已被标记，说明有环
            if head.val == 10**6:
                return True
            # 标记节点
            head.val = 10**6
            head = head.next
        return False
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 快慢指针 | `slow` 走 1 步，`fast` 走 2 步 |
| 循环条件 | `fast != null && fast.next != null` |
| 相遇判断 | `slow == fast` 表示有环 |
| 边界处理 | 空链表或单节点直接返回 false |

---

## 为什么快慢指针一定会相遇？

假设：
- 环的长度是 L
- `slow` 进入环时，`fast` 已经在环内
- `fast` 相对于 `slow` 每次追赶 1 步

最多 L 次移动后，`fast` 一定会追上 `slow`（相当于追及问题）

---

## 方法对比

| 方法 | 时间 | 空间 | 是否修改原链表 | 说明 |
|------|------|------|--------------|------|
| 快慢指针 | O(n) | O(1) | 否 | **推荐** |
| 哈希表 | O(n) | O(n) | 否 | 思路简单 |
| 标记法 | O(n) | O(1) | 是 | 修改原链表 |

---

## 变种/延伸

- **变种1**：找环的起点（LC 142）
- **变种2**：找环的长度
- **变种3**：判断两个链表是否相交
