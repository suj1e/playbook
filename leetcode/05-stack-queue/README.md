# 栈和队列专题

## 专题概述

栈和队列是两种重要的线性数据结构，本专题涵盖：

- **单调栈**：寻找下一个更大/更小元素
- **队列**：滑动窗口最大值
- **括号匹配**：栈的经典应用

### 核心知识点

1. 栈：后进先出（LIFO）
2. 队列：先进先出（FIFO）
3. 双端队列：支持两端操作

---

## 学习路径

1. 先阅读 [templates.md](./templates.md) 熟悉栈队列操作模板
2. 按顺序完成各分类题目：
   - [ ] 01-mono-stack（单调栈）
   - [ ] 02-queue（队列问题）
   - [ ] 03-parentheses（括号匹配）

---

## 题目分类

| 分类 | 题目数 | 难度 | 说明 |
|------|-------|------|------|
| 01-mono-stack | 3 | ⭐⭐⭐ | 寻找下一个更大元素 |
| 02-queue | 1 | ⭐⭐⭐ | 滑动窗口最大值 |
| 03-parentheses | 2 | ⭐⭐ | 括号匹配 |

---

## 进度追踪

### 01-mono-stack
- [ ] lc-739-daily-temperatures (Medium)
- [ ] lc-496-next-greater-element-i (Easy)
- [ ] lc-84-largest-rectangle-in-histogram (Hard)

### 02-queue
- [ ] lc-239-sliding-window-maximum (Hard)

### 03-parentheses
- [ ] lc-20-valid-parentheses (Easy)
- [ ] lc-22-generate-parentheses (Medium)

---

## 解题模板总结

### 单调栈模板（递增）
```java
Deque<Integer> stack = new ArrayDeque<>();
for (int i = 0; i < nums.length; i++) {
    while (!stack.isEmpty() && nums[stack.peek()] < nums[i]) {
        int idx = stack.pop();
        // 处理 idx 的下一个更大元素是 i
    }
    stack.push(i);
}
```

### 滑动窗口最大值模板
```java
Deque<int[]> deque = new ArrayDeque<>();  // 存储 [索引, 值]
for (int i = 0; i < nums.length; i++) {
    // 移除超出窗口的元素
    if (!deque.isEmpty() && deque.peekFirst()[0] <= i - k) {
        deque.pollFirst();
    }
    // 维护单调递减
    while (!deque.isEmpty() && deque.peekLast()[1] < nums[i]) {
        deque.pollLast();
    }
    deque.offerLast(new int[]{i, nums[i]});
    if (i >= k - 1) {
        result[i - k + 1] = deque.peekFirst()[1];
    }
}
```

---

## 参考资源

- [LeetCode 栈标签](https://leetcode.cn/tag/stack/)
- [LeetCode 队列标签](https://leetcode.cn/tag/queue/)
