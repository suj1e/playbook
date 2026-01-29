# 贪心算法代码模板

贪心算法是一种在每一步选择中都采取当前状态下最优决策的算法，期望通过局部最优达到全局最优。贪心算法不保证能得到最优解，但在某些问题中确实有效。

---

## 1. 贪心算法核心思想

**核心**：局部最优 → 全局最优

**适用条件**：
1. 贪心选择性质：每一步的贪心选择最终能导致问题的最优解
2. 最优子结构：问题的最优解包含子问题的最优解

**解题步骤**：
1. 分析问题，确定贪心策略
2. 证明贪心策略的正确性（反证法、数学归纳法）
3. 实现代码

---

## 2. 区间问题模板

### 2.1 区间排序

```java
// Java 排序区间
int[][] intervals = {{1, 3}, {2, 6}, {8, 10}};

// 按起始位置排序
Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

// 按结束位置排序
Arrays.sort(intervals, (a, b) -> a[1] - b[1]);

// 按区间长度排序
Arrays.sort(intervals, (a, b) -> (a[1] - a[0]) - (b[1] - b[0]));
```

```python
# Python 排序区间
intervals = [[1, 3], [2, 6], [8, 10]]

# 按起始位置排序
intervals.sort(key=lambda x: x[0])

# 按结束位置排序
intervals.sort(key=lambda x: x[1])

# 按区间长度排序
intervals.sort(key=lambda x: x[1] - x[0])
```

---

### 2.2 合并区间

#### Java

```java
public int[][] merge(int[][] intervals) {
    // 按起始位置排序
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

    List<int[]> merged = new ArrayList<>();

    for (int[] interval : intervals) {
        // 如果结果为空，或当前区间不与最后一个区间重叠
        if (merged.isEmpty() || merged.get(merged.size() - 1)[1] < interval[0]) {
            merged.add(interval);
        } else {
            // 合并区间：更新结束位置为较大值
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
    # 按起始位置排序
    intervals.sort(key=lambda x: x[0])

    merged = []

    for interval in intervals:
        # 如果结果为空，或当前区间不与最后一个区间重叠
        if not merged or merged[-1][1] < interval[0]:
            merged.append(interval)
        else:
            # 合并区间
            merged[-1][1] = max(merged[-1][1], interval[1])

    return merged
```

---

### 2.3 无重叠区间

#### Java

```java
public int eraseOverlapIntervals(int[][] intervals) {
    if (intervals.length == 0) return 0;

    // 按结束位置排序（贪心：选择结束最早的区间）
    Arrays.sort(intervals, (a, b) -> a[1] - b[1]);

    int count = 1;  // 保留的区间数量
    int end = intervals[0][1];

    for (int i = 1; i < intervals.length; i++) {
        // 当前区间的起始 >= 上一个保留区间的结束，不重叠
        if (intervals[i][0] >= end) {
            count++;
            end = intervals[i][1];
        }
        // 否则跳过（删除当前区间）
    }

    return intervals.length - count;  // 需要删除的区间数
}
```

#### Python

```python
def erase_overlap_intervals(intervals):
    if not intervals:
        return 0

    # 按结束位置排序
    intervals.sort(key=lambda x: x[1])

    count = 1  # 保留的区间数量
    end = intervals[0][1]

    for i in range(1, len(intervals)):
        if intervals[i][0] >= end:
            count += 1
            end = intervals[i][1]

    return len(intervals) - count
```

---

### 2.4 用最少数量的箭引爆气球

#### Java

```java
public int findMinArrowShots(int[][] points) {
    if (points.length == 0) return 0;

    // 按结束位置排序
    Arrays.sort(points, (a, b) -> Integer.compare(a[1], b[1]));

    int arrows = 1;
    int end = points[0][1];

    for (int i = 1; i < points.length; i++) {
        // 当前气球的起始 > 上一箭的位置，需要新的一箭
        if (points[i][0] > end) {
            arrows++;
            end = points[i][1];
        }
        // 否则一箭可以同时引爆
    }

    return arrows;
}
```

#### Python

```python
def findMinArrowShots(points):
    if not points:
        return 0

    # 按结束位置排序
    points.sort(key=lambda x: x[1])

    arrows = 1
    end = points[0][1]

    for i in range(1, len(points)):
        if points[i][0] > end:
            arrows += 1
            end = points[i][1]

    return arrows
```

---

## 3. 分发问题模板

### 3.1 分发饼干

#### Java

```java
public int findContentChildren(int[] g, int[] s) {
    // g: 孩子的胃口, s: 饼干的大小
    Arrays.sort(g);
    Arrays.sort(s);

    int child = 0, cookie = 0;

    while (child < g.length && cookie < s.length) {
        // 当前饼干能满足当前孩子
        if (s[cookie] >= g[child]) {
            child++;  // 满足这个孩子
        }
        cookie++;  // 无论是否满足，都要用掉这个饼干
    }

    return child;  // 被满足的孩子数量
}
```

#### Python

