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

## 🔍 详细任务分解

### 00-templates 通用模板

**目标**：提供所有专题共用的基础代码模板

| 文件 | 必需内容 | 状态 |
|------|----------|------|
| java-basic.md | 输入输出（Scanner/BufferedReader）、数组操作、List/Set/Map常用API、字符串处理、排序、lambda表达式 | |
| python-basic.md | 输入输出（input/sys.stdin）、列表推导、字典/集合操作、字符串处理、sorted/sort、装饰器 | |
| language-comparison.md | 数据类型、循环、函数、类、异常处理、集合操作的对照表 | |

---

### 01-array 数组专题

**核心知识点**：
- 数组的内存布局（连续内存）
- 双指针技巧（对撞指针、快慢指针）
- 滑动窗口（固定/可变大小）
- 二分查找（标准/变形）
- 前缀和思想

**templates.md 必需内容**：
1. 数组初始化方式
2. 双指针模板（对撞、快慢）
3. 滑动窗口模板（固定大小、可变大小）
4. 二分查找模板（标准、左边界、右边界）
5. 常见陷阱（索引越界、空数组、重复元素）

**README.md 必需内容**：
- 专题概述：为什么数组重要、时间复杂度特点
- 学习路径：从双指针 → 滑动窗口 → 二分查找
- 题目分类表（含难度、频次）
- 参考资源链接

**题目分类详解**：

#### 01-two-pointers 双指针
- lc-167: 两数之和II（有序数组）→ 对撞指针
- lc-125: 验证回文串 → 左右指针
- lc-11: 盛水最多的容器 → 对撞指针 + 贪心

#### 02-sliding-window 滑动窗口
- lc-209: 长度最小的子数组 → 可变窗口 + 求和
- lc-3: 无重复字符的最长子串 → 可变窗口 + 哈希
- lc-438: 找字母异位词 → 固定窗口 + 计数

#### 03-binary-search 二分查找
- lc-704: 标准二分查找 → 基础模板
- lc-35: 搜索插入位置 → 左边界模板
- lc-33: 旋转数组搜索 → 变形二分

#### 04-others 其他
- lc-1: 两数之和 → 哈希表
- lc-15: 三数之和 → 排序 + 双指针 + 去重

---

### 02-string 字符串专题

**核心知识点**：
- 字符串的不可变性（Java/Python都适用）
- 双指针处理字符串
- KMP算法（模式匹配）
- 子序列问题（DP基础）

**templates.md 必需内容**：
1. 字符串初始化和基本操作
2. 双指针处理字符串（反转、去重）
3. KMP算法完整实现（计算next数组 + 匹配）
4. 子序列判断模板
5. 字符串与数组转换

**题目分类详解**：

#### 01-two-pointers 双指针
- lc-344: 反转字符串 → 基础对撞指针
- lc-5: 最长回文子串 → 中心扩散法/动态规划

#### 02-kmp KMP算法
- lc-28: strStr() → 标准KMP
- lc-459: 重复子串 → KMP变形

#### 03-subsequence 子序列
- lc-392: 判断子序列 → 双指针贪心
- lc-516: 最长回文子序列 → 区间DP

---

### 03-linked-list 链表专题

**核心知识点**：
- 链表的基本结构和操作
- 虚拟头节点技巧
- 双指针（快慢指针、前后指针）
- 环的检测与处理

**templates.md 必需内容**：
1. 链表节点定义（Java/Python）
2. 遍历、反转、合并链表
3. 快慢指针找中点/倒数第k个
4. 环检测和入环点计算
5. 虚拟头节点使用场景

**题目分类详解**：

#### 01-basic 基础操作
- lc-206: 反转链表 → 迭代/递归
- lc-21: 合并两个有序链表 → 双指针
- lc-83: 删除排序链表重复元素 → 单指针

#### 02-two-pointers 双指针技巧
- lc-19: 删除倒数第N个节点 → 快慢指针 + 虚拟头
- lc-142: 环形链表II → 快慢指针 + 数学推导

#### 03-cycle 环问题
- lc-141: 环形链表 → 快慢指针检测

---

### 04-hash 哈希表专题

