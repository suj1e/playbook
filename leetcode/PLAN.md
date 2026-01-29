# 力扣刷题手册 - 内容规划

> 本文档详细规划了整个力扣刷题手册的内容结构，用于追踪完成进度。
> 最后更新：2025-01-29

## 📋 项目概述

**目标**：构建一套系统化的力扣刷题手册，涵盖所有核心算法与数据结构专题，提供 Java 和 Python 双语言实现。

**特色**：
- 按专题分类，循序渐进
- 每个专题包含通用模板、核心知识点、典型题目
- Java 和 Python 双语实现
- 详细的解题思路和复杂度分析
- 关键点总结和变种延伸

**目标读者**：
- 准备算法面试的工程师
- 需要系统复习数据结构与算法的开发者
- 希望掌握多种编程语言实现的程序员

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

## 📝 文件模板规范

### 撰写原则

1. **双语对照**：Java 和 Python 代码并列展示
2. **思路清晰**：先讲思路，再给代码
3. **完整详尽**：包含题目、思路、代码、复杂度、关键点、变种
4. **实用导向**：模板代码可直接复制使用
5. **渐进式**：从简单到复杂，循序渐进

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

**必须包含以下内容**：

1. **数据结构定义**
   - Java/Python 完整定义
   - 包含构造函数、常用方法
   - 添加注释说明各字段用途

2. **核心操作模板**
   - 每个操作的完整实现
   - 双语对照
   - 时间/空间复杂度标注

3. **典型题目模式**
   - 模式名称和适用场景
   - 模板代码（可直接套用）
   - 相关题目列表

4. **常见陷阱**
   - 列出该专题常见的易错点
   - 给出正确的处理方式

```markdown
# [专题名称] 代码模板

## 数据结构定义

### Java
```java
// 数据结构定义
public class ListNode {
    int val;
    ListNode next;
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}
```

### Python
```python
# 数据结构定义
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

---

## 核心操作模板

### 操作一：xxx

**功能**：简要说明

#### Java
```java
// 模板代码
public ListNode reverseList(ListNode head) {
    // 实现
}
```

**复杂度**：O(n) 时间, O(1) 空间

#### Python
```python
# 模板代码
def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
    # 实现
```

**复杂度**：O(n) 时间, O(1) 空间

---

## 典型题目模式

### 模式一：xxx 模式
- **适用场景**：描述什么情况下使用此模式
- **识别特征**：题目中如何判断应该用这个模式
- **模板代码**：
  ```java
  // 模板
  ```
- **相关题目**：[lc-xxx](../xxx), [lc-yyy](../yyy)

---

## 常见陷阱

| 陷阱 | 说明 | 解决方案 |
|------|------|----------|
| 边界条件 | 忘记处理空指针 | 在循环开始前检查 |
| ... | ... | ... |
```

### 3. 每道题目的文档模板

**必须包含以下内容**：

1. **元信息**
   - 题号、中英文名称
   - 双语链接
   - 难度、标签、相似题目
   - 频次统计（如果是高频题标注⭐）

2. **题目描述**
   - 完整的题目描述（中文）
   - 示例（至少2个）

3. **解题思路**
   - 可能包含多种方法
   - 每种方法：思路、复杂度、步骤

4. **代码实现**
   - Java 和 Python 完整代码
   - 代码注释清晰
   - 关键行标注

5. **关键点**
   - 表格形式
   - 列出易错点和技巧

6. **变种/延伸**
   - 相关题目变种
   - 进阶思考

```markdown
# [题号] 题目英文名 / 题目中文名

[中文链接](https://leetcode.cn/problems/xxx/) | [English](https://leetcode.com/problems/xxx/)

**难度**：Easy / Medium / Hard
**标签**：数组 | 双指针
**相似题目**：[lc-xxx](...) | [lc-yyy](...)
**面试频次**：⭐⭐⭐⭐⭐ (腾讯/字节/阿里高频)

---

## 题目描述

完整题目描述...

**示例 1**：
```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2**：
```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**约束条件**：
- 2 <= nums.length <= 10⁴
- -10⁹ <= nums[i] <= 10⁹

---

## 解题思路

### 方法一：暴力枚举

**思路**：
双重循环枚举所有可能的两数组合，找到和为 target 的组合。

**复杂度**：
- 时间：O(n²)
- 空间：O(1)

**步骤**：
1. 外层循环 i 从 0 到 n-2
2. 内层循环 j 从 i+1 到 n-1
3. 如果 nums[i] + nums[j] == target，返回 [i, j]

---

### 方法二：哈希表

**思路**：
用哈希表存储每个数的索引，遍历时检查 target - nums[i] 是否在表中。

**复杂度**：
- 时间：O(n)
- 空间：O(n)

**步骤**：
1. 创建哈希表 map
2. 遍历数组，对于每个 nums[i]：
   - 检查 target - nums[i] 是否在 map 中
   - 如果存在，返回 [map[target-nums[i]], i]
   - 否则，将 nums[i]: i 存入 map
3. 无解返回空数组

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：暴力枚举
    public int[] twoSum(int[] nums, int target) {
        int n = nums.length;
        for (int i = 0; i < n - 1; i++) {           // 外层循环
            for (int j = i + 1; j < n; j++) {        // 内层循环，从 i+1 开始
                if (nums[i] + nums[j] == target) {   // 找到目标
                    return new int[]{i, j};
                }
            }
        }
        return new int[]{};  // 无解
    }

    // 方法二：哈希表
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();  // 数值 -> 索引
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];         // 目标补数
            if (map.containsKey(complement)) {        // 检查补数是否存在
                return new int[]{map.get(complement), i};
            }
            map.put(nums[i], i);                      // 存入当前数
        }
        return new int[]{};
    }
}
```

### Python

```python
from typing import List

class Solution:
    # 方法一：暴力枚举
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        n = len(nums)
        for i in range(n - 1):                # 外层循环
            for j in range(i + 1, n):         # 内层循环
                if nums[i] + nums[j] == target:
                    return [i, j]
        return []  # 无解

    # 方法二：哈希表
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        num_map = {}  # 数值 -> 索引
        for i, num in enumerate(nums):
            complement = target - num          # 目标补数
            if complement in num_map:          # 检查补数是否存在
                return [num_map[complement], i]
            num_map[num] = i                  # 存入当前数
        return []
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 互补数关系 | 两数之和为 target，则一数是另一数的补数 |
| 先查后存 | 哈希表法必须先检查补数，再存入当前数（避免自己匹配自己） |
| 边界条件 | 题目保证必有解，但代码仍应处理无解情况 |
| 哈希表键值 | 键存数值，值存索引 |

---

## 变种/延伸

### 变种1：数组已排序
- 题目：[lc-167 两数之和 II - 输入有序数组](../01-array/01-two-pointers/lc-167-two-sum-ii.md)
- 变化：数组升序排列
- 解法：双指针（头尾向中间）

### 变种2：返回所有组合
- 题目：[lc-15 三数之和](../01-array/04-others/lc-15-3sum.md)
- 变化：找出所有不重复的三元组，和为0
- 解法：排序 + 双指针 + 去重

### 进阶思考
1. 如果数组中有重复元素怎么办？
2. 如果要求返回所有可能的组合（不止一组）怎么办？
3. 如果数组非常大（10⁹ 级别），无法全部加载到内存怎么办？
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
