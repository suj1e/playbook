# 力扣刷题手册 - 内容规划

> 本文档详细规划了整个力扣刷题手册的内容结构，用于追踪完成进度。

---

## 目录结构

```
leetcode/
├── README.md                    # 总览：学习路线、进度追踪
│
├── 00-templates/                # 通用模板（所有专题共用）
│   ├── java-basic.md            # Java：输入输出、常用类
│   ├── python-basic.md          # Python：输入输出、常用库
│   └── language-comparison.md   # Java vs Python 语法对照
│
├── 01-array/                    # 数组专题
│   ├── README.md                # 专题概述 + 学习路径
│   ├── templates.md             # 数组操作模板
│   ├── 01-two-pointers/         # 分类：双指针
│   │   ├── lc-167-two-sum-ii.md
│   │   ├── lc-125-valid-palindrome.md
│   │   └── lc-11-container-with-most-water.md
│   ├── 02-sliding-window/       # 分类：滑动窗口
│   │   ├── lc-209-min-size-subarray.md
│   │   ├── lc-3-longest-substring-without-repeating.md
│   │   └── lc-438-find-all-anagrams.md
│   ├── 03-binary-search/        # 分类：二分查找
│   │   ├── lc-704-binary-search.md
│   │   ├── lc-35-search-insert-position.md
│   │   └── lc-33-search-in-rotated-array.md
│   └── 04-others/               # 其他类型
│       ├── lc-1-two-sum.md
│       └── lc-15-3sum.md
│
├── 02-string/                   # 字符串专题
│   ├── README.md
│   ├── templates.md
│   ├── 01-two-pointers/
│   │   ├── lc-344-reverse-string.md
│   │   └── lc-5-longest-palindromic-substring.md
│   ├── 02-kmp/
│   │   ├── lc-28-find-the-index-of-the-first-occurrence.md
│   │   └── lc-459-repeated-substring-pattern.md
│   └── 03-subsequence/
│       ├── lc-392-is-subsequence.md
│       └── lc-516-longest-palindromic-subsequence.md
│
├── 03-linked-list/              # 链表专题
│   ├── README.md
│   ├── templates.md
│   ├── 01-basic/
│   │   ├── lc-206-reverse-linked-list.md
│   │   ├── lc-21-merge-two-sorted-lists.md
│   │   └── lc-83-remove-duplicates.md
│   ├── 02-two-pointers/
│   │   ├── lc-19-remove-nth-node-from-end.md
│   │   └── lc-142-linked-list-cycle-ii.md
│   └── 03-cycle/
│       └── lc-141-linked-list-cycle.md
│
├── 04-hash/                     # 哈希表专题
│   ├── README.md
│   ├── templates.md
│   ├── 01-frequency/
│   │   ├── lc-1-two-sum.md
│   │   └── lc-347-top-k-frequent-elements.md
│   ├── 02-subarray/
│   │   ├── lc-560-subarray-sum-equals-k.md
│   │   └── lc-128-longest-consecutive-sequence.md
│   └── 03-grouping/
│       ├── lc-49-group-anagrams.md
│       └── lc-347-top-k-frequent-elements.md
│
├── 05-stack-queue/              # 栈和队列
│   ├── README.md
│   ├── templates.md
│   ├── 01-mono-stack/
│   │   ├── lc-739-daily-temperatures.md
│   │   ├── lc-496-next-greater-element-i.md
│   │   └── lc-84-largest-rectangle-in-histogram.md
│   ├── 02-queue/
│   │   └── lc-239-sliding-window-maximum.md
│   └── 03-parentheses/
│       ├── lc-20-valid-parentheses.md
│       └── lc-22-generate-parentheses.md
│
├── 06-tree/                     # 二叉树专题
│   ├── README.md
│   ├── templates.md
│   ├── 01-traversal/
│   │   ├── lc-144-binary-tree-preorder-traversal.md
│   │   ├── lc-94-binary-tree-inorder-traversal.md
│   │   ├── lc-145-binary-tree-postorder-traversal.md
│   │   └── lc-102-binary-tree-level-order-traversal.md
│   ├── 02-properties/
│   │   ├── lc-104-maximum-depth-of-binary-tree.md
│   │   ├── lc-110-balanced-binary-tree.md
│   │   └── lc-236-lowest-common-ancestor-of-a-binary-tree.md
│   ├── 03-bst/
│   │   ├── lc-98-validate-binary-search-tree.md
│   │   └── lc-230-kth-smallest-element-in-a-bst.md
│   └── 04-lca/
│       └── lc-236-lowest-common-ancestor-of-a-binary-tree.md
│
├── 07-heap/                     # 堆专题
│   ├── README.md
│   ├── templates.md
│   ├── 01-top-k/
│   │   ├── lc-215-kth-largest-element-in-an-array.md
│   │   └── lc-347-top-k-frequent-elements.md
│   └── 02-merge/
│       └── lc-23-merge-k-sorted-lists.md
│
├── 08-graph/                    # 图专题
│   ├── README.md
│   ├── templates.md
│   ├── 01-dfs-bfs/
│   │   ├── lc-200-number-of-islands.md
│   │   ├── lc-797-all-paths-from-source-to-target.md
│   │   └── lc-994-rotting-oranges.md
│   ├── 02-topological/
│   │   └── lc-207-course-schedule.md
│   ├── 03-shortest-path/
│   │   └── lc-743-network-delay-time.md
│   └── 04-union-find/
│       ├── lc-200-number-of-islands.md
│       └── lc-547-number-of-provinces.md
│
├── 09-dp/                       # 动态规划专题
│   ├── README.md
│   ├── templates.md
│   ├── 01-climbing/
│   │   ├── lc-70-climbing-stairs.md
│   │   └── lc-746-min-cost-climbing-stairs.md
│   ├── 02-knapsack/
│   │   ├── lc-416-partition-equal-subset-sum.md
│   │   └── lc-494-target-sum.md
│   ├── 03-subsequence/
│   │   ├── lc-300-longest-increasing-subsequence.md
│   │   ├── lc-1143-longest-common-subsequence.md
│   │   └── lc-72-edit-distance.md
│   ├── 04-interval/
│   │   └── lc-516-longest-palindromic-subsequence.md
│   └── 05-stock/
│       ├── lc-121-best-time-to-buy-and-sell-stock.md
│       └── lc-122-best-time-to-buy-and-sell-stock-ii.md
│
├── 10-backtracking/             # 回溯专题
│   ├── README.md
│   ├── templates.md
│   ├── 01-combination/
│   │   └── lc-77-combinations.md
│   ├── 02-permutation/
│   │   ├── lc-46-permutations.md
│   │   └── lc-47-permutations-ii.md
│   ├── 03-subset/
│   │   └── lc-78-subsets.md
│   └── 04-sudoku/
│       └── lc-51-n-queens.md
│
├── 11-greedy/                   # 贪心专题
│   ├── README.md
│   ├── templates.md
│   ├── 01-interval/
│   │   ├── lc-56-merge-intervals.md
│   │   └── lc-435-non-overlapping-intervals.md
│   └── 02-scheduling/
│       └── lc-455-assign-cookies.md
│
└── 12-interview/                # 面试冲刺
    ├── README.md
    ├── top-100/
    └── by-company/
```

