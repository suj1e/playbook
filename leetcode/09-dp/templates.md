# 动态规划代码模板

动态规划（DP）是解决最优化问题的重要方法，核心思想是将问题分解为重叠子问题，通过存储子问题的解避免重复计算。

---

## 1. DP 解题五步法

1. **定义状态**：明确 dp 数组/变量的含义
2. **状态转移方程**：找出状态之间的关系
3. **初始化**：确定边界条件的初始值
4. **计算顺序**：确定遍历方向
5. **返回结果**：明确最终答案的位置

---

## 2. 一维 DP 模板

### 2.1 基础一维 DP

**适用场景**：爬楼梯、打家劫舍等

#### Java

```java
public int oneDimensionDP(int n) {
    if (n <= 1) return n;

    // 1. 定义状态：dp[i] 表示第 i 个问题的解
    int[] dp = new int[n + 1];

    // 2. 初始化
    dp[0] = 0;
    dp[1] = 1;

    // 3. 状态转移
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];  // 示例转移方程
    }

    // 4. 返回结果
    return dp[n];
}
```

#### Python

```python
def one_dimension_dp(n):
    if n <= 1:
        return n

    # 1. 定义状态
    dp = [0] * (n + 1)

    # 2. 初始化
    dp[0] = 0
    dp[1] = 1

    # 3. 状态转移
    for i in range(2, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]

    # 4. 返回结果
    return dp[n]
```

---

### 2.2 空间优化（滚动数组）

#### Java

```java
public int oneDimensionDPOptimized(int n) {
    if (n <= 1) return n;

    // 只保留需要的状态
    int prev2 = 0;  // dp[i-2]
    int prev1 = 1;  // dp[i-1]

    for (int i = 2; i <= n; i++) {
        int curr = prev1 + prev2;  // dp[i]
        prev2 = prev1;
        prev1 = curr;
    }

    return prev1;
}
```

#### Python

```python
def one_dimension_dp_optimized(n):
    if n <= 1:
        return n

    prev2 = 0  # dp[i-2]
    prev1 = 1  # dp[i-1]

    for i in range(2, n + 1):
        curr = prev1 + prev2
        prev2 = prev1
        prev1 = curr

    return prev1
```

---

## 3. 二维 DP 模板

### 3.1 标准二维 DP

**适用场景**：最长公共子序列、编辑距离、路径计数等

#### Java

```java
public int twoDimensionDP(String s1, String s2) {
    int m = s1.length(), n = s2.length();

    // 1. 定义状态：dp[i][j] 表示 s1[0..i-1] 和 s2[0..j-1] 的结果
    int[][] dp = new int[m + 1][n + 1];

    // 2. 初始化：第一行和第一列
    for (int i = 0; i <= m; i++) dp[i][0] = 0;
    for (int j = 0; j <= n; j++) dp[0][j] = 0;

    // 3. 状态转移
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }

    // 4. 返回结果
    return dp[m][n];
}
```

#### Python

```python
def two_dimension_dp(s1, s2):
    m, n = len(s1), len(s2)

    # 1. 定义状态
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    # 2. 初始化（默认为 0，可省略）

    # 3. 状态转移
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s1[i - 1] == s2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

    # 4. 返回结果
    return dp[m][n]
```

---

### 3.2 二维 DP 空间优化

#### Java

```java
public int twoDimensionDPOptimized(String s1, String s2) {
    int m = s1.length(), n = s2.length();

    // 只保留两行
    int[] prev = new int[n + 1];
    int[] curr = new int[n + 1];

    for (int i = 1; i <= m; i++) {
        curr = new int[n + 1];  // 重置当前行
        for (int j = 1; j <= n; j++) {
            if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                curr[j] = prev[j - 1] + 1;
            } else {
                curr[j] = Math.max(prev[j], curr[j - 1]);
            }
        }
        prev = curr;  // 更新前一行
    }

    return prev[n];
}
```

#### Python

```python
def two_dimension_dp_optimized(s1, s2):
    m, n = len(s1), len(s2)

    # 只保留两行
    prev = [0] * (n + 1)

    for i in range(1, m + 1):
        curr = [0] * (n + 1)
        for j in range(1, n + 1):
            if s1[i - 1] == s2[j - 1]:
                curr[j] = prev[j - 1] + 1
            else:
                curr[j] = max(prev[j], curr[j - 1])
        prev = curr

    return prev[n]
```

---

## 4. 0-1 背包模板

**适用场景**：分割等和子集、目标和等

### Java

