# 435. Non-overlapping Intervals / 无重叠区间

[中文链接](https://leetcode.cn/problems/non-overlapping-intervals/) | [English](https://leetcode.com/problems/non-overlapping-intervals/)

**难度**：Medium
**标签**：数组 | 贪心 | 区间
**相似题目**：[lc-56](./lc-56-merge-intervals.md) | [lc-452](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给定一个区间的集合 `intervals`，其中 intervals[i] = [starti, endi] 。返回 需要移除区间的最小数量，使剩余区间互不重叠 。

**注意**：区间 [1,2] 和 [2,3] 的边界"相互接触"，但没有重叠。

**示例 1**：
```
输入：intervals = [[1,2],[2,3],[3,4],[1,3]]
输出：1
解释：[1,3] 被移除，剩余区间互不重叠。
```

**示例 2**：
```
输入：intervals = [[1,2],[1,2],[1,2]]
输出：2
解释：需要移除两个 [1,2]，使剩余区间互不重叠。
```

**示例 3**：
```
输入：intervals = [[1,2],[2,3]]
输出：0
解释：区间已经不重叠。
```

**约束条件**：
- 1 <= intervals.length <= 10⁵
- intervals[i].length == 2
- -5 * 10⁴ <= starti < endi <= 5 * 10⁴

---

## 解题思路

### 方法一：贪心（推荐）

**思路**：
按区间终点排序，贪心选择最早结束的区间。

**核心思想**：
- 选择结束早的区间，为后面留出更多空间
- 用最少的区间覆盖最多的范围

**复杂度**：
- 时间：O(n log n)
- 空间：O(1)

**步骤**：
1. 按区间终点排序
2. 贪心选择不重叠的区间
3. 返回需要移除的数量 = 总数 - 选择的数量

---

## 代码实现

### Java

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        if (intervals.length == 0) return 0;

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
            // 有重叠，跳过（相当于删除）
        }

        return intervals.length - count;
    }
}
```

### Python

```python
from typing import List

class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        if not intervals:
            return 0

        # 按终点排序
        intervals.sort(key=lambda x: x[1])

        count = 1
        end = intervals[0][1]

        for i in range(1, len(intervals)):
            if intervals[i][0] >= end:
                # 无重叠，选择这个区间
                count += 1
                end = intervals[i][1]

        return len(intervals) - count
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 按终点排序 | 贪心策略：选择最早结束 |
| 重叠判断 | 当前起点 >= 上一个终点 |
| 贪心选择 | 只要能选就选，不跳跃 |
| 返回移除数 | 总数 - 选择的数量 |
| [1,2] 和 [2,3] | 不算重叠，边界相接 |

---

## 图解

```
intervals = [[1,2],[2,3],[3,4],[1,3]]

排序后（按终点）:
[[1,2],[2,3],[3,4],[1,3]]
排序前需调整: [[1,2],[1,3],[2,3],[3,4]]

贪心选择过程:
end = 2, count = 1

[1,3]: start=1 < end=2, 重叠, 跳过

[2,3]: start=2 >= end=2, 不重叠
      count = 2, end = 3

[3,4]: start=3 >= end=3, 不重叠
      count = 3, end = 4

选择的区间: [1,2], [2,3], [3,4]
移除的区间: [1,3]
返回: 4 - 3 = 1

---

区间示意:
[1,2]   [2,3]   [3,4]
 └─────┘ └───────┘ └───────┘
        [1,3] ← 重叠，需要移除

移除 [1,3] 后:
[1,2] [2,3] [3,4]
 └───────┘ └───────┘ └───────┘
```

---

## 变种/延伸

### 变种1：合并区间
- **题目**：[lc-56 合并区间](./lc-56-merge-intervals.md)
- **变化**：合并重叠区间
- **解法**：排序 + 合并

### 变种2：用最少数量的箭头刺破气球
- **题目**：[lc-452 用最少数量的箭头刺破气球](./)
- **变化**：区间选中的箭头最少
- **解法**：类似本题，按右边界排序

### 变种3：无重叠区间的子集数量
- **题目**：计算最多能选多少个不重叠区间
- **解法**：本题就是计算这个数量

### 进阶思考

1. **为什么按终点排序是正确的？**
   - 贪心策略：早结束的区间占用空间更小
   - 数学证明：交换法可以证明
   - 类似活动选择问题

2. **如果要求返回具体移除的区间？**
   - 记录被跳过的区间索引
   - 或者重建结果列表

3. **如何处理超大输入？**
   - 排序需要 O(n log n)
   - 贪心选择 O(n)
   - 可以优化排序部分

4. **如果区间已经是排序的？**
   - 直接处理，跳过排序步骤
   - 时间可以降到 O(n)

5. **如何处理负坐标？**
   - 排序逻辑不变
   - 只是比较起点和终点时注意负数比较
   - 或者统一加偏移量

6. **与活动选择问题的关系？**
   - 本题本质就是活动选择问题
   - 区间 = 活动的起止时间
   - 目标 = 最大化可选活动数量
   - 贪心算法相同
