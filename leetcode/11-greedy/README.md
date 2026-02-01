# 11-greedy / 贪心专题

## 专题概述

贪心算法在每一步选择中都采取当前状态下最优的选择，本专题涵盖：

- **区间问题**：区间合并、区间调度
- **调度问题**：任务分配、资源分配

### 核心知识点

1. 贪心选择性质：局部最优 → 全局最优
2. 最优子结构
3. 贪心策略的正确性证明

---

## 学习路径

1. 先阅读 [templates.md](./templates.md) 熟悉贪心思想
2. 按顺序完成各分类题目：
   - [x] 01-interval（区间问题）
   - [ ] 02-scheduling（调度问题）

---

## 题目分类

| 分类 | 题目数 | 难度 | 说明 |
|------|-------|------|------|
| 01-interval | 2 | ⭐⭐⭐ | 区间合并、无重叠区间 |
| 02-scheduling | 1 | ⭐⭐ | 分发饼干 |

---

## 进度追踪

### 01-interval
- [x] lc-56-merge-intervals (Medium)
- [ ] lc-435-non-overlapping-intervals (Medium)

### 02-scheduling
- [ ] lc-455-assign-cookies (Easy)

---

## 解题模板总结

### 区间排序

```java
// 按区间起点排序
Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

// 按区间终点排序
Arrays.sort(intervals, (a, b) -> a[1] - b[1]);
```

### 区间合并模板

```java
public int[][] merge(int[][] intervals) {
    if (intervals.length <= 1) return intervals;

    // 按起点排序
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

    List<int[]> merged = new ArrayList<>();
    int[] current = intervals[0];

    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] <= current[1]) {
            // 有重叠，合并
            current[1] = Math.max(current[1], intervals[i][1]);
        } else {
            // 无重叠，加入结果
            merged.add(current);
            current = intervals[i];
        }
    }
    merged.add(current);

    return merged.toArray(new int[merged.size()][]);
}
```

### 无重叠区间模板

```java
public int eraseOverlapIntervals(int[][] intervals) {
    if (intervals.length <= 1) return 0;

    // 按终点排序（贪心：选择最早结束的区间）
    Arrays.sort(intervals, (a, b) -> a[1] - b[1]);

    int count = 1;
    int end = intervals[0][1];

    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] >= end) {
            // 无重叠，选择这个区间
            count++;
            end = intervals[i][1];
        }
    }

    return intervals.length - count;
}
```

---

## 参考资源

- [LeetCode 贪心标签](https://leetcode.cn/tag/greedy/)
