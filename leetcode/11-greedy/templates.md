# 贪心算法代码模板

贪心通过每步的局部最优达到全局最优。

---

## 1. 区间问题

### 1.1 合并区间

**适用场景**：重叠区间的合并

```
区间: [[1,3],[2,6],[8,10],[15,18]]

排序后: [[1,3],[2,6],[8,10],[15,18]]

合并过程:
[1,3] ∩ [2,6] = [2,3] 重叠 -> 合并为 [1,6]
[1,6] ∩ [8,10] = ∅ 不重叠
[1,6] ∩ [15,18] = ∅ 不重叠

结果: [[1,6],[8,10],[15,18]]
```

#### Java
```java
public int[][] merge(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

    List<int[]> merged = new ArrayList<>();

    for (int[] interval : intervals) {
        if (merged.isEmpty() || merged.get(merged.size() - 1)[1] < interval[0]) {
            merged.add(interval);
        } else {
            // 合并区间：更新结束位置
            merged.get(merged.size() - 1)[1] =
                Math.max(merged.get(merged.size() - 1)[1], interval[1]);
        }
    }

    return merged.toArray(new int[merged.size()][]);
}
```

#### Python
```python
def merge(intervals):
    intervals.sort(key=lambda x: x[0])

    merged = []

    for interval in intervals:
        if not merged or merged[-1][1] < interval[0]:
            merged.append(interval)
        else:
            merged[-1][1] = max(merged[-1][1], interval[1])

    return merged
```

---

### 1.2 无重叠区间

**适用场景**：计算需要移除的区间数量

```
区间: [[1,2],[2,3],[3,4],[1,3]]

排序后: [[1,2],[1,3],[2,3],[3,4]]

按结束位置排序: [[1,2],[2,3],[3,4],[1,3]]

贪心策略: 选择结束最早的区间
1. 选择 [1,2], end=2, count=1
2. [2,3] 的 start >= end(2>=2), 选择, end=3, count=2
3. [3,4] 的 start >= end(3>=3), 选择, end=4, count=3
4. [1,3] 的 start < end(1<4), 跳过

需要移除: 4 - 3 = 1 个区间
```

#### Java
```java
public int eraseOverlapIntervals(int[][] intervals) {
    if (intervals.length == 0) return 0;

    // 按结束位置排序
    Arrays.sort(intervals, (a, b) -> a[1] - b[1]);

    int count = 1;
    int end = intervals[0][1];

    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] >= end) {  // 不重叠
            count++;
            end = intervals[i][1];
        }
        // 跳过重叠区间
    }

    return intervals.length - count;
}
```

#### Python
```python
def erase_overlap_intervals(intervals):
    if not intervals:
        return 0

    intervals.sort(key=lambda x: x[1])

    count = 1
    end = intervals[0][1]

    for i in range(1, len(intervals)):
        if intervals[i][0] >= end:  # 不重叠
            count += 1
            end = intervals[i][1]

    return len(intervals) - count
```

---

## 2. 分发问题

### 2.1 分发饼干

**适用场景**：分配问题

```
孩子胃口 g = [1,2,3], 饼干大小 s = [1,1]

排序后: g = [1,2,3], s = [1,1]

贪心: 小饼干先满足小胃口
1. s[0]=1 >= g[0]=1, 满足孩子0, i++
2. s[1]=1 < g[1]=2, 无法满足

满足孩子数: 1
```

#### Java
```java
public int findContentChildren(int[] g, int[] s) {
    Arrays.sort(g);
    Arrays.sort(s);

    int child = 0, cookie = 0;

    while (child < g.length && cookie < s.length) {
        if (s[cookie] >= g[child]) {
            child++;  // 满足这个孩子
        }
        cookie++;  // 无论是否满足，都用掉这个饼干
    }

    return child;
}
```

#### Python
```python
def find_content_children(g, s):
    g.sort()
    s.sort()

    child = cookie = 0

    while child < len(g) and cookie < len(s):
        if s[cookie] >= g[child]:
            child += 1
        cookie += 1

    return child
```

---

## 3. 股票问题

### 3.1 买卖股票的最佳时机 II

**适用场景**：可以多次交易

```
价格: [7,1,5,3,6,4]

贪心策略: 只要后一天比前一天高，就交易
1->5: +4
3->6: +3

总利润: 4 + 3 = 7

图示:
  7
   \
    1---5---6
        \   \
         3   4

上涨段: [1,5] 和 [3,6]
```

#### Java
```java
public int maxProfit(int[] prices) {
    int profit = 0;

    for (int i = 1; i < prices.length; i++) {
        if (prices[i] > prices[i - 1]) {
            profit += prices[i] - prices[i - 1];
        }
    }

    return profit;
}
```

#### Python
```python
def max_profit(prices):
    profit = 0

    for i in range(1, len(prices)):
        if prices[i] > prices[i - 1]:
            profit += prices[i] - prices[i - 1]

    return profit
```

---

## 4. 跳跃游戏

### 4.1 跳跃游戏 II（最小跳跃次数）

**适用场景**：计算最小跳跃次数

```
数组: [2,3,1,1,4]

索引:   0  1  2  3  4

贪心: 在当前能跳的范围内选择能跳最远的
i=0: 可跳到 [1,2], 选择 maxReach=1+3=4 (跳到索引1)
i=1: 可跳到 [2,3,4], 选择 maxReach=1+1+3=5 (跳到索引4)
i=2: 跳过
i=3: 跳过
i=4: 到达末尾

跳跃: 0->1->4, 共 2 次
```

#### Java
```java
public int jump(int[] nums) {
    int jumps = 0;
    int currentEnd = 0;  // 当前跳跃能到达的边界
    int farthest = 0;    // 下一次能到达的最远位置

    for (int i = 0; i < nums.length - 1; i++) {
        farthest = Math.max(farthest, i + nums[i]);

        if (i == currentEnd) {  // 到达边界，必须跳跃
            jumps++;
            currentEnd = farthest;

            if (currentEnd >= nums.length - 1) break;
        }
    }

    return jumps;
}
```

#### Python
```python
def jump(nums):
    jumps = 0
    current_end = 0
    farthest = 0

    for i in range(len(nums) - 1):
        farthest = max(farthest, i + nums[i])

        if i == current_end:
            jumps += 1
            current_end = farthest

            if current_end >= len(nums) - 1:
                break

    return jumps
```

---

## 5. 复杂度总结

| 问题类型 | 时间复杂度 | 空间复杂度 | 关键 |
|---------|-----------|-----------|-----|
| 区间合并 | O(n log n) | O(1) 或 O(n) | 排序 |
| 无重叠区间 | O(n log n) | O(1) | 贪心选择 |
| 分发饼干 | O(n log n) | O(1) | 双指针 |
| 股票 II | O(n) | O(1) | 累加正收益 |
| 跳跃游戏 | O(n) | O(1) | 维护最远可达 |

**贪心 vs DP**：
- 贪心：局部最优 → 全局最优（不保证）
- DP：考虑所有可能，保证全局最优
