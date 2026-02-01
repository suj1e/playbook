# 128. Longest Consecutive Sequence / 最长连续序列

[中文链接](https://leetcode.cn/problems/longest-consecutive-sequence/) | [English](https://leetcode.com/problems/longest-consecutive-sequence/)

**难度**：Medium
**标签**：数组 | 哈希表 | 并查集
**相似题目**：[lc-298](./) | [lc-547](../../08-graph/04-union-find/lc-547-number-of-provinces.md)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现时间复杂度为 O(n) 的算法解决此问题。

**示例 1**：
```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

**示例 2**：
```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

**约束条件**：
- 0 <= nums.length <= 10⁵
- -10⁹ <= nums[i] <= 10⁹

---

## 解题思路

### 方法一：排序 + 遍历

**思路**：
排序后遍历，找最长的连续序列。

**复杂度**：
- 时间：O(n log n)
- 空间：O(1) 或 O(n)（取决于排序）

---

### 方法二：哈希表（推荐）

**思路**：
用 HashSet 存储所有数字，然后枚举每个"序列起点"。

**关键优化**：
只从"序列起点"开始枚举（即 num-1 不在集合中的数字），避免重复计算。

**复杂度**：
- 时间：O(n)
- 空间：O(n)

**步骤**：
1. 将所有数字存入 HashSet
2. 遍历每个数字，只当它是序列起点时才展开
3. 从起点开始不断 +1，计算序列长度
4. 更新最大长度

---

### 方法三：并查集

**思路**：
将相邻数字合并到同一个集合，找最大的集合大小。

**复杂度**：
- 时间：O(n α(n)) ≈ O(n)
- 空间：O(n)

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：排序 O(n log n)
    public int longestConsecutive(int[] nums) {
        if (nums.length == 0) return 0;

        Arrays.sort(nums);

        int maxLen = 1;
        int curLen = 1;

        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == nums[i - 1]) {
                continue;  // 跳过重复
            } else if (nums[i] == nums[i - 1] + 1) {
                curLen++;
            } else {
                maxLen = Math.max(maxLen, curLen);
                curLen = 1;
            }
        }

        return Math.max(maxLen, curLen);
    }

    // 方法二：哈希表 O(n)
    public int longestConsecutive(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int num : nums) {
            set.add(num);
        }

        int maxLen = 0;

        for (int num : set) {
            // 只从序列起点开始
            if (!set.contains(num - 1)) {
                int curNum = num;
                int curLen = 1;

                // 向后扩展
                while (set.contains(curNum + 1)) {
                    curNum++;
                    curLen++;
                }

                maxLen = Math.max(maxLen, curLen);
            }
        }

        return maxLen;
    }
}
```

### Python

```python
class Solution:
    # 方法一：排序 O(n log n)
    def longestConsecutive(self, nums: List[int]) -> int:
        if not nums:
            return 0

        nums.sort()

        max_len = 1
        cur_len = 1

        for i in range(1, len(nums)):
            if nums[i] == nums[i - 1]:
                continue  # 跳过重复
            elif nums[i] == nums[i - 1] + 1:
                cur_len += 1
            else:
                max_len = max(max_len, cur_len)
                cur_len = 1

        return max(max_len, cur_len)

    # 方法二：哈希表 O(n)
    def longestConsecutive(self, nums: List[int]) -> int:
        num_set = set(nums)

        max_len = 0

        for num in num_set:
            # 只从序列起点开始
            if num - 1 not in num_set:
                cur_num = num
                cur_len = 1

                # 向后扩展
                while cur_num + 1 in num_set:
                    cur_num += 1
                    cur_len += 1

                max_len = max(max_len, cur_len)

        return max_len
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 序列起点判断 | num - 1 不在集合中，说明 num 是序列起点 |
| 避免重复计算 | 只从起点枚举，每个元素最多被访问一次 |
| HashSet 查找 | O(1) 时间判断数字是否存在 |
| 跳过重复元素 | 排序法需要特殊处理重复元素 |
| 最坏情况 | 所有元素连续，每个元素被访问两次（起点判断 + 扩展） |

---

## 图解

```
数组: [100, 4, 200, 1, 3, 2]

HashSet: {100, 4, 200, 1, 3, 2}

遍历过程:
1. num = 100: 99 不在 set 中，100 是起点
   扩展: 101 不在，序列 [100]，长度 1

2. num = 4: 3 在 set 中，4 不是起点，跳过

3. num = 200: 199 不在 set 中，200 是起点
   扩展: 201 不在，序列 [200]，长度 1

4. num = 1: 0 不在 set 中，1 是起点
   扩展: 2 在，3 在，4 在，5 不在
   序列 [1,2,3,4]，长度 4

5. num = 3: 2 在 set 中，3 不是起点，跳过

6. num = 2: 1 在 set 中，2 不是起点，跳过

最长序列: [1,2,3,4]，长度 4
```

---

## 变种/延伸

### 变种1：最长的连续递增序列（要求在原数组中连续）
- **题目**：[lc-674 最长连续递增序列](./)
- **变化**：要求子数组（位置连续）
- **解法**：直接遍历，O(n) 时间

### 变种2：二叉树中最长连续序列
- **题目**：[lc-298 二叉树最长连续序列](./)
- **变化**：在二叉树中找连续序列（父子节点值相差1）
- **解法**：DFS 遍历

### 变种3：最长的等差数列
- **题目**：[lc-1027 最长等差数列](./)
- **变化**：相邻元素差值相等
- **解法**：动态规划

### 变种4：最长连续序列的路径
- **变化**：输出最长序列的具体元素
- **解法**：在哈希表法中记录序列

### 进阶思考

1. **为什么哈希表方法是 O(n)？**
   - 每个元素最多被访问两次：
     1. 作为起点判断
     2. 在序列扩展时被访问
   - 外层循环 O(n)，内层 while 总共执行 O(n) 次

2. **如果要求返回最长序列本身，怎么做？**
   - 在哈希表法中，找到最长序列时记录起点和长度
   - 或者返回 [start, start + length - 1] 的所有数

3. **可以用并查集解决吗？**
   - 可以，将相邻数字合并
   - 最后找最大的集合大小
   - 但实现比哈希表复杂

4. **如果数字范围很小（如 0-100），如何优化？**
   - 可以用布尔数组代替 HashSet
   - 空间复杂度仍是 O(n)，但常数更小

5. **如何处理海量数据（无法全部放入内存）？**
   - 可以使用外部排序 + 归并
   - 或者使用布隆过滤器 + 分块处理
