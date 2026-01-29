# 动态规划代码模板

动态规划（DP）通过存储子问题解避免重复计算。

---

## 1. 一维 DP 模板

### 1.1 基础一维 DP

**适用场景**：爬楼梯、打家劫舍

```
问题: 爬楼梯，每次可以爬 1 或 2 个台阶

dp[i] = 爬到第 i 阶的方法数

dp[0] = 1  (地面)
dp[1] = 1  (从 0 爬 1 步)
dp[2] = dp[1] + dp[0] = 2  (从 0 爬 2 步，或从 1 爬 1 步)
dp[3] = dp[2] + dp[1] = 3
```

#### Java
```java
public int climbStairs(int n) {
    if (n <= 1) return n;

    int[] dp = new int[n + 1];
    dp[0] = 1;
    dp[1] = 1;

    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];  // 状态转移
    }

    return dp[n];
}

// 空间优化
public int climbStairs(int n) {
    if (n <= 1) return n;

    int prev2 = 0, prev1 = 1;

    for (int i = 2; i <= n; i++) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }

    return prev1;
}
```

#### Python
```python
def climb_stairs(n):
    if n <= 1:
        return n

    dp = [0] * (n + 1)
    dp[0] = 1
    dp[1] = 1

    for i in range(2, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]

    return dp[n]

# 空间优化
def climb_stairs(n):
    if n <= 1:
        return n

    prev2, prev1 = 0, 1

    for i in range(2, n + 1):
        curr = prev1 + prev2
        prev2, prev1 = prev1, curr

    return prev1
```

---

## 2. 二维 DP 模板

### 2.1 标准二维 DP

**适用场景**：最长公共子序列、编辑距离

```
问题: LCS (text1="abcde", text2="ace")

dp[i][j] = text1[0..i-1] 和 text2[0..j-1] 的 LCS 长度

      ""  a   c   e
""     0   0   0   0
a      0   1   1   1
b      0   1   1   1
c      0   1   2   2
d      0   1   2   2
e      0   1   2   3

LCS = "ace", 长度 = 3
```

#### Java
```java
public int longestCommonSubsequence(String text1, String text2) {
    int m = text1.length(), n = text2.length();
    int[][] dp = new int[m + 1][n + 1];

    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }

    return dp[m][n];
}
```

#### Python
```python
def longest_common_subsequence(text1, text2):
    m, n = len(text1), len(text2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i - 1] == text2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

    return dp[m][n]
```

---

## 3. 0-1 背包模板

### 3.1 标准 0-1 背包

**适用场景**：分割等和子集、目标和

```
问题: 物品重量 [1,3,4], 容量 4

dp[j] = 容量为 j 时的最大价值

一维数组（倒序遍历）:
初始: dp = [0,0,0,0,0]

物品1 (w=1):
i=4: dp[4] = max(dp[4], dp[3]+v) = v
i=3: dp[3] = max(dp[3], dp[2]+v) = v
i=2: dp[2] = max(dp[2], dp[1]+v) = v
i=1: dp[1] = max(dp[1], dp[0]+v) = v

物品2 (w=3):
i=4: dp[4] = max(dp[4], dp[1]+v) = v+v
i=3: dp[3] = max(dp[3], dp[0]+v) = v

物品3 (w=4):
i=4: dp[4] = max(dp[4], dp[0]+v) = max(2v, v) = 2v

倒序原因: 每个物品只能用一次
```

#### Java
```java
public boolean canPartition(int[] nums) {
    int sum = 0;
    for (int num : nums) sum += num;
    if (sum % 2 != 0) return false;

    int target = sum / 2;
    boolean[] dp = new boolean[target + 1];
    dp[0] = true;

    for (int num : nums) {
        // 倒序遍历，避免重复使用
        for (int j = target; j >= num; j--) {
            dp[j] = dp[j] || dp[j - num];
        }
    }

    return dp[target];
}
```

#### Python
```python
def can_partition(nums):
    total = sum(nums)
    if total % 2 != 0:
        return False

    target = total // 2
    dp = [False] * (target + 1)
    dp[0] = True

    for num in nums:
        # 倒序遍历
        for j in range(target, num - 1, -1):
            dp[j] = dp[j] or dp[j - num]

    return dp[target]
```

---

## 4. LIS 模板（最长递增子序列）

### 4.1 标准 DP + 二分优化

**适用场景**：最长递增子序列

```
数组: [10, 9, 2, 5, 3, 7, 101, 18]

标准 DP:
dp[i] = 以 nums[i] 结尾的 LIS 长度
dp[0]=1, dp[1]=1, dp[2]=1, dp[3]=2, dp[4]=2, dp[5]=3, dp[6]=4, dp[7]=4
LIS = 4 ([2,3,7,18])

二分优化:
tails[i] = 长度为 i+1 的 LIS 的最小末尾元素

过程: 10 -> [10]
     9  -> [9]   (9<10, 替换)
     2  -> [2]   (2<9, 替换)
     5  -> [2,5]
     3  -> [2,3] (3<5, 替换)
     7  -> [2,3,7]
     101-> [2,3,7,101]
     18 -> [2,3,7,18] (18<101, 替换)

LIS = len(tails) = 4
```

#### Java
```java
// 方法1：标准 DP O(n²)
public int lengthOfLIS(int[] nums) {
    int n = nums.length;
    int[] dp = new int[n];
    Arrays.fill(dp, 1);

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
public int lengthOfLIS(int[] nums) {
    List<Integer> tails = new ArrayList<>();

    for (int num : nums) {
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
            tails.add(num);
        } else {
            tails.set(left, num);
        }
    }

    return tails.size();
}
```

#### Python
```python
import bisect

# 方法1：标准 DP
def length_of_lis(nums):
    n = len(nums)
    dp = [1] * n

    for i in range(1, n):
        for j in range(i):
            if nums[i] > nums[j]:
                dp[i] = max(dp[i], dp[j] + 1)

    return max(dp)

# 方法2：二分优化
def length_of_lis(nums):
    tails = []

    for num in nums:
        pos = bisect.bisect_left(tails, num)
        if pos == len(tails):
            tails.append(num)
        else:
            tails[pos] = num

    return len(tails)
```

---

## 5. 复杂度总结

| 问题类型 | 时间复杂度 | 空间复杂度 | 可优化 |
|---------|-----------|-----------|-------|
| 一维 DP | O(n) | O(n) → O(1) | 滚动变量 |
| 二维 DP | O(mn) | O(mn) → O(min(m,n)) | 滚动数组 |
| 0-1 背包 | O(nc) | O(c) | 倒序遍历 |
| 完全背包 | O(nc) | O(c) | 正序遍历 |
| LIS（标准） | O(n²) | O(n) | - |
| LIS（二分） | O(n log n) | O(n) | - |
| 编辑距离 | O(mn) | O(mn) → O(n) | 滚动数组 |