---

## 文件模板

### 1. 每个专题的 README.md 模板

```markdown
# [专题名称] 专题

## 专题概述

本专题的核心知识点：
- 知识点1：说明
- 知识点2：说明

## 学习路径

1. 先阅读 [templates.md](./templates.md) 熟悉代码模板
2. 按顺序完成各分类题目：
   - [ ] 01-分类名（基础，必做）
   - [ ] 02-分类名（中等难度）

## 题目分类

| 分类 | 题目数 | 难度 | 说明 |
|------|-------|------|------|
| 01-xxx | 3 | ⭐⭐ | 基础必会 |
| 02-yyy | 4 | ⭐⭐⭐ | 进阶 |

## 进度追踪

- [ ] 01-xxx (0/3)
- [ ] 02-yyy (0/4)

## 参考资源

- [LeetCode 专题标签](https://leetcode.cn/tag/)
```

### 2. 每个专题的 templates.md 模板

```markdown
# [专题名称] 代码模板

## 数据结构定义

### Java
```java
// 数据结构定义
```

### Python
```python
# 数据结构定义
```

---

## 核心操作模板

### 操作一：xxx

#### Java
```java
// 模板代码
```

#### Python
```python
# 模板代码
```

---

## 典型题目模式

### 模式一：xxx 模式
- 适用场景：...
- 模板代码：...
```

