# 226. Invert Binary Tree / 翻转二叉树

[中文链接](https://leetcode.cn/problems/invert-binary-tree/) | [English](https://leetcode.com/problems/invert-binary-tree/)

**难度**：Easy
**标签**：树 | 深度优先搜索 | 广度优先搜索 | 二叉树
**相似题目**：[lc-100](./) | [lc-101](./) | [lc-572](./)
**面试频次**：⭐⭐⭐

---

## 题目描述

给你一棵二叉树的根节点 `root` ，翻转这棵二叉树，并返回其根节点。

**示例 1**：
```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

**示例 2**：
```
输入：root = [2,1,3]
输出：[2,3,1]
```

**示例 3**：
```
输入：root = []
输出：[]
```

**约束条件**：
- 树中节点数目范围在 [0, 100] 内
- -100 <= Node.val <= 100

---

## 解题思路

### 方法一：递归（DFS）

**思路**：
递归翻转每个节点的左右子树。

**复杂度**：
- 时间：O(n)
- 空间：O(h)

**步骤**：
1. 基准情况：空树直接返回
2. 递归翻转左子树
3. 递归翻转右子树
4. 交换左右子节点
5. 返回根节点

---

### 方法二：迭代（BFS）

**思路**：
使用队列按层处理，每处理一个节点就交换其左右子节点。

**复杂度**：
- 时间：O(n)
- 空间：O(n)

**步骤**：
1. 根节点入队
2. 循环处理队列：
   - 弹出节点
   - 交换其左右子节点
   - 子节点入队
3. 返回根节点

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
    // 方法一：递归
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;

        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);

        root.left = right;
        root.right = left;

        return root;
    }

    // 方法二：迭代 BFS
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();

            // 交换左右子节点
            TreeNode temp = node.left;
            node.left = node.right;
            node.right = temp;

            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }

        return root;
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
    # 方法一：递归
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None

        left = self.invertTree(root.left)
        right = self.invertTree(root.right)

        root.left = right
        root.right = left

        return root

    # 方法二：迭代 BFS
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None

        queue = deque([root])

        while queue:
            node = queue.popleft()

            # 交换左右子节点
            node.left, node.right = node.right, node.left

            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)

        return root
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 递归思路 | 先递归翻转子树，再交换 |
| 迭代思路 | 按层遍历，边遍历边交换 |
| Python 交换 | a, b = b, a 语法糖 |
| 原地操作 | 不创建新节点，只交换指针 |
| 后序遍历 | 递归本质是后序遍历 |

---

## 图解

```
原树:
      4
     / \
    2   7
   / \ / \
  1  3 6  9

翻转后:
      4
     / \
    7   2
   / \ / \
  9  6 3  1

递归过程:
invertTree(4)
  -> invertTree(2): 返回 2 (左右子树已翻转)
     -> invertTree(1): 返回 1
     -> invertTree(3): 返回 3
     -> 交换 1 <-> 3
  -> invertTree(7): 返回 7 (左右子树已翻转)
     -> invertTree(6): 返回 6
     -> invertTree(9): 返回 9
     -> 交换 6 <-> 9
  -> 交换 2 <-> 7
```

---

## 变种/延伸

### 变种1：相同的树
- **题目**：[lc-100 相同的树](./)
- **变化**：判断两棵树是否相同
- **解法**：同步遍历比较

### 变种2：对称二叉树
- **题目**：[lc-101 对称二叉树](./)
- **变化**：判断是否轴对称
- **解法**：递归比较镜像位置

### 变种3：另一棵树的子树
- **题目**：[lc-572 另一棵树的子树](./)
- **变化**：判断一棵树是否是另一棵树的子树
- **解法**：DFS + 相同树判断

### 进阶思考

1. **翻转操作是否可逆？**
   - 可以，翻转两次恢复原样
   - invertTree(invertTree(root)) == root

2. **如何判断翻转是否正确？**
   - 层序遍历，检查每层是否对称
   - 或递归检查镜像关系

3. **空间复杂度能优化吗？**
   - 递归需要 O(h) 栈空间
   - 迭代需要 O(w) 队列空间
   - 都不能优化到 O(1)

4. **Morris 遍历能实现 O(1) 空间吗？**
   - 理论上可以
   - 但实现复杂，实际意义不大

5. **翻转二叉树的应用场景？**
   - 镜像二叉树判断
   - 树的对称性分析
   - 某些树操作的逆操作
