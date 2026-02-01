# 104. Maximum Depth of Binary Tree / 二叉树的最大深度

[中文链接](https://leetcode.cn/problems/maximum-depth-of-binary-tree/) | [English](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

**难度**：Easy
**标签**：树 | 深度优先搜索 | 广度优先搜索 | 二叉树
**相似题目**：[lc-559](./) | [lc-110](./lc-110-balanced-binary-tree.md) | [lc-111](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给定一个二叉树 `root` ，返回其最大深度。

二叉树的 **最大深度** 是指从根节点到最远叶子节点的最长路径上的节点数。

**示例 1**：
```
输入：root = [3,9,20,null,null,15,7]
输出：3
```

**示例 2**：
```
输入：root = [1,null,2]
输出：2
```

**约束条件**：
- 树中节点的数量在 [0, 10⁴] 范围内
- -100 <= Node.val <= 100

---

## 解题思路

### 方法一：DFS（递归）

**思路**：
树的深度 = max(左子树深度, 右子树深度) + 1

**复杂度**：
- 时间：O(n)
- 空间：O(h)

**步骤**：
1. 基准情况：空树深度为 0
2. 递归计算左子树深度
3. 递归计算右子树深度
4. 返回 max(left, right) + 1

---

### 方法二：BFS（层序遍历）

**思路**：
按层遍历，层数即为深度。

**复杂度**：
- 时间：O(n)
- 空间：O(n)

**步骤**：
1. 根节点入队
2. 每处理完一层，深度 +1
3. 队列为空时返回深度

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
    // 方法一：DFS 递归
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;

        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);

        return Math.max(leftDepth, rightDepth) + 1;
    }

    // 方法二：BFS 层序遍历
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int depth = 0;

        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            depth++;

            for (int i = 0; i < levelSize; i++) {
                TreeNode node = queue.poll();
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }
        }

        return depth;
    }
}
```

### Python

```python
from typing import Optional
from collections import deque

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    # 方法一：DFS 递归
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0

        left_depth = self.maxDepth(root.left)
        right_depth = self.maxDepth(root.right)

        return max(left_depth, right_depth) + 1

    # 方法二：BFS 层序遍历
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0

        queue = deque([root])
        depth = 0

        while queue:
            level_size = len(queue)
            depth += 1

            for _ in range(level_size):
                node = queue.popleft()
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

        return depth
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 递归公式 | depth = max(left, right) + 1 |
| 基准情况 | 空树深度为 0 |
| BFS 层数 | 处理完的层数就是深度 |
| DFS 栈深 | 递归栈的深度最大为树高 |
| 叶子节点 | 左右子树都为空的节点 |

---

## 图解

```
二叉树:
      3
     / \
    9   20
       /  \
      15   7

DFS 递归过程:
maxDepth(3)
  = max(maxDepth(9), maxDepth(20)) + 1

  maxDepth(9) = max(0, 0) + 1 = 1

  maxDepth(20)
    = max(maxDepth(15), maxDepth(7)) + 1
    = max(1, 1) + 1 = 2

  maxDepth(3) = max(1, 2) + 1 = 3

BFS 层序遍历:
第1层: [3], depth = 1
第2层: [9, 20], depth = 2
第3层: [15, 7], depth = 3

结果: 3
```

---

## 变种/延伸

### 变种1：平衡二叉树
- **题目**：[lc-110 平衡二叉树](./lc-110-balanced-binary-tree.md)
- **变化**：判断是否平衡（左右子树高度差 <= 1）
- **解法**：递归计算深度，检查平衡性

### 变种2：最小深度
- **题目**：[lc-111 二叉树的最小深度](./)
- **变化**：从根到最近叶子节点的最短路径
- **解法**：BFS 或 DFS（注意叶子节点定义）

### 变种3：N 叉树的最大深度
- **题目**：[lc-559 N 叉树的最大深度](./)
- **变化**：每个节点有多个子节点
- **解法**：遍历所有子节点取最大

### 进阶思考

1. **DFS 和 BFS 的空间复杂度？**
   - DFS：O(h)，h 为树高
   - BFS：O(w)，w 为树的最大宽度

2. **如何优化递归的重复计算？**
   - 本题没有重复计算
   - 如果有，可以用记忆化（哈希表）

3. **最小深度要注意什么？**
   - 叶子节点是没有子节点的节点
   - 不能只看左或右为空就返回 1

4. **如何自底向上计算深度？**
   - 后序遍历框架
   - 左右子树都处理完后返回

5. **最大深度的应用场景？**
   - 判断树是否平衡
   - 计算树的直径
   - 判断树的倾斜度