**核心知识点**：
- 哈希表的时间复杂度
- 常见哈希表结构（HashMap/HashSet）
- 前缀和 + 哈希表
- 哈希表优化查找

**templates.md 必需内容**：
1. HashMap/HashSet 初始化和基本操作
2. 计数问题的通用模式
3. 前缀和 + 哈希表模板
4. 分组问题的哈希策略
5. 哈希表常见陷阱（键类型、null处理）

**题目分类详解**：

#### 01-frequency 计数问题
- lc-1: 两数之和 → 哈希表查找
- lc-347: 前K个高频元素 → 哈希表 + 堆

#### 02-subarray 子数组问题
- lc-560: 和为K的子数组 → 前缀和 + 哈希表
- lc-128: 最长连续序列 → 哈希表去重 + 智力题

#### 03-grouping 分组问题
- lc-49: 字母异位词分组 → 排序/计数作为key

---

### 05-stack-queue 栈队列专题

**核心知识点**：
- 栈的LIFO特性（括号匹配、单调栈）
- 队列的FIFO特性（BFS）
- 单调栈的应用
- 双端队列的滑动窗口

**templates.md 必需内容**：
1. Stack/Queue 初始化和基本操作
2. 单调递增/递减栈模板
3. 双端队列实现单调队列
4. 括号匹配模板
5. 表达式求值模板

**题目分类详解**：

#### 01-mono-stack 单调栈
- lc-739: 每日温度 → 单调递增栈
- lc-496: 下一个更大元素I → 单调栈
- lc-84: 柱状图中最大矩形 → 单调栈 + 面积计算

#### 02-queue 队列应用
- lc-239: 滑动窗口最大值 → 单调队列

#### 03-parentheses 括号问题
- lc-20: 有效的括号 → 栈匹配
- lc-22: 生成括号 → 回溯/DFS

---

### 06-tree 二叉树专题

**核心知识点**：
- 二叉树的遍历方式（前中后序、层序）
- 递归与迭代的转换
- 树的属性计算（深度、高度、直径）
- BST特性验证
- LCA（最近公共祖先）

**templates.md 必需内容**：
1. 树节点定义（TreeNode）
2. 递归遍历模板（前中后序）
3. 迭代遍历模板（栈实现）
4. 层序遍历模板（队列）
5. 树的深度/直径计算
6. BST验证模板
7. LCA递归模板

**题目分类详解**：

#### 01-traversal 遍历
- lc-144/94/145: 前/中/后序遍历 → 递归/迭代
- lc-102: 层序遍历 → 队列BFS

#### 02-properties 树的属性
- lc-104: 二叉树最大深度 → DFS/BFS
- lc-110: 平衡二叉树 → 自底向上递归
- lc-236: 最近公共祖先 → 递归/迭代

#### 03-bst 二叉搜索树
- lc-98: 验证BST → 中序遍历/递归范围
- lc-230: 二叉搜索树第k小 → 中序遍历

---

### 07-heap 堆专题

**核心知识点**：
- 堆的特性（完全二叉树）
- 优先队列的使用
- TopK问题
- 堆排序思想

**templates.md 必需内容**：
1. 优先队列初始化（Java: PriorityQueue, Python: heapq）
2. 大根堆/小根堆创建
3. 堆的插入和删除
4. TopK问题模板
5. 堆排序模板

**题目分类详解**：

#### 01-top-k TopK问题
- lc-215: 第K大元素 → 小根堆
- lc-347: 前K个高频元素 → 堆 + 哈希表

#### 02-merge 合并问题
- lc-23: 合并K个有序链表 → 堆

---

### 08-graph 图专题

**核心知识点**：
- 图的表示（邻接矩阵/邻接表）
- DFS/BFS遍历
- 拓扑排序
- 最短路径（Dijkstra、Floyd）
- 并查集

**templates.md 必需内容**：
1. 图的邻接表表示
2. DFS递归/迭代模板
3. BFS队列模板
4. 拓扑排序模板（Kahn算法）
5. Dijkstra算法模板
6. 并查集模板（路径压缩+按秩合并）

**题目分类详解**：

#### 01-dfs-bfs 遍历
- lc-200: 岛屿数量 → DFS/BFS + 网格图
- lc-797: 所有可能路径 → DFS回溯
- lc-994: 腐烂橘子 → BFS多源

