# 链表专题

## 专题概述

链表是一种基础的数据结构，本专题涵盖：

- **基础操作**：反转、合并、删除等
- **双指针**：快慢指针、倒数第 k 个节点
- **环检测**：判断链表是否有环、找环的起点

### 核心知识点

1. 链表的结构：单向、双向、循环
2. 虚拟头节点：简化边界处理
3. 快慢指针：找中点、倒数第 k 个
4. 链表反转：迭代和递归

---

## 学习路径

1. 先阅读 [templates.md](./templates.md) 熟悉链表操作模板
2. 按顺序完成各分类题目：
   - [ ] 01-basic（基础操作，必做）
   - [ ] 02-two-pointers（快慢指针）
   - [ ] 03-cycle（环检测）

---

## 题目分类

| 分类 | 题目数 | 难度 | 说明 |
|------|-------|------|------|
| 01-basic | 3 | ⭐⭐ | 基础操作，必会 |
| 02-two-pointers | 2 | ⭐⭐⭐ | 快慢指针应用 |
| 03-cycle | 1 | ⭐⭐ | 环检测 |

---

## 进度追踪

### 01-basic
- [ ] lc-206-reverse-linked-list (Easy)
- [ ] lc-21-merge-two-sorted-lists (Easy)
- [ ] lc-83-remove-duplicates (Easy)

### 02-two-pointers
- [ ] lc-19-remove-nth-node-from-end (Medium)
- [ ] lc-142-linked-list-cycle-ii (Medium)

### 03-cycle
- [ ] lc-141-linked-list-cycle (Easy)

---

## 解题模板总结

### 链表定义
```java
// 单向链表
class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}
```

```python
# 单向链表
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

### 反转链表模板
```java
ListNode prev = null, curr = head;
while (curr != null) {
    ListNode next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
}
return prev;
```

### 快慢指针模板
```java
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}
```

---

## 参考资源

- [LeetCode 链表标签](https://leetcode.cn/tag/linked-list/)