### 3. 每道题目的文档模板

```markdown
# [题号] 题目英文名

[中文链接](https://leetcode.cn/problems/xxx/) | [English](https://leetcode.com/problems/xxx/)

**难度**：Easy / Medium / Hard
**标签**：数组 | 双指针
**相似题目**：[lc-xxx](...)

---

## 题目描述

...

---

## 解题思路

### 方法一：方法名

**思路**：详细的解题思路

**复杂度**：
- 时间：O(n)
- 空间：O(1)

**步骤**：
1. 第一步
2. 第二步

---

## 代码实现

### Java
```java
class Solution {
    public int methodName(int[] nums) {
        // 代码
    }
}
```

### Python
```python
class Solution:
    def methodName(self, nums: List[int]) -> int:
        # 代码
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| xxx | 说明 |
| xxx | 说明 |

---

## 变种/延伸

- 变种1：...
- 变种2：...
```

---

## 内容完成进度追踪

### 00-templates 通用模板
- [ ] java-basic.md - Java 基础模板
- [ ] python-basic.md - Python 基础模板
- [ ] language-comparison.md - 语法对照

### 01-array 数组专题
- [ ] README.md
- [ ] templates.md
- [ ] 01-two-pointers/
  - [ ] lc-167-two-sum-ii.md
  - [ ] lc-125-valid-palindrome.md
  - [ ] lc-11-container-with-most-water.md
- [ ] 02-sliding-window/
  - [ ] lc-209-min-size-subarray.md
  - [ ] lc-3-longest-substring-without-repeating.md
  - [ ] lc-438-find-all-anagrams.md
- [ ] 03-binary-search/
  - [ ] lc-704-binary-search.md
  - [ ] lc-35-search-insert-position.md
  - [ ] lc-33-search-in-rotated-array.md
- [ ] 04-others/
  - [ ] lc-1-two-sum.md
  - [ ] lc-15-3sum.md

### 02-string 字符串专题
- [ ] README.md
- [ ] templates.md
- [ ] 01-two-pointers/
  - [ ] lc-344-reverse-string.md
  - [ ] lc-5-longest-palindromic-substring.md
- [ ] 02-kmp/
  - [ ] lc-28-find-the-index-of-the-first-occurrence.md
  - [ ] lc-459-repeated-substring-pattern.md
- [ ] 03-subsequence/
  - [ ] lc-392-is-subsequence.md
  - [ ] lc-516-longest-palindromic-subsequence.md

### 03-linked-list 链表专题
- [ ] README.md
- [ ] templates.md
- [ ] 01-basic/
  - [ ] lc-206-reverse-linked-list.md
  - [ ] lc-21-merge-two-sorted-lists.md
  - [ ] lc-83-remove-duplicates.md
- [ ] 02-two-pointers/
  - [ ] lc-19-remove-nth-node-from-end.md
  - [ ] lc-142-linked-list-cycle-ii.md
- [ ] 03-cycle/
  - [ ] lc-141-linked-list-cycle.md

### 04-hash 哈希表专题
- [ ] README.md
- [ ] templates.md
- [ ] 01-frequency/
  - [ ] lc-1-two-sum.md
  - [ ] lc-347-top-k-frequent-elements.md
- [ ] 02-subarray/
  - [ ] lc-560-subarray-sum-equals-k.md
  - [ ] lc-128-longest-consecutive-sequence.md
- [ ] 03-grouping/
  - [ ] lc-49-group-anagrams.md

### 05-stack-queue 栈队列专题
- [ ] README.md
- [ ] templates.md
- [ ] 01-mono-stack/
  - [ ] lc-739-daily-temperatures.md
  - [ ] lc-496-next-greater-element-i.md
  - [ ] lc-84-largest-rectangle-in-histogram.md
- [ ] 02-queue/
  - [ ] lc-239-sliding-window-maximum.md
- [ ] 03-parentheses/
  - [ ] lc-20-valid-parentheses.md
  - [ ] lc-22-generate-parentheses.md

### 06-tree 二叉树专题
- [ ] README.md
- [ ] templates.md
- [ ] 01-traversal/
  - [ ] lc-144-binary-tree-preorder-traversal.md
  - [ ] lc-94-binary-tree-inorder-traversal.md
  - [ ] lc-145-binary-tree-postorder-traversal.md
  - [ ] lc-102-binary-tree-level-order-traversal.md
- [ ] 02-properties/
  - [ ] lc-104-maximum-depth-of-binary-tree.md
  - [ ] lc-110-balanced-binary-tree.md
  - [ ] lc-236-lowest-common-ancestor-of-a-binary-tree.md
- [ ] 03-bst/
  - [ ] lc-98-validate-binary-search-tree.md
  - [ ] lc-230-kth-smallest-element-in-a-bst.md
- [ ] 04-lca/
  - [ ] lc-236-lowest-common-ancestor-of-a-binary-tree.md

### 07-heap 堆专题
- [ ] README.md
- [ ] templates.md
- [ ] 01-top-k/
  - [ ] lc-215-kth-largest-element-in-an-array.md
  - [ ] lc-347-top-k-frequent-elements.md
- [ ] 02-merge/
  - [ ] lc-23-merge-k-sorted-lists.md

### 08-graph 图专题
- [ ] README.md
- [ ] templates.md
- [ ] 01-dfs-bfs/
  - [ ] lc-200-number-of-islands.md
  - [ ] lc-797-all-paths-from-source-to-target.md
  - [ ] lc-994-rotting-oranges.md
- [ ] 02-topological/
  - [ ] lc-207-course-schedule.md
- [ ] 03-shortest-path/
  - [ ] lc-743-network-delay-time.md
- [ ] 04-union-find/
  - [ ] lc-200-number-of-islands.md
  - [ ] lc-547-number-of-provinces.md

### 09-dp 动态规划专题
- [ ] README.md
- [ ] templates.md
- [ ] 01-climbing/
  - [ ] lc-70-climbing-stairs.md
  - [ ] lc-746-min-cost-climbing-stairs.md
- [ ] 02-knapsack/
  - [ ] lc-416-partition-equal-subset-sum.md
  - [ ] lc-494-target-sum.md
- [ ] 03-subsequence/
  - [ ] lc-300-longest-increasing-subsequence.md
  - [ ] lc-1143-longest-common-subsequence.md
  - [ ] lc-72-edit-distance.md
- [ ] 04-interval/
  - [ ] lc-516-longest-palindromic-subsequence.md
- [ ] 05-stock/
  - [ ] lc-121-best-time-to-buy-and-sell-stock.md
  - [ ] lc-122-best-time-to-buy-and-sell-stock-ii.md

### 10-backtracking 回溯专题
- [ ] README.md
- [ ] templates.md
- [ ] 01-combination/
  - [ ] lc-77-combinations.md
- [ ] 02-permutation/
  - [ ] lc-46-permutations.md
  - [ ] lc-47-permutations-ii.md
- [ ] 03-subset/
  - [ ] lc-78-subsets.md
- [ ] 04-sudoku/
  - [ ] lc-51-n-queens.md

### 11-greedy 贪心专题
- [ ] README.md
- [ ] templates.md
- [ ] 01-interval/
  - [ ] lc-56-merge-intervals.md
  - [ ] lc-435-non-overlapping-intervals.md
- [ ] 02-scheduling/
  - [ ] lc-455-assign-cookies.md

### 12-interview 面试冲刺
- [ ] README.md
- [ ] top-100/
- [ ] by-company/

---

## 总体进度

| 专题 | 进度 |
|------|------|
| 00-templates | 0% |
| 01-array | 0% |
| 02-string | 0% |
| 03-linked-list | 0% |
| 04-hash | 0% |
| 05-stack-queue | 0% |
| 06-tree | 0% |
| 07-heap | 0% |
| 08-graph | 0% |
| 09-dp | 0% |
| 10-backtracking | 0% |
| 11-greedy | 0% |
| 12-interview | 0% |

---

## 执行说明

1. 按顺序完成各专题内容
2. 每完成一个文件，在上述进度表中打勾
3. 每道题文档必须包含：题目描述、解题思路、Java代码、Python代码、关键点
4. 代码模板必须清晰、可直接复制使用