#### 02-topological 拓扑排序
- lc-207: 课程表 → 拓扑排序检测环

#### 03-shortest-path 最短路径
- lc-743: 网络延迟时间 → Dijkstra

#### 04-union-find 并查集
- lc-200: 岛屿数量 → 并查集
- lc-547: 省份数量 → 并查集

---

### 09-dp 动态规划专题

**核心知识点**：
- DP问题的识别特征（重叠子问题、最优子结构）
- DP解题步骤（定义状态、状态转移、初始化、计算顺序）
- 常见DP模式（线性、区间、背包、状态机）
- 空间优化技巧

**templates.md 必需内容**：
1. 一维DP模板
2. 二维DP模板
3. 01背包模板
4. 完全背包模板
5. LIS模板
6. LCS模板
7. DP空间优化技巧

**题目分类详解**：

#### 01-climbing 爬楼梯类
- lc-70: 爬楼梯 → 基础DP
- lc-746: 使用最小花费爬楼梯 → DP带代价

#### 02-knapsack 背包问题
- lc-416: 分割等和子集 → 01背包
- lc-494: 目标和 → 01背包变形

#### 03-subsequence 子序列
- lc-300: 最长递增子序列 → 二分优化DP
- lc-1143: 最长公共子序列 → 二维DP
- lc-72: 编辑距离 → 经典DP

#### 04-interval 区间DP
- lc-516: 最长回文子序列 → 区间DP

#### 05-stock 股票问题
- lc-121: 买卖股票的最佳时机 → 状态机DP
- lc-122: 买卖股票的最佳时机II → 贪心/DP

---

### 10-backtracking 回溯专题

**核心知识点**：
- 回溯的适用场景（组合、排列、子集、搜索）
- 回溯模板（选择、递归、撤销）
- 剪枝优化
- 回溯 vs DFS

**templates.md 必需内容**：
1. 回溯算法通用模板
2. 组合问题模板
3. 排列问题模板
4. 子集问题模板
5. 剪枝技巧
6. 去重策略（used数组）

**题目分类详解**：

#### 01-combination 组合
- lc-77: 组合 → 基础回溯

#### 02-permutation 排列
- lc-46: 全排列 → 回溯
- lc-47: 全排列II → 回溯 + 去重

#### 03-subset 子集
- lc-78: 子集 → 回溯/迭代

#### 04-sudoku 复杂回溯
- lc-51: N皇后 → 经典回溯

---

### 11-greedy 贪心专题

**核心知识点**：
- 贪心的适用场景（局部最优 → 全局最优）
- 区间问题贪心策略
- 贪心 vs DP 的选择

**templates.md 必需内容**：
1. 区间合并模板
2. 区间调度模板
3. 贪心证明思路
4. 常见贪心策略

**题目分类详解**：

#### 01-interval 区间问题
- lc-56: 合并区间 → 排序 + 合并
- lc-435: 无重叠区间 → 贪心

#### 02-scheduling 调度问题
- lc-455: 分发饼干 → 贪心

---

### 12-interview 面试冲刺

**目标**：按公司分类的高频题汇总

| 分类 | 内容 |
|------|------|
| top-100 | LeetCode 热题100 |
| by-company | 按公司分类：字节、腾讯、阿里、美团、华为等 |

---

## 📊 进度追踪表

### 总体进度概览

| 专题 | 文件数 | 已完成 | 进度 | 优先级 |
|------|-------|-------|------|-------|
| 00-templates | 3 | 0 | 0% | 🔴 最高 |
| 01-array | 11 | 0 | 0% | 🔴 最高 |
| 02-string | 8 | 0 | 0% | 🟡 高 |
| 03-linked-list | 8 | 0 | 0% | 🟡 高 |
| 04-hash | 6 | 0 | 0% | 🟡 高 |
| 05-stack-queue | 9 | 0 | 0% | 🟡 高 |
| 06-tree | 11 | 0 | 0% | 🟡 高 |
| 07-heap | 5 | 0 | 0% | 🟢 中 |
| 08-graph | 9 | 0 | 0% | 🟢 中 |
| 09-dp | 13 | 0 | 0% | 🟢 中 |
| 10-backtracking | 7 | 0 | 0% | 🟢 中 |
| 11-greedy | 5 | 0 | 0% | 🟢 中 |
| 12-interview | - | 0 | 0% | ⚪ 低 |