```python
def findContentChildren(g, s):
    # g: 孩子的胃口, s: 饼干的大小
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

### 3.2 柠檬水找零

#### Java

```java
public boolean lemonadeChange(int[] bills) {
    int five = 0, ten = 0;  // 5元和10元的数量

    for (int bill : bills) {
        if (bill == 5) {
            five++;
        } else if (bill == 10) {
            if (five == 0) return false;  // 没有5元找零
            five--;
            ten++;
        } else {  // bill == 20
            if (ten > 0 && five > 0) {  // 优先用10+5找零
                ten--;
                five--;
            } else if (five >= 3) {  // 否则用5+5+5找零
                five -= 3;
            } else {
                return false;
            }
        }
    }

    return true;
}
```

#### Python

```python
def lemonadeChange(bills):
    five = ten = 0

    for bill in bills:
        if bill == 5:
            five += 1
        elif bill == 10:
            if five == 0:
                return False
            five -= 1
            ten += 1
        else:  # bill == 20
            if ten > 0 and five > 0:
                ten -= 1
                five -= 1
            elif five >= 3:
                five -= 3
            else:
                return False

    return True
```

---

## 4. 股票问题（贪心）

### 4.1 买卖股票的最佳时机 II

**贪心策略**：只要后一天比前一天高，就进行交易

#### Java

```java
public int maxProfit(int[] prices) {
    int profit = 0;

    for (int i = 1; i < prices.length; i++) {
        // 只要有正收益就交易
        if (prices[i] > prices[i - 1]) {
            profit += prices[i] - prices[i - 1];
        }
    }

    return profit;
}
```

#### Python

```python
def maxProfit(prices):
    profit = 0

    for i in range(1, len(prices)):
        if prices[i] > prices[i - 1]:
            profit += prices[i] - prices[i - 1]

    return profit
```

---

## 5. 跳跃问题

### 5.1 跳跃游戏

#### Java

```java
public boolean canJump(int[] nums) {
    int maxReach = 0;  // 能到达的最远位置

    for (int i = 0; i < nums.length; i++) {
        // 如果当前位置不能到达
        if (i > maxReach) return false;

        // 更新能到达的最远位置
        maxReach = Math.max(maxReach, i + nums[i]);

        // 如果已经能到达或超过最后一个位置
        if (maxReach >= nums.length - 1) return true;
    }

    return true;
}
```

#### Python

```python
def canJump(nums):
    max_reach = 0

    for i in range(len(nums)):
        if i > max_reach:
            return False

        max_reach = max(max_reach, i + nums[i])

        if max_reach >= len(nums) - 1:
            return True

    return True
```

---

### 5.2 跳跃游戏 II（最小跳跃次数）

#### Java

```java
public int jump(int[] nums) {
    if (nums.length == 1) return 0;

    int jumps = 0;
    int currentEnd = 0;  // 当前跳跃能到达的边界
    int farthest = 0;    // 下一次跳跃能到达的最远位置

    for (int i = 0; i < nums.length - 1; i++) {
        // 更新下一次能到达的最远位置
        farthest = Math.max(farthest, i + nums[i]);

        // 到达当前跳跃的边界，必须进行下一次跳跃
        if (i == currentEnd) {
            jumps++;
            currentEnd = farthest;

            // 如果已经能到达最后一个位置
            if (currentEnd >= nums.length - 1) break;
        }
    }

    return jumps;
}
```

#### Python

```python
def jump(nums):
    if len(nums) == 1:
        return 0

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

## 6. 贪心 + 堆

### 6.1 分配金子

#### Java

```java
import java.util.*;

public class Solution {
    public int findMinArrowShots(int[][] points) {
        if (points.length == 0) return 0;

        // 按起始位置排序
        Arrays.sort(points, (a, b) -> a[0] - b[0]);

        // 最小堆，存储区间的结束位置
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();

        for (int[] point : points) {
            // 如果堆为空，或当前区间与堆顶区间不重叠
            if (minHeap.isEmpty() || point[0] > minHeap.peek()) {
                minHeap.offer(point[1]);
            } else {
                // 重叠，更新堆顶为较小的结束位置
                minHeap.poll();
                minHeap.offer(Math.min(minHeap.peek(), point[1]));
            }
        }

        return minHeap.size();
    }
}
```

#### Python

```python
import heapq

def findMinArrowShots(points):
    if not points:
        return 0

    # 按起始位置排序
    points.sort(key=lambda x: x[0])

    # 最小堆，存储区间的结束位置
    min_heap = []

    for point in points:
        if not min_heap or point[0] > min_heap[0]:
            heapq.heappush(min_heap, point[1])
        else:
            heapq.heappop(min_heap)
            heapq.heappush(min_heap, min(min_heap[0], point[1]))

    return len(min_heap)
```

---

## 7. 字符串贪心

### 7.1 分割平衡字符串

#### Java

```java
public int balancedStringSplit(String s) {
    int balance = 0;
    int count = 0;

    for (char c : s.toCharArray()) {
        if (c == 'L') {
            balance++;
        } else {
            balance--;
        }

        // 平衡时计数
        if (balance == 0) {
            count++;
        }
    }

    return count;
}
```

