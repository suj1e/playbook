# 回溯算法代码模板

回溯算法是一种通过穷举所有可能情况来寻找问题解决方案的算法，本质是深度优先搜索（DFS）+ 状态重置。常用于解决组合、排列、子集、搜索问题。

---

## 1. 回溯算法核心模板

### 1.1 通用回溯框架

#### Java

```java
public void backtrack(参数列表) {
    // 终止条件
    if (满足终止条件) {
        收集结果;
        return;
    }

    // 单层搜索逻辑
    for (选择 : 本层集合中的所有选择) {
        // 处理节点
        做选择;

        // 递归
        backtrack(下一层参数);

        // 回溯：撤销选择
        撤销选择;
    }
}
```

#### Python

```python
def backtrack(parameters):
    # 终止条件
    if 满足终止条件:
        收集结果
        return

    # 单层搜索逻辑
    for 选择 in 本层集合中的所有选择:
        # 处理节点
        做选择

        # 递归
        backtrack(下一层参数)

        # 回溯：撤销选择
        撤销选择
```

---

## 2. 组合问题模板

**特点**：给定 n 个数，找出所有满足条件的组合，不考虑顺序

### 2.1 标准组合（C(n, k)）

#### Java

```java
class Solution {
    private List<List<Integer>> result = new ArrayList<>();
    private LinkedList<Integer> path = new LinkedList<>();

    public List<List<Integer>> combine(int n, int k) {
        backtrack(n, k, 1);
        return result;
    }

    private void backtrack(int n, int k, int startIndex) {
        // 终止条件：path 大小等于 k
        if (path.size() == k) {
            result.add(new ArrayList<>(path));
            return;
        }

        // 从 startIndex 开始遍历
        for (int i = startIndex; i <= n; i++) {
            path.add(i);           // 处理节点
            backtrack(n, k, i + 1); // 递归
            path.removeLast();     // 回溯
        }
    }
}
```

#### Python

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        result = []
        path = []

        def backtrack(start_index):
            # 终止条件
            if len(path) == k:
                result.append(path.copy())
                return

            # 从 start_index 开始遍历
            for i in range(start_index, n + 1):
                path.append(i)           # 处理节点
                backtrack(i + 1)         # 递归
                path.pop()               # 回溯

        backtrack(1)
        return result
```

---

### 2.2 组合总和（可重复使用）

#### Java

```java
class Solution {
    private List<List<Integer>> result = new ArrayList<>();
    private LinkedList<Integer> path = new LinkedList<>();

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);  // 排序便于剪枝
        backtrack(candidates, target, 0, 0);
        return result;
    }

    private void backtrack(int[] candidates, int target, int sum, int startIndex) {
        // 终止条件
        if (sum == target) {
            result.add(new ArrayList<>(path));
            return;
        }

        // 从 startIndex 开始（可重复使用当前元素）
        for (int i = startIndex; i < candidates.length; i++) {
            // 剪枝：如果加上当前数超过 target，直接结束
            if (sum + candidates[i] > target) break;

            path.add(candidates[i]);
            backtrack(candidates, target, sum + candidates[i], i);  // 注意是 i 不是 i+1
            path.removeLast();
        }
    }
}
```

#### Python

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []
        path = []

        candidates.sort()  # 排序便于剪枝

        def backtrack(start_index, current_sum):
            # 终止条件
            if current_sum == target:
                result.append(path.copy())
                return

            for i in range(start_index, len(candidates)):
                # 剪枝
                if current_sum + candidates[i] > target:
                    break

                path.append(candidates[i])
                backtrack(i, current_sum + candidates[i])  # i 而不是 i+1
                path.pop()

        backtrack(0, 0)
        return result
```

---

## 3. 排列问题模板

**特点**：考虑顺序，[1,2] 和 [2,1] 是不同的排列

### 3.1 标准全排列

#### Java