**说明**：
- 🔴 最高：基础模板，所有专题依赖
- 🟡 高：核心数据结构与算法
- 🟢 中：进阶算法
- ⚪ 低：冲刺阶段内容

---

## 📋 详细任务清单

### 00-templates 通用模板（0/3）

| 文件 | 状态 | 备注 |
|------|------|------|
| java-basic.md | ⬜ | Java基础API模板 |
| python-basic.md | ⬜ | Python基础API模板 |
| language-comparison.md | ⬜ | 双语语法对照 |

---

### 01-array 数组专题（0/11）

| 分类 | 文件 | 难度 | 频次 | 状态 |
|------|------|------|------|------|
| README | README.md | - | - | ⬜ |
| templates | templates.md | - | - | ⬜ |
| two-pointers | lc-167 | Medium | ⭐⭐⭐⭐ | ⬜ |
| two-pointers | lc-125 | Easy | ⭐⭐⭐ | ⬜ |
| two-pointers | lc-11 | Medium | ⭐⭐⭐⭐⭐ | ⬜ |
| sliding-window | lc-209 | Medium | ⭐⭐⭐⭐ | ⬜ |
| sliding-window | lc-3 | Medium | ⭐⭐⭐⭐⭐ | ⬜ |
| sliding-window | lc-438 | Medium | ⭐⭐⭐ | ⬜ |
| binary-search | lc-704 | Easy | ⭐⭐⭐⭐ | ⬜ |
| binary-search | lc-35 | Easy | ⭐⭐⭐⭐ | ⬜ |
| binary-search | lc-33 | Medium | ⭐⭐⭐⭐⭐ | ⬜ |

**补充题目**：
- lc-1 (two-sum) - 哈希表入门
- lc-15 (3sum) - 双指针+去重

---

### 02-string 字符串专题（0/8）

| 分类 | 文件 | 难度 | 频次 | 状态 |
|------|------|------|------|------|
| README | README.md | - | - | ⬜ |
| templates | templates.md | - | - | ⬜ |
| two-pointers | lc-344 | Easy | ⭐⭐⭐ | ⬜ |
| two-pointers | lc-5 | Medium | ⭐⭐⭐⭐⭐ | ⬜ |
| kmp | lc-28 | Medium | ⭐⭐⭐ | ⬜ |
| kmp | lc-459 | Easy | ⭐⭐ | ⬜ |
| subsequence | lc-392 | Easy | ⭐⭐⭐ | ⬜ |
| subsequence | lc-516 | Medium | ⭐⭐⭐⭐ | ⬜ |

---

### 03-linked-list 链表专题（0/8）

| 分类 | 文件 | 难度 | 频次 | 状态 |
|------|------|------|------|------|
| README | README.md | - | - | ⬜ |
| templates | templates.md | - | - | ⬜ |
| basic | lc-206 | Easy | ⭐⭐⭐⭐⭐ | ⬜ |
| basic | lc-21 | Easy | ⭐⭐⭐⭐⭐ | ⬜ |
| basic | lc-83 | Easy | ⭐⭐⭐ | ⬜ |
| two-pointers | lc-19 | Medium | ⭐⭐⭐⭐ | ⬜ |
| two-pointers | lc-142 | Medium | ⭐⭐⭐⭐ | ⬜ |
| cycle | lc-141 | Easy | ⭐⭐⭐⭐ | ⬜ |

---

### 04-hash 哈希表专题（0/6）

| 分类 | 文件 | 难度 | 频次 | 状态 |
|------|------|------|------|------|
| README | README.md | - | - | ⬜ |
| templates | templates.md | - | - | ⬜ |
| frequency | lc-1 | Easy | ⭐⭐⭐⭐⭐ | ⬜ |
| frequency | lc-347 | Medium | ⭐⭐⭐⭐ | ⬜ |
| subarray | lc-560 | Medium | ⭐⭐⭐⭐⭐ | ⬜ |
| subarray | lc-128 | Medium | ⭐⭐⭐⭐ | ⬜ |
| grouping | lc-49 | Medium | ⭐⭐⭐ | ⬜ |

