# 21. 合并两个有序链表

[中文](https://leetcode.cn/problems/merge-two-sorted-lists/) | [English](https://leetcode.com/problems/merge-two-sorted-lists/)

**难度**：Easy
**标签**：链表 | 递归

---

## 题目描述

将两个升序链表合并为一个新的**升序**链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。

**示例 1**：
```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

**示例 2**：
```
输入：l1 = [], l2 = []
输出：[]
```

**示例 3**：
```
输入：l1 = [], l2 = [0]
输出：[0]
```

---

## 解题思路

### 方法一：迭代（推荐）

**思路**：
- 使用虚拟头节点简化操作
- 比较两个链表当前节点的值，选择较小的接入结果链表
- 处理完一个链表后，直接连接另一个链表的剩余部分

**复杂度**：
- 时间：O(n + m)
- 空间：O(1)

---

### 方法二：递归

**思路**：
- 比较两个头节点，选择较小的作为结果
- 递归处理剩余部分

**复杂度**：
- 时间：O(n + m)
- 空间：O(n + m)（递归栈）

---

## 代码实现

### 方法一：迭代

#### Java

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        // 虚拟头节点
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;

        // 比较并合并
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
}
```

#### Python

```python
class Solution:
    def mergeTwoLists(self, list1: ListNode, list2: ListNode) -> ListNode:
        # 虚拟头节点
        dummy = ListNode(0)
        curr = dummy

        # 比较并合并
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

### 方法二：递归

#### Java

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        // 递归终止条件
        if (list1 == null) return list2;
        if (list2 == null) return list1;

        // 选择较小的节点
        if (list1.val <= list2.val) {
            list1.next = mergeTwoLists(list1.next, list2);
            return list1;
        } else {
            list2.next = mergeTwoLists(list1, list2.next);
            return list2;
        }
    }
}
```

#### Python

```python
class Solution:
    def mergeTwoLists(self, list1: ListNode, list2: ListNode) -> ListNode:
        # 递归终止条件
        if not list1:
            return list2
        if not list2:
            return list1

        # 选择较小的节点
        if list1.val <= list2.val:
            list1.next = self.mergeTwoLists(list1.next, list2)
            return list1
        else:
            list2.next = self.mergeTwoLists(list1, list2.next)
            return list2
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 虚拟头节点 | 简化边界处理 |
| 剩余部分 | 只需直接连接，无需再比较 |
| 递归思路 | 每次选择较小的头节点 |
| 时间复杂度 | O(n + m)，n 和 m 是两个链表的长度 |

---

## 图解

```
list1: 1 -> 2 -> 4
list2: 1 -> 3 -> 4

步骤1: 选择 list1[1]
dummy -> 1

list1: 2 -> 4
list2: 1 -> 3 -> 4

步骤2: 选择 list2[1]
dummy -> 1 -> 1

list1: 2 -> 4
list2: 3 -> 4

步骤3: 选择 list1[2]
dummy -> 1 -> 1 -> 2

list1: 4
list2: 3 -> 4

步骤4: 选择 list2[3]
dummy -> 1 -> 1 -> 2 -> 3

list1: 4
list2: 4

步骤5: 选择 list1[4]
dummy -> 1 -> 1 -> 2 -> 3 -> 4

list1: null
list2: 4

步骤6: 连接剩余 list2
dummy -> 1 -> 1 -> 2 -> 3 -> 4 -> 4
```

---

## 方法对比

| 方法 | 时间 | 空间 | 说明 |
|------|------|------|------|
| 迭代 | O(n + m) | O(1) | **推荐** |
| 递归 | O(n + m) | O(n + m) | 代码简洁 |

---

## 变种/延伸

- **变种1**：合并 K 个有序链表
- **变种2**：合并两个降序链表
- **变种3**：判断一个链表是否由两个有序链表合并而成
