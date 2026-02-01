# 102. Binary Tree Level Order Traversal / 二叉树的层序遍历

[中文链接](https://leetcode.cn/problems/binary-tree-level-order-traversal/) | [English](https://leetcode.com/problems/binary-tree-level-order-traversal/)

**难度**：Medium
**标签**：树 | 广度优先搜索 | 二叉树
**相似题目**：[lc-103](./) | [lc-107](./) | [lc-199](./)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

给你二叉树的根节点 `root` ，返回其节点值的 **层序遍历** 。 （即逐层地，从左到右访问所有节点）。

**示例 1**：
```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]
```

**示例 2**：
```
输入：root = [1]
输出：[[1]]
```

**示例 3**：
```
输入：root = []
输出：[]
```

**约束条件**：
- 树中节点数目在范围 [0, 2000] 内
- -1000 <= Node.val <= 1000

---

## 解题思路

### 方法一：BFS（队列）

**思路**：
使用队列进行广度优先搜索，每处理一层就记录一层的结果。

**复杂度**：
- 时间：O(n)
- 空间：O(n)

**步骤**：
1. 根节点入队
2. 循环处理队列（按层处理）：
   - 记录当前层的节点数
   - 处理当前层所有节点
   - 将子节点加入队列
3. 返回结果

---

### 方法二：DFS（递归）

**思路**：
使用 DFS，用 depth 参数标记当前层数。

**复杂度**：
- 时间：O(n)
- 空间：O(h)

**步骤**：
1. 递归遍历节点，传入当前深度
2. 确保 result 数组有当前层的位置
3. 将节点值加入对应层

---

## 代码实现

### Java

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    // 方法一：BFS
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) return result;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            int levelSize = queue.size();  // 当前层的节点数
            List<Integer> currentLevel = new ArrayList<>();

            for (int i = 0; i < levelSize; i++) {
                TreeNode node = queue.poll();
                currentLevel.add(node.val);

                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }

            result.add(currentLevel);
        }

        return result;
    }

    // 方法二：DFS
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        dfs(root, 0, result);
        return result;
    }

    private void dfs(TreeNode node, int depth, List<List<Integer>> result) {
        if (node == null) return;

        // 确保 result 有当前层的位置
        if (depth == result.size()) {
            result.add(new ArrayList<>());
        }

        result.get(depth).add(node.val);

        dfs(node.left, depth + 1, result);
        dfs(node.right, depth + 1, result);
    }
}
```

### Python

```python
from typing import List, Optional
from collections import deque

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    # 方法一：BFS
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        result = []
        if not root:
            return result

        queue = deque([root])

        while queue:
            level_size = len(queue)
            current_level = []

            for _ in range(level_size):
                node = queue.popleft()
                current_level.append(node.val)

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

            result.append(current_level)

        return result

    # 方法二：DFS
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        result = []

        def dfs(node, depth):
            if not node:
                return

            # 确保有当前层
            if depth == len(result):
                result.append([])

            result[depth].append(node.val)

            dfs(node.left, depth + 1)
            dfs(node.right, depth + 1)

        dfs(root, 0)
        return result
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 按层处理 | 先记录当前层节点数，再处理 |
| levelSize | 在循环开始时记录队列大小 |
| DFS depth | 用深度参数区分不同层 |
| 逐层添加 | 每处理完一层，添加到结果 |
| 左右顺序 | 先左后右，保证同层从左到右 |

---

## 图解

```
二叉树:
      3
     / \
    9   20
       /  \
      15   7

层序遍历 BFS:

初始: queue = [3]

第1层: queue = [3]
  levelSize = 1
  弹出 3, 加入 [3]
  左子 9 入队, 右子 20 入队
  queue = [9, 20]
  result = [[3]]

第2层: queue = [9, 20]
  levelSize = 2
  弹出 9, 加入 [9], 无子节点
  弹出 20, 加入 [9, 20]
  左子 15 入队, 右子 7 入队
  queue = [15, 7]
  result = [[3], [9, 20]]

第3层: queue = [15, 7]
  levelSize = 2
  弹出 15, 加入 [15], 无子节点
  弹出 7, 加入 [15, 7], 无子节点
  queue = []
  result = [[3], [9, 20], [15, 7]]

结果: [[3], [9, 20], [15, 7]]
```

---

## 变种/延伸

### 变种1：锯齿形层序遍历
- **题目**：[lc-103 二叉树的锯齿形层序遍历](./)
- **变化**：奇数层从右到左
- **解法**：BFS + 偶数层反转

### 变种2：层序遍历 II（从下到上）
- **题目**：[lc-107 二叉树的层序遍历 II](./)
- **变化**：从下往上遍历
- **解法**：BFS + 反转结果

### 变种3：二叉树的右视图
- **题目**：[lc-199 二叉树的右视图](./)
- **变化**：每层最右边的节点
- **解法**：BFS 记录每层最后一个节点

### 变种4：在每个树行中找最大值
- **题目**：[lc-515 在每个树行中找最大值](./)
- **变化**：返回每层的最大值
- **解法**：BFS 时记录每层最大值

### 进阶思考

1. **BFS 和 DFS 的区别？**
   - BFS：按层遍历，用队列
   - DFS：深度优先，用栈/递归

2. **为什么要先记录 levelSize？**
   - 区分当前层和下一层的节点
   - 否则无法按层输出

3. **DFS 如何实现按层输出？**
   - 传入 depth 参数
   - 根据 depth 决定放入哪一层

4. **如何实现锯齿形遍历？**
   - 偶数层（0-based）反转
   - 或者在遍历时改变入队顺序

5. **层序遍历有哪些应用？**
   - 按层级打印树
   - 找最短路径（无权图）
   - 拓扑排序
   - 计算树的宽度