#### Python

```python
def balancedStringSplit(s):
    balance = count = 0

    for c in s:
        if c == 'L':
            balance += 1
        else:
            balance -= 1

        if balance == 0:
            count += 1

    return count
```

---

## 8. K 次取反后最大化的数组和

#### Java

```java
public int largestSumAfterKNegations(int[] nums, int k) {
    // 按绝对值从大到小排序
    Arrays.sort(nums, (a, b) -> Math.abs(b) - Math.abs(a));

    for (int i = 0; i < nums.length; i++) {
        // 还可以取反，且当前数为负数
        if (k > 0 && nums[i] < 0) {
            nums[i] = -nums[i];
            k--;
        }
    }

    // 如果 k 还剩下且为奇数，将绝对值最小的数取反
    if (k % 2 == 1) {
        nums[nums.length - 1] = -nums[nums.length - 1];
    }

    // 计算总和
    int sum = 0;
    for (int num : nums) {
        sum += num;
    }

    return sum;
}
```

#### Python

```python
def largestSumAfterKNegations(nums, k):
    # 按绝对值从大到小排序
    nums.sort(key=lambda x: -abs(x))

    for i in range(len(nums)):
        if k > 0 and nums[i] < 0:
            nums[i] = -nums[i]
            k -= 1

    if k % 2 == 1:
        nums[-1] = -nums[-1]

    return sum(nums)
```

---

## 9. 摆动序列

#### Java

```java
public int wiggleMaxLength(int[] nums) {
    if (nums.length < 2) return nums.length;

    int up = 1, down = 1;  // 上升和下降的序列长度

    for (int i = 1; i < nums.length; i++) {
        if (nums[i] > nums[i - 1]) {
            up = down + 1;  // 上升序列长度 = 下降序列长度 + 1
        } else if (nums[i] < nums[i - 1]) {
            down = up + 1;  // 下降序列长度 = 上升序列长度 + 1
        }
        // 相等时，序列长度不变
    }

    return Math.max(up, down);
}
```

#### Python

```python
def wiggleMaxLength(nums):
    if len(nums) < 2:
        return len(nums)

    up = down = 1

    for i in range(1, len(nums)):
        if nums[i] > nums[i - 1]:
            up = down + 1
        elif nums[i] < nums[i - 1]:
            down = up + 1

    return max(up, down)
```

---

## 10. 复杂度总结

| 问题类型 | 时间复杂度 | 空间复杂度 | 关键操作 |
|---------|-----------|-----------|---------|
| 区间合并 | O(n log n) | O(1) 或 O(n) | 排序 + 遍历 |
| 无重叠区间 | O(n log n) | O(1) | 排序 + 贪心选择 |
| 股票买卖 | O(n) | O(1) | 遍历求正值和 |
| 跳跃游戏 | O(n) | O(1) | 维护最远可达位置 |
| 分发饼干 | O(n log n) | O(1) | 双指针 |

---

## 11. 贪心 vs 动态规划

| 特性 | 贪心 | 动态规划 |
|------|------|---------|
| 选择方式 | 每步选当前最优 | 考虑所有可能 |
| 最优解 | 不保证全局最优 | 保证全局最优 |
| 时间复杂度 | 通常较低 | 通常较高 |
| 适用问题 | 特定结构问题 | 具有最优子结构 |
| 状态转移 | 无需考虑之前状态 | 需要考虑之前状态 |

**选择建议**：
- 能用贪心就用贪心（效率高）
- 不确定是否可行时，用 DP（保证正确性）
- 需要证明贪心策略的正确性

---

## 12. 贪心算法证明方法

### 12.1 反证法

假设存在某个最优解不包含贪心选择，然后证明可以替换为贪心选择而不使解变差。

**示例**：活动选择问题
- 贪心策略：选择结束时间最早的活动
- 证明：假设最优解的第一个活动 A 不是结束最早的，替换为结束最早的活动 B，B 结束更早，为后续活动留出更多时间，解不会变差

### 12.2 数学归纳法

证明在每一步贪心选择后，剩余子问题的最优解与原问题的最优解一致。

### 12.3 交换论证法

证明任意最优解都可以通过一系列变换转化为贪心解，且解不变差。

---

## 13. 常见陷阱

| 陷阱 | 说明 | 解决方案 |
|------|------|----------|
| 局部最优 ≠ 全局最优 | 贪心策略不一定正确 | 需要证明或找反例 |
| 排序关键字错误 | 区间问题排序方式多样 | 根据题意选择排序方式 |
| 边界条件 | 开区间 vs 闭区间 | 明确区间端点的含义 |
| 贪心顺序错误 | 先操作哪个元素有影响 | 确定正确的操作顺序 |
| 忽略特殊情况 | 如全相等元素 | 考虑边界情况 |
| 整数溢出 | 累加结果可能溢出 | 使用 long 或提前取模 |
