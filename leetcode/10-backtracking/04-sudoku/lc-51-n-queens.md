# 51. N-Queens / N 皇后

[中文链接](https://leetcode.cn/problems/n-queens/) | [English](https://leetcode.com/problems/n-queens/)

**难度**：Hard
**标签**：数组 | 回溯 | 回溯算法
**相似题目**：[lc-52](./) | [lc-37](./) | [lc-1301](./)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

按照国际象棋的规则，皇后可以攻击与之处在同一行、同一列、同一斜线上的棋子。

n 皇后问题 研究的是如何将 `n` 个皇后放置在 `n×n` 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 `n` ，返回所有不同的 **n 皇后问题** 的解决方案。

每一种解法包含一个不同的 **n 皇后问题** 的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

**示例 1**：
```
输入：n = 4
输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
解释：4 皇后问题存在如下两个不同的解法。
[
[".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
```

**示例 2**：
```
输入：n = 1
输出：[["Q"]]
```

**约束条件**：
- 1 <= n <= 9

---

## 解题思路

### 方法一：回溯（推荐）

**思路**：
逐行放置皇后，检查每个位置是否有效。

**复杂度**：
- 时间：O(n!)
- 空间：O(n)

**步骤**：
1. 从第 0 行开始放置皇后
2. 对于每一行，尝试每一列
3. 检查位置是否有效（列、对角线无冲突）
4. 有效则放置，递归下一行
5. 回溯撤销选择

---

## 代码实现

### Java

```java
class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> result = new ArrayList<>();
        char[][] board = new char[n][n];

        // 初始化棋盘
        for (int i = 0; i < n; i++) {
            Arrays.fill(board[i], '.');
        }

        backtrack(result, board, 0, n);
        return result;
    }

    private void backtrack(List<List<String>> result, char[][] board, int row, int n) {
        // 基准情况：所有行都放置完毕
        if (row == n) {
            result.add(construct(board));
            return;
        }

        for (int col = 0; col < n; col++) {
            if (isValid(board, row, col, n)) {
                board[row][col] = 'Q';
                backtrack(result, board, row + 1, n);
                board[row][col] = '.';  // 回溯
            }
        }
    }

    // 检查位置是否有效
    private boolean isValid(char[][] board, int row, int col, int n) {
        // 检查列
        for (int i = 0; i < row; i++) {
            if (board[i][col] == 'Q') return false;
        }

        // 检查 45° 对角线（左上）
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] == 'Q') return false;
        }

        // 检查 135° 对角线（右上）
        for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (board[i][j] == 'Q') return false;
        }

        return true;
    }

    // 构建结果字符串
    private List<String> construct(char[][] board) {
        List<String> result = new ArrayList<>();
        for (char[] row : board) {
            result.add(new String(row));
        }
        return result;
    }
}
```

### Python

```python
from typing import List

class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        result = []
        board = [['.' for _ in range(n)] for _ in range(n)]

        def backtrack(row):
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

        # 检查 45° 对角线（左上）
        for i, j in zip(range(row - 1, -1, -1), range(col - 1, -1, -1)):
            if board[i][j] == 'Q':
                return False

        # 检查 135° 对角线（右上）
        for i, j in zip(range(row - 1, -1, -1), range(col + 1, n)):
            if board[i][j] == 'Q':
                return False

        return True
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 逐行放置 | 每行只放一个皇后 |
| 列检查 | 检查上方同列是否有皇后 |
| 对角线检查 | 检查两条对角线 |
| 回溯还原 | 撤销选择时恢复为 '.' |
| 时间复杂度 | O(n!) 最坏情况 |

---

## 图解

```
n = 4 的解法:

解法 1:
. Q . .
. . . Q
Q . . .
. . Q .

坐标: (0,1), (1,3), (2,0), (3,2)

解法 2:
. . Q .
Q . . .
. . . Q
. Q . .

坐标: (0,2), (1,0), (2,3), (3,1)

---

isValid 检查示例:

要放置 (row=2, col=1)

检查列: 检查 (0,1), (1,1)
检查左上对角: 检查 (1,0)
检查右上对角: 检查 (1,2)

如果有任何一个位置是 'Q'，则无效
```

---

## 变种/延伸

### 变种1：N 皇后 II
- **题目**：[lc-52 N皇后 II](./)
- **变化**：只求解法数量
- **解法**：回溯计数即可

### 变种2：N 皇后 II 的位运算优化
- **题目**：位运算优化
- **变化**：用位运算表示列和对角线
- **解法**：用整数位表示状态

### 变种3：硬板路由
- **题目**：[lc-1301 硬板路由](./)
- **变化**：更复杂的棋盘问题
- **解法**：状态压缩 DP

### 进阶思考

1. **为什么只需要检查上方？**
   - 逐行放置，下方还没有放置
   - 只需检查上方是否有冲突

2. **如何优化空间复杂度？**
   - 用一维数组表示列
   - col[i] = 第 i 行皇后所在的列

3. **位运算如何优化？**
   - 用整数位表示列、对角线状态
   - 位运算快速检查和更新

4. **如何求 N 皇后的解法数量？**
   - 回溯计数
   - 已知解：n=1(1), n=2(0), n=3(0), n=4(2), n=5(10), n=6(4), n=7(40), n=8(92)

5. **为什么 N=3 无解？**
   - 棋盘 3×3，3 个皇后
   - 每行放一个，必然冲突
   - 无法满足"互不攻击"的条件