---

### 05-stack-queue 栈队列（0/9）

| 分类 | 文件 | 难度 | 频次 | 状态 |
|------|------|------|------|------|
| README | README.md | - | - | ⬜ |
| templates | templates.md | - | - | ⬜ |
| mono-stack | lc-739 | Medium | ⭐⭐⭐⭐ | ⬜ |
| mono-stack | lc-496 | Easy | ⭐⭐⭐ | ⬜ |
| mono-stack | lc-84 | Hard | ⭐⭐⭐⭐⭐ | ⬜ |
| queue | lc-239 | Hard | ⭐⭐⭐⭐⭐ | ⬜ |
| parentheses | lc-20 | Easy | ⭐⭐⭐⭐⭐ | ⬜ |
| parentheses | lc-22 | Medium | ⭐⭐⭐⭐ | ⬜ |

---

### 06-tree 二叉树专题（0/11）

| 分类 | 文件 | 难度 | 频次 | 状态 |
|------|------|------|------|------|
| README | README.md | - | - | ⬜ |
| templates | templates.md | - | - | ⬜ |
| traversal | lc-144 | Easy | ⭐⭐⭐⭐ | ⬜ |
| traversal | lc-94 | Easy | ⭐⭐⭐⭐ | ⬜ |
| traversal | lc-145 | Easy | ⭐⭐⭐⭐ | ⬜ |
| traversal | lc-102 | Medium | ⭐⭐⭐⭐⭐ | ⬜ |
| properties | lc-104 | Easy | ⭐⭐⭐⭐⭐ | ⬜ |
| properties | lc-110 | Easy | ⭐⭐⭐⭐ | ⬜ |
| properties | lc-236 | Medium | ⭐⭐⭐⭐⭐ | ⬜ |
| bst | lc-98 | Medium | ⭐⭐⭐⭐ | ⬜ |
| bst | lc-230 | Medium | ⭐⭐⭐ | ⬜ |

---

### 07-heap 堆专题（0/5）

| 分类 | 文件 | 难度 | 频次 | 状态 |
|------|------|------|------|------|
| README | README.md | - | - | ⬜ |
| templates | templates.md | - | - | ⬜ |
| top-k | lc-215 | Medium | ⭐⭐⭐⭐⭐ | ⬜ |
| top-k | lc-347 | Medium | ⭐⭐⭐⭐ | ⬜ |
| merge | lc-23 | Hard | ⭐⭐⭐⭐⭐ | ⬜ |

---

### 08-graph 图专题（0/9）

| 分类 | 文件 | 难度 | 频次 | 状态 |
|------|------|------|------|------|
| README | README.md | - | - | ⬜ |
| templates | templates.md | - | - | ⬜ |
| dfs-bfs | lc-200 | Medium | ⭐⭐⭐⭐⭐ | ⬜ |
| dfs-bfs | lc-797 | Medium | ⭐⭐⭐ | ⬜ |
| dfs-bfs | lc-994 | Medium | ⭐⭐⭐⭐ | ⬜ |
| topological | lc-207 | Medium | ⭐⭐⭐⭐⭐ | ⬜ |
| shortest-path | lc-743 | Medium | ⭐⭐⭐⭐ | ⬜ |
| union-find | lc-200 | Medium | ⭐⭐⭐⭐⭐ | ⬜ |
| union-find | lc-547 | Medium | ⭐⭐⭐⭐ | ⬜ |

---

### 09-dp 动态规划（0/13）

| 分类 | 文件 | 难度 | 频次 | 状态 |
|------|------|------|------|------|
| README | README.md | - | - | ⬜ |
| templates | templates.md | - | - | ⬜ |
| climbing | lc-70 | Easy | ⭐⭐⭐⭐⭐ | ⬜ |
| climbing | lc-746 | Easy | ⭐⭐⭐ | ⬜ |
| knapsack | lc-416 | Medium | ⭐⭐⭐⭐ | ⬜ |
| knapsack | lc-494 | Medium | ⭐⭐⭐⭐ | ⬜ |
| subsequence | lc-300 | Medium | ⭐⭐⭐⭐⭐ | ⬜ |
| subsequence | lc-1143 | Medium | ⭐⭐⭐⭐ | ⬜ |
| subsequence | lc-72 | Medium | ⭐⭐⭐⭐⭐ | ⬜ |
| interval | lc-516 | Medium | ⭐⭐⭐⭐ | ⬜ |
| stock | lc-121 | Easy | ⭐⭐⭐⭐⭐ | ⬜ |
| stock | lc-122 | Medium | ⭐⭐⭐⭐⭐ | ⬜ |

