# 56. Merge Intervals / 合并区间

[中文链接](https://leetcode.cn/problems/merge-intervals/) | [English](https://leetcode.com/problems/merge-intervals/)

**难度**：Medium
**标签**：数组 | 排序 | 贪心
**相似题目**：[lc-57](./) | [lc-252](./) | [lc-253](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]` 。请你合并所有重叠的区间，并返回 **一个不重叠的区间数组**，该数组需恰好覆盖输入中所有区间。

**示例 1**：
```
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6]。
```

**示例 2**：
```
输入：intervals = [[1,4],[4,5]]
输出：[[1,5]]
解释：区间 [1,4] 和 [4,5] 可被视为重叠区间。
```

**约束条件**：
- 1 <= intervals.length <= 10⁴
- intervals[i].length == 2
- 0 <= starti <= endi <= 10⁴

---

## 解题思路

### 方法一：排序 + 贪心（推荐）

**思路**：
按起点排序后，依次合并重叠区间。

**复杂度**：
- 时间：O(n log n)
- 空间：O(log n)（排序栈空间）或 O(n)（结果列表）

**步骤**：
1. 按区间起点排序
2. 遍历排序后的区间
3. 如果重叠（当前起点 <= 上一个终点），合并
4. 如果不重叠，加入结果列表

---

## 代码实现

### Java

```java
class Solution {
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

        merged.add(current);  // 添加最后一个区间
        return merged.toArray(new int[merged.size()][]);
    }
}
```

### Python

```python
from typing import List

class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        if not intervals:
            return []

        # 按起点排序
        intervals.sort(key=lambda x: x[0])

        merged = []
        current = intervals[0]

        for interval in intervals[1:]:
            if interval[0] <= current[1]:
                # 有重叠，合并
                current[1] = max(current[1], interval[1])
            else:
                # 无重叠，加入结果
                merged.append(current)
                current = interval

        merged.append(current)
        return merged
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 按起点排序 | 确保相邻区间可能重叠 |
| 重叠判断 | interval[i][0] <= current[1] |
| 合并操作 | 取较大终点值 |
| 不重叠处理 | 加入结果，开始新区间 |
| [1,4] 和 [4,5] | 视为重叠（端点相接） |

---

## 图解

```
输入: [[1,3],[2,6],[8,10],[15,18]]

排序后: [[1,3],[2,6],[8,10],[15,18]]  (已排序)

处理过程:
current = [1,3]

[2,6]: 2 <= 3, 有重叠
      合并: current = [1, max(3,6)] = [1,6]

[8,10]: 8 > 6, 无重叠
       加入 [1,6], current = [8,10]

[15,18]: 15 > 10, 无重叠
        加入 [8,10], current = [15,18]

结束: 加入 [15,18]

结果: [[1,6],[8,10],[15,18]]

---

区间合并示意:
[1,3]   [2,6]          [8,10]   [15,18]
  ├──────┤  └──────────┘    ───────   ─────────
     [1,6]                       [8,10]   [15,18]

重叠判断:
[a, b] 和 [c, d]
如果 c <= b，则有重叠
合并后为 [a, max(b, d)]
```

---

## 变种/延伸

### 变种1：无重叠区间
- **题目**：[lc-435 无重叠区间](./lc-435-non-overlapping-intervals.md)
- **变化**：计算需要移除的最小区间数
- **解法**：贪心 + 按终点排序

### 变种2：插入区间
- **题目**：[lc-57 插入区间](./)
- **变化**：插入新区间并合并重叠部分
- **解法**：找到插入位置，合并前后

### 变种3：区间列表的交集
- **题目**：[lc-986 区间列表的交集](./)
- **变化**：求两个区间列表的交集
- **解法**：双指针遍历两个列表

### 进阶思考

1. **为什么 [1,4] 和 [4,5] 算重叠？**
   - 题目定义如此
   - 端点相接视为一个连续区间

2. **按终点排序可以吗？**
   - 可以，但合并逻辑不同
   - 按起点排序更直观

3. **如何判断三个区间是否重叠？**
   - 合并前两个后，再与第三个比较
   - 或者用区间树（线段树）维护

4. **如何返回合并次数？**
   - 每次合并时计数
   - 或 结果数量与原数量的差

5. **如果区间很多如何优化？**
   - 线段树：O(log n) 查询/更新
   - 树状数组：按坐标离散化
   - 适用需要频繁查询/更新的场景