```java
class Solution {
    private List<List<Integer>> result = new ArrayList<>();
    private LinkedList<Integer> path = new LinkedList<>();

    public List<List<Integer>> permute(int[] nums) {
        boolean[] used = new boolean[nums.length];
        backtrack(nums, used);
        return result;
    }

    private void backtrack(int[] nums, boolean[] used) {
        // 终止条件
        if (path.size() == nums.length) {
            result.add(new ArrayList<>(path));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (used[i]) continue;  // 已使用过，跳过

            path.add(nums[i]);
            used[i] = true;
            backtrack(nums, used);
            path.removeLast();
            used[i] = false;  // 回溯
        }
    }
}
```

#### Python

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        result = []
        path = []
        used = [False] * len(nums)

        def backtrack():
            # 终止条件
            if len(path) == len(nums):
                result.append(path.copy())
                return

            for i in range(len(nums)):
                if used[i]:
                    continue

                path.append(nums[i])
                used[i] = True
                backtrack()
                path.pop()
                used[i] = False

        backtrack()
        return result
```

---

### 3.2 含重复元素的全排列（去重）

#### Java

```java
class Solution {
    private List<List<Integer>> result = new ArrayList<>();
    private LinkedList<Integer> path = new LinkedList<>();

    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);  // 排序，让相同元素相邻
        boolean[] used = new boolean[nums.length];
        backtrack(nums, used);
        return result;
    }

    private void backtrack(int[] nums, boolean[] used) {
        if (path.size() == nums.length) {
            result.add(new ArrayList<>(path));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            // 去重逻辑
            if (used[i]) continue;
            if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) continue;

            path.add(nums[i]);
            used[i] = true;
            backtrack(nums, used);
            path.removeLast();
            used[i] = false;
        }
    }
}
```

#### Python

```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        result = []
        path = []
        nums.sort()  # 排序
        used = [False] * len(nums)

        def backtrack():
            if len(path) == len(nums):
                result.append(path.copy())
                return

            for i in range(len(nums)):
                # 去重逻辑
                if used[i]:
                    continue
                if i > 0 and nums[i] == nums[i - 1] and not used[i - 1]:
                    continue

                path.append(nums[i])
                used[i] = True
                backtrack()
                path.pop()
                used[i] = False

        backtrack()
        return result
```

---

## 4. 子集问题模板

**特点**：集合的所有子集，包括空集

### 4.1 不含重复元素的子集

#### Java

```java
class Solution {
    private List<List<Integer>> result = new ArrayList<>();
    private LinkedList<Integer> path = new LinkedList<>();

    public List<List<Integer>> subsets(int[] nums) {
        backtrack(nums, 0);
        return result;
    }

    private void backtrack(int[] nums, int startIndex) {
        // 每个节点都是子集，都要收集结果
        result.add(new ArrayList<>(path));

        for (int i = startIndex; i < nums.length; i++) {
            path.add(nums[i]);
            backtrack(nums, i + 1);
            path.removeLast();
        }
    }
}
```

#### Python

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        result = []
        path = []

        def backtrack(start_index):
            # 每个节点都是子集
            result.append(path.copy())

            for i in range(start_index, len(nums)):
                path.append(nums[i])
                backtrack(i + 1)
                path.pop()

        backtrack(0)
        return result
```

---

### 4.2 含重复元素的子集（去重）

#### Java

```java
class Solution {
    private List<List<Integer>> result = new ArrayList<>();
    private LinkedList<Integer> path = new LinkedList<>();

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);  // 排序
        backtrack(nums, 0);
        return result;
    }

    private void backtrack(int[] nums, int startIndex) {
        result.add(new ArrayList<>(path));

        for (int i = startIndex; i < nums.length; i++) {
            // 去重：同一层不能使用重复元素
            if (i > startIndex && nums[i] == nums[i - 1]) continue;

            path.add(nums[i]);
            backtrack(nums, i + 1);
            path.removeLast();
        }
    }
}
```

#### Python

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        result = []
        path = []
        nums.sort()  # 排序

        def backtrack(start_index):
            result.append(path.copy())

            for i in range(start_index, len(nums)):
                # 去重
                if i > start_index and nums[i] == nums[i - 1]:
                    continue

                path.append(nums[i])
                backtrack(i + 1)
                path.pop()

        backtrack(0)
        return result
