# 数组专题

## 专题概述

数组是最基础的数据结构，也是力扣中最常见的数据结构之一。本专题涵盖：

- **双指针**：利用两个指针在数组中移动，降低时间复杂度
- **滑动窗口**：处理子数组/子串问题的利器
- **二分查找**：在有序数组中快速查找
- **基础操作**：遍历、修改、统计等

### 核心知识点

1. 数组的随机访问特性：O(1) 时间访问任意位置
2. 双指针技巧：相向指针、同向双指针（滑动窗口）、快慢指针
3. 二分查找：有序数组中的高效查找
4. 滑动窗口：处理连续子数组问题

---

## 学习路径

1. 先阅读 [templates.md](./templates.md) 熟悉数组操作模板
2. 按顺序完成各分类题目：
   - [ ] 01-two-pointers（双指针基础，必做）
   - [ ] 02-sliding-window（滑动窗口，中等）
   - [ ] 03-binary-search（二分查找，必做）
   - [ ] 04-others（其他数组问题）

---

## 题目分类

| 分类 | 题目数 | 难度 | 说明 |
|------|-------|------|------|
| 01-two-pointers | 3 | ⭐⭐ | 基础必会，相向双指针 |
| 02-sliding-window | 3 | ⭐⭐⭐ | 滑动窗口固定/可变 |
| 03-binary-search | 3 | ⭐⭐ | 二分查找及其变形 |
| 04-others | 2 | ⭐⭐⭐ | 哈希表辅助的数组问题 |

---

## 进度追踪

### 01-two-pointers
- [ ] lc-167-two-sum-ii (Easy)
- [ ] lc-125-valid-palindrome (Easy)
- [ ] lc-11-container-with-most-water (Medium)

### 02-sliding-window
- [ ] lc-209-min-size-subarray (Medium)
- [ ] lc-3-longest-substring-without-repeating (Medium)
- [ ] lc-438-find-all-anagrams (Medium)

### 03-binary-search
- [ ] lc-704-binary-search (Easy)
- [ ] lc-35-search-insert-position (Easy)
- [ ] lc-33-search-in-rotated-array (Medium)

### 04-others
- [ ] lc-1-two-sum (Easy)
- [ ] lc-15-3sum (Medium)

---

## 解题模板总结

### 双指针模板
```java
// 相向双指针
int left = 0, right = n - 1;
while (left < right) {
    int sum = arr[left] + arr[right];
    if (sum == target) {
        // 找到答案
        left++; right--;
    } else if (sum < target) {
        left++;
    } else {
        right--;
    }
}
```

### 滑动窗口模板
```java
// 固定大小窗口
for (int i = 0; i < n; i++) {
    // 进入窗口
    window.add(arr[i]);
    if (i >= k) {
        // 离开窗口
        window.remove(arr[i - k]);
    }
    if (i >= k - 1) {
        // 更新答案
    }
}

// 可变大小窗口
int left = 0;
for (int right = 0; right < n; right++) {
    // 进入窗口
    window.add(arr[right]);
    while (window需要收缩) {
        // 离开窗口
        window.remove(arr[left]);
        left++;
    }
    // 更新答案
}
```

### 二分查找模板
```java
int left = 0, right = n - 1;
while (left <= right) {
    int mid = left + (right - left) / 2;
    if (arr[mid] == target) {
        return mid;
    } else if (arr[mid] < target) {
        left = mid + 1;
    } else {
        right = mid - 1;
    }
}
return -1;
```

---

## 参考资源

- [LeetCode 数组标签](https://leetcode.cn/tag/array/)
- [LeetCode 双指针标签](https://leetcode.cn/tag/two-pointers/)