```java
public boolean knapsack01(int[] weights, int[] values, int capacity) {
    int n = weights.length;

    // dp[j] 表示容量为 j 时的最大价值
    int[] dp = new int[capacity + 1];

    for (int i = 0; i < n; i++) {
        // 从后向前遍历，避免重复使用
        for (int j = capacity; j >= weights[i]; j--) {
            dp[j] = Math.max(dp[j], dp[j - weights[i]] + values[i]);
        }
    }

    return dp[capacity];
}

// 判断是否可以装满（分割等和子集）
public boolean canPartition(int[] nums) {
    int sum = 0;
    for (int num : nums) sum += num;
    if (sum % 2 != 0) return false;

    int target = sum / 2;
    boolean[] dp = new boolean[target + 1];
    dp[0] = true;

    for (int num : nums) {
        for (int j = target; j >= num; j--) {
            dp[j] = dp[j] || dp[j - num];
        }
    }

    return dp[target];
}
```

### Python

```python
def knapsack_01(weights, values, capacity):
    n = len(weights)

    # dp[j] 表示容量为 j 时的最大价值
    dp = [0] * (capacity + 1)

    for i in range(n):
        # 从后向前遍历
        for j in range(capacity, weights[i] - 1, -1):
            dp[j] = max(dp[j], dp[j - weights[i]] + values[i])

    return dp[capacity]

# 分割等和子集
def can_partition(nums):
    total = sum(nums)
    if total % 2 != 0:
        return False

    target = total // 2
    dp = [False] * (target + 1)
    dp[0] = True

    for num in nums:
        for j in range(target, num - 1, -1):
            dp[j] = dp[j] or dp[j - num]

    return dp[target]
```

---

## 5. 完全背包模板

**适用场景**：硬币兑换、组合总和等

### Java

```java
public int coinChange(int[] coins, int amount) {
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, amount + 1);  // 初始化为最大值
    dp[0] = 0;

    for (int coin : coins) {
        // 从前向后遍历（可重复使用）
        for (int j = coin; j <= amount; j++) {
            dp[j] = Math.min(dp[j], dp[j - coin] + 1);
        }
    }

    return dp[amount] > amount ? -1 : dp[amount];
}
```

### Python

```python
def coin_change(coins, amount):
    dp = [amount + 1] * (amount + 1)
    dp[0] = 0

    for coin in coins:
        # 从前向后遍历
        for j in range(coin, amount + 1):
            dp[j] = min(dp[j], dp[j - coin] + 1)

    return dp[amount] if dp[amount] <= amount else -1
```

---

## 6. 最长递增子序列（LIS）

### Java

```java
// 方法1：标准 DP O(n²)
public int lengthOfLIS(int[] nums) {
    int n = nums.length;
    int[] dp = new int[n];
    Arrays.fill(dp, 1);  // 每个数本身是一个长度为 1 的序列

    int maxLen = 1;
    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
        maxLen = Math.max(maxLen, dp[i]);
    }

    return maxLen;
}

// 方法2：二分优化 O(n log n)
public int lengthOfLISBinarySearch(int[] nums) {
    List<Integer> tails = new ArrayList<>();

    for (int num : nums) {
        // 二分查找第一个 >= num 的位置
        int left = 0, right = tails.size();
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (tails.get(mid) < num) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }

        if (left == tails.size()) {
            tails.add(num);  // num 比所有元素都大
        } else {
            tails.set(left, num);  // 替换
        }
    }

    return tails.size();
}
```

### Python

```python
import bisect

# 方法1：标准 DP O(n²)
def length_of_lis(nums):
    n = len(nums)
    dp = [1] * n

    for i in range(1, n):
        for j in range(i):
            if nums[i] > nums[j]:
                dp[i] = max(dp[i], dp[j] + 1)

    return max(dp)

# 方法2：二分优化 O(n log n)
def length_of_lis_binary(nums):
    tails = []

    for num in nums:
        # 二分查找第一个 >= num 的位置
        pos = bisect.bisect_left(tails, num)
        if pos == len(tails):
            tails.append(num)
        else:
            tails[pos] = num

    return len(tails)
```

---

## 7. 编辑距离

### Java

```java
public int minDistance(String word1, String word2) {
    int m = word1.length(), n = word2.length();

    // dp[i][j] 表示 word1[0..i-1] 转换为 word2[0..j-1] 的最小操作数
    int[][] dp = new int[m + 1][n + 1];

    // 初始化边界
    for (int i = 0; i <= m; i++) dp[i][0] = i;  // 删除 i 个字符
    for (int j = 0; j <= n; j++) dp[0][j] = j;  // 插入 j 个字符

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];  // 字符相同，无需操作
            } else {
                dp[i][j] = 1 + Math.min(
                    dp[i - 1][j - 1],  // 替换
                    Math.min(dp[i - 1][j],    // 删除
                             dp[i][j - 1])    // 插入
                );
            }
        }
    }

    return dp[m][n];
}
```

### Python

```python
def min_distance(word1, word2):
    m, n = len(word1), len(word2)

    # dp[i][j] 表示最小操作数
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    # 初始化边界
    for i in range(m + 1):
        dp[i][0] = i
    for j in range(n + 1):
        dp[0][j] = j

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if word1[i - 1] == word2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = 1 + min(
                    dp[i - 1][j - 1],  # 替换
                    dp[i - 1][j],      # 删除
                    dp[i][j - 1]       # 插入
                )

    return dp[m][n]
```