```

---

## 5. 分割问题模板

**适用场景**：分割回文串、表达式添加运算符等

### 5.1 分割回文串

#### Java

```java
class Solution {
    private List<List<String>> result = new ArrayList<>();
    private LinkedList<String> path = new LinkedList<>();

    public List<List<String>> partition(String s) {
        backtrack(s, 0);
        return result;
    }

    private void backtrack(String s, int startIndex) {
        // 终止条件：到达字符串末尾
        if (startIndex >= s.length()) {
            result.add(new ArrayList<>(path));
            return;
        }

        for (int i = startIndex; i < s.length(); i++) {
            // 截取子串
            String sub = s.substring(startIndex, i + 1);

            // 判断是否是回文
            if (!isPalindrome(sub)) continue;

            path.add(sub);
            backtrack(s, i + 1);
            path.removeLast();
        }
    }

    private boolean isPalindrome(String s) {
        int left = 0, right = s.length() - 1;
        while (left < right) {
            if (s.charAt(left++) != s.charAt(right--)) return false;
        }
        return true;
    }
}
```

#### Python

```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        result = []
        path = []

        def backtrack(start_index):
            # 终止条件
            if start_index >= len(s):
                result.append(path.copy())
                return

            for i in range(start_index, len(s)):
                sub = s[start_index:i + 1]

                # 判断是否是回文
                if sub != sub[::-1]:
                    continue

                path.append(sub)
                backtrack(i + 1)
                path.pop()

        backtrack(0)
        return result
```

---

## 6. 棋盘问题模板

### 6.1 N 皇后

#### Java

```java
class Solution {
    private List<List<String>> result = new ArrayList<>();

    public List<List<String>> solveNQueens(int n) {
        char[][] board = new char[n][n];
        for (char[] row : board) {
            Arrays.fill(row, '.');
        }
        backtrack(board, n, 0);
        return result;
    }

    private void backtrack(char[][] board, int n, int row) {
        // 终止条件：最后一行已放置
        if (row == n) {
            result.add(construct(board));
            return;
        }

        for (int col = 0; col < n; col++) {
            // 检查是否可以放置
            if (isValid(board, row, col, n)) {
                board[row][col] = 'Q';
                backtrack(board, n, row + 1);
                board[row][col] = '.';  // 回溯
            }
        }
    }

    private boolean isValid(char[][] board, int row, int col, int n) {
        // 检查列
        for (int i = 0; i < row; i++) {
            if (board[i][col] == 'Q') return false;
        }

        // 检查 45° 对角线
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] == 'Q') return false;
        }

        // 检查 135° 对角线
        for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (board[i][j] == 'Q') return false;
        }

        return true;
    }

    private List<String> construct(char[][] board) {
        List<String> res = new ArrayList<>();
        for (char[] row : board) {
            res.add(new String(row));
        }
        return res;
    }
}
```

#### Python

```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        result = []
        board = [['.' for _ in range(n)] for _ in range(n)]

        def backtrack(row):
            # 终止条件
            if row == n:
                result.append([''.join(row) for row in board])
                return

            for col in range(n):
                if self.is_valid(board, row, col, n):
                    board[row][col] = 'Q'
                    backtrack(row + 1)
                    board[row][col] = '.'  # 回溯

        backtrack(0)
        return result

    def is_valid(self, board, row, col, n):
        # 检查列
        for i in range(row):
            if board[i][col] == 'Q':
                return False

        # 检查 45° 对角线
        for i, j in zip(range(row - 1, -1, -1), range(col - 1, -1, -1)):
            if board[i][j] == 'Q':
                return False

        # 检查 135° 对角线
        for i, j in zip(range(row - 1, -1, -1), range(col + 1, n)):
            if board[i][j] == 'Q':
                return False

        return True
```

---

### 6.2 解数独

#### Java

```java
class Solution {
    public void solveSudoku(char[][] board) {
        backtrack(board);
    }

    private boolean backtrack(char[][] board) {
        for (int row = 0; row < 9; row++) {
            for (int col = 0; col < 9; col++) {
                if (board[row][col] != '.') continue;  // 跳过已填位置

                for (char num = '1'; num <= '9'; num++) {
                    if (isValid(board, row, col, num)) {
                        board[row][col] = num;

                        if (backtrack(board)) {
                            return true;
                        }

                        board[row][col] = '.';  // 回溯
                    }
                }
                return false;  // 1-9 都不合适
            }
        }
        return true;  // 全部填完
    }