---

### 10-backtracking 回溯（0/7）

| 分类 | 文件 | 难度 | 频次 | 状态 |
|------|------|------|------|------|
| README | README.md | - | - | ⬜ |
| templates | templates.md | - | - | ⬜ |
| combination | lc-77 | Medium | ⭐⭐⭐⭐ | ⬜ |
| permutation | lc-46 | Medium | ⭐⭐⭐⭐⭐ | ⬜ |
| permutation | lc-47 | Medium | ⭐⭐⭐⭐ | ⬜ |
| subset | lc-78 | Medium | ⭐⭐⭐⭐ | ⬜ |
| sudoku | lc-51 | Hard | ⭐⭐⭐⭐⭐ | ⬜ |

---

### 11-greedy 贪心（0/5）

| 分类 | 文件 | 难度 | 频次 | 状态 |
|------|------|------|------|------|
| README | README.md | - | - | ⬜ |
| templates | templates.md | - | - | ⬜ |
| interval | lc-56 | Medium | ⭐⭐⭐⭐⭐ | ⬜ |
| interval | lc-435 | Medium | ⭐⭐⭐⭐ | ⬜ |
| scheduling | lc-455 | Easy | ⭐⭐⭐ | ⬜ |

---

### 12-interview 面试冲刺

| 分类 | 内容 | 状态 |
|------|------|------|
| README | 面试指南总览 | ⬜ |
| top-100 | LeetCode热题100精选 | ⬜ |
| by-company | 按公司分类（字节/腾讯/阿里/美团/华为） | ⬜ |

---

## 📝 完成标准检查清单

### ✅ README.md 完成标准

- [ ] 专题概述（为什么重要）
- [ ] 核心知识点列表
- [ ] 学习路径建议
- [ ] 题目分类表格（含难度、频次）
- [ ] 参考资源链接

### ✅ templates.md 完成标准

- [ ] 数据结构定义（Java/Python）
- [ ] 核心操作模板（每个操作含复杂度分析）
- [ ] 典型题目模式（至少3个）
- [ ] 常见陷阱表格

### ✅ 题目文档完成标准

- [ ] 元信息（题号、名称、链接、难度、标签、频次）
- [ ] 完整题目描述（中文）
- [ ] 至少2个示例
- [ ] 解题思路（至少1种方法，含复杂度）
- [ ] Java完整代码（带注释）
- [ ] Python完整代码（带注释）
- [ ] 关键点表格
- [ ] 变种/延伸（至少2个相关题目）

---

## 🚀 执行计划

### 第一阶段：基础模板（优先）
1. 完成 00-templates（所有专题依赖此）
2. 完成 01-array（最基础）

### 第二阶段：核心数据结构
3. 完成 02-string
4. 完成 03-linked-list
5. 完成 04-hash

### 第三阶段：线性结构进阶
6. 完成 05-stack-queue
7. 完成 06-tree
8. 完成 07-heap

### 第四阶段：高级算法
9. 完成 08-graph
10. 完成 09-dp
11. 完成 10-backtracking
12. 完成 11-greedy

### 第五阶段：面试冲刺
13. 完成 12-interview

---

## 💡 使用说明

1. **状态标记**：
   - ⬜ = 未开始
   - 🟡 = 进行中
   - ✅ = 已完成

2. **频次说明**：
   - ⭐⭐⭐⭐⭐ = 必考题
   - ⭐⭐⭐⭐ = 高频题
   - ⭐⭐⭐ = 常见题
   - ⭐⭐ = 一般题

3. **更新进度**：
   - 完成一个文件后，将 ⬜ 改为 ✅
   - 更新总体进度表的百分比