---

## 8. 区间 DP 模板

**适用场景**：最长回文子序列、矩阵连乘等

### Java

```java
public int longestPalindromeSubseq(String s) {
    int n = s.length();

    // dp[i][j] 表示 s[i..j] 的最长回文子序列长度
    int[][] dp = new int[n][n];

    // 单个字符是长度为 1 的回文
    for (int i = 0; i < n; i++) {
        dp[i][i] = 1;
    }

    // 按区间长度递增的顺序遍历
    for (int len = 2; len <= n; len++) {
        for (int i = 0; i <= n - len; i++) {
            int j = i + len - 1;

            if (s.charAt(i) == s.charAt(j)) {
                dp[i][j] = dp[i + 1][j - 1] + 2;
            } else {
                dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
    }

    return dp[0][n - 1];
}
```

### Python

```python
def longest_palindrome_subseq(s):
    n = len(s)

    # dp[i][j] 表示 s[i..j] 的最长回文子序列长度
    dp = [[0] * n for _ in range(n)]

    # 单个字符
    for i in range(n):
        dp[i][i] = 1

    # 按区间长度递增遍历
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1

            if s[i] == s[j]:
                dp[i][j] = dp[i + 1][j - 1] + 2
            else:
                dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])

    return dp[0][n - 1]
```

---

## 9. 状态机 DP 模板

**适用场景**：股票买卖、打家劫舍 II 等

### 股票买卖（一次交易）

#### Java

```java
public int maxProfit(int[] prices) {
    int n = prices.length;

    // hold: 持有股票时的最大收益
    // cash: 不持有股票时的最大收益
    int hold = Integer.MIN_VALUE, cash = 0;

    for (int price : prices) {
        // 保持持有 或 买入
        hold = Math.max(hold, -price);
        // 保持不持有 或 卖出
        cash = Math.max(cash, hold + price);
    }

    return cash;
}
```

#### Python

```python
def max_profit(prices):
    hold = float('-inf')  # 持有股票
    cash = 0              # 不持有股票

    for price in prices:
        hold = max(hold, -price)
        cash = max(cash, hold + price)

    return cash
```

---

### 股票买卖（无限次交易）

#### Java

```java
public int maxProfit(int[] prices) {
    int hold = Integer.MIN_VALUE, cash = 0;

    for (int price : prices) {
        int prevHold = hold;
        hold = Math.max(hold, cash - price);
        cash = Math.max(cash, prevHold + price);
    }

    return cash;
}
```

#### Python

```python
def max_profit(prices):
    hold = float('-inf')
    cash = 0

    for price in prices:
        prev_hold = hold
        hold = max(hold, cash - price)
        cash = max(cash, prev_hold + price)

    return cash
```

---

## 10. DP 复杂度总结

| 问题类型 | 时间复杂度 | 空间复杂度 | 可优化 |
|---------|-----------|-----------|-------|
| 一维 DP | O(n) | O(n) → O(1) | 滚动变量 |
| 二维 DP | O(mn) | O(mn) → O(min(m,n)) | 滚动数组 |
| 0-1 背包 | O(nc) | O(c) | 倒序遍历 |
| 完全背包 | O(nc) | O(c) | 正序遍历 |
| LIS（标准） | O(n²) | O(n) | - |
| LIS（二分） | O(n log n) | O(n) | - |
| 编辑距离 | O(mn) | O(mn) → O(n) | 滚动数组 |
| 区间 DP | O(n³) | O(n²) | 难优化 |

---

## 11. DP 问题识别特征

| 特征 | 可能是 DP |
|------|----------|
| 求最大/最小值 | ✅ |
| 求方案数量 | ✅ |
| 求可行性 | ✅ |
| 存在重叠子问题 | ✅ |
| 满足最优子结构 | ✅ |
| 需要枚举所有可能 | ❌（用回溯） |
| 数据范围大 | ❌（可能需要贪心） |

---

## 12. 常见陷阱

| 陷阱 | 说明 | 解决方案 |
|------|------|----------|
| 初始化错误 | 边界条件没考虑清楚 | 仔细分析 dp[0] 或 dp[0][j] 的含义 |
| 遍历顺序错误 | 依赖的状态还没计算 | 确保被依赖的状态先计算 |
| 空间优化错误 | 0-1 背包要倒序，完全背包要正序 | 理解能否重复使用 |
| 状态定义错误 | dp 含义不明确 | 写注释明确 dp[i] 表示什么 |
| 越界访问 | 数组索引超出范围 | 检查循环条件和数组大小 |
| 整数溢出 | 结果超过 int 范围 | 使用 long 或取模 |