    private boolean isValid(char[][] board, int row, int col, char num) {
        // 检查行
        for (int i = 0; i < 9; i++) {
            if (board[row][i] == num) return false;
        }

        // 检查列
        for (int i = 0; i < 9; i++) {
            if (board[i][col] == num) return false;
        }

        // 检查 3x3 宫
        int startRow = (row / 3) * 3;
        int startCol = (col / 3) * 3;
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (board[startRow + i][startCol + j] == num) return false;
            }
        }

        return true;
    }
}
```

#### Python

```python
class Solution:
    def solveSudoku(self, board: List[List[str]]) -> None:
        self.backtrack(board)

    def backtrack(self, board):
        for row in range(9):
            for col in range(9):
                if board[row][col] != '.':
                    continue

                for num in map(str, range(1, 10)):
                    if self.is_valid(board, row, col, num):
                        board[row][col] = num

                        if self.backtrack(board):
                            return True

                        board[row][col] = '.'  # 回溯

                return False  # 1-9 都不合适

        return True  # 全部填完

    def is_valid(self, board, row, col, num):
        # 检查行
        for i in range(9):
            if board[row][i] == num:
                return False

        # 检查列
        for i in range(9):
            if board[i][col] == num:
                return False

        # 检查 3x3 宫
        start_row, start_col = (row // 3) * 3, (col // 3) * 3
        for i in range(3):
            for j in range(3):
                if board[start_row + i][start_col + j] == num:
                    return False

        return True
```

---

## 7. 剪枝优化技巧

### 7.1 排序 + 剪枝

```java
// 排序后，可以在循环中提前退出
Arrays.sort(candidates);  // 排序

for (int i = startIndex; i < candidates.length; i++) {
    // 剪枝：如果当前数已超过目标，后面的更大，直接退出
    if (sum + candidates[i] > target) break;

    // ... 正常逻辑
}
```

### 7.2 组合中的剪枝

```java
// 组合问题中，如果剩下的元素不够凑齐 k 个，可以提前返回
for (int i = startIndex; i <= n - (k - path.size()) + 1; i++) {
    // ... 正常逻辑
}
```

---

## 8. 复杂度总结

| 问题类型 | 时间复杂度 | 空间复杂度 |
|---------|-----------|-----------|
| 组合 | O(C(n,k) × k) | O(k) |
| 排列 | O(n! × n) | O(n) |
| 子集 | O(n × 2^n) | O(n) |
| 分割回文串 | O(n × 2^n) | O(n) |
| N 皇后 | O(n!) | O(n) |

**说明**：
- 回溯的本质是穷举，时间复杂度一般较高
- 空间复杂度主要来自递归调用栈和存储路径的集合

---

## 9. 回溯 vs DFS vs DP

| 特性 | 回溯 | DFS | DP |
|------|------|-----|----|
| 本质 | 暴力穷举 | 图遍历 | 优化重复计算 |
| 返回时 | 撤销状态 | 无需撤销 | 无 |
| 适用 | 求所有方案 | 路径问题 | 最优化问题 |
| 复杂度 | 指数级 | 线性 | 多项式 |

---

## 10. 常见陷阱

| 陷阱 | 说明 | 解决方案 |
|------|------|----------|
| 忘记回溯 | 路径没有撤销 | 在递归调用后撤销选择 |
| 去重逻辑错误 | 组合去重和排列去重不同 | 组合用 startIndex，排列用 used 数组 |
| 深拷贝 vs 浅拷贝 | 收集结果时引用被修改 | 使用 new ArrayList<>(path) 或 path.copy() |
| 剪枝位置错误 | 提前剪枝导致遗漏正确答案 | 确保剪枝不会影响正确结果 |
| 终止条件 | 忘记处理或处理错误 | 明确什么情况下应该收集结果 |
| 重复元素处理 | 相同元素在同一层被重复使用 | 排序 + 跳过同一层相同元素 |
