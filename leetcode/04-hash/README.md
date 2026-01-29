# 哈希表专题

## 专题概述

哈希表是基于键值对存储的数据结构，提供 O(1) 的查找、插入、删除操作。本专题涵盖：

- **频率统计**：统计元素出现次数
- **子数组问题**：使用前缀和 + 哈希表
- **分组问题**：按照某种特征分组

### 核心知识点

1. 哈希表的时间复杂度：平均 O(1)，最坏 O(n)
2. 哈希冲突的解决方法
3. 哈希表的应用场景

---

## 学习路径

1. 先阅读 [templates.md](./templates.md) 熟悉哈希表操作模板
2. 按顺序完成各分类题目：
   - [ ] 01-frequency（频率统计）
   - [ ] 02-subarray（子数组问题）
   - [ ] 03-grouping（分组问题）

---

## 题目分类

| 分类 | 题目数 | 难度 | 说明 |
|------|-------|------|------|
| 01-frequency | 2 | ⭐⭐ | 统计频率，TopK |
| 02-subarray | 2 | ⭐⭐⭐ | 前缀和 + 哈希表 |
| 03-grouping | 1 | ⭐⭐ | 按特征分组 |

---

## 进度追踪

### 01-frequency
- [ ] lc-1-two-sum (Easy)
- [ ] lc-347-top-k-frequent-elements (Medium)

### 02-subarray
- [ ] lc-560-subarray-sum-equals-k (Medium)
- [ ] lc-128-longest-consecutive-sequence (Medium)

### 03-grouping
- [ ] lc-49-group-anagrams (Medium)

---

## 解题模板总结

### 频率统计模板
```java
Map<Integer, Integer> freq = new HashMap<>();
for (int num : nums) {
    freq.put(num, freq.getOrDefault(num, 0) + 1);
}
```

### 前缀和 + 哈希表模板
```java
Map<Integer, Integer> prefixCount = new HashMap<>();
prefixCount.put(0, 1);  // 前缀和为 0 出现 1 次
int sum = 0;
for (int num : nums) {
    sum += num;
    int target = sum - k;
    count += prefixCount.getOrDefault(target, 0);
    prefixCount.put(sum, prefixCount.getOrDefault(sum, 0) + 1);
}
```

---

## 参考资源

- [LeetCode 哈希表标签](https://leetcode.cn/tag/hash-table/)
