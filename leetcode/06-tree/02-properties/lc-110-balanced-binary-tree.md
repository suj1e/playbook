# 110. Balanced Binary Tree / 平衡二叉树

[中文链接](https://leetcode.cn/problems/balanced-binary-tree/) | [English](https://leetcode.com/problems/balanced-binary-tree/)

**难度**：Easy
**标签**：树 | 深度优先搜索 | 二叉树
**相似题目**：[lc-104](./lc-104-maximum-depth-of-binary-tree.md) | [lc-543](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

> 一个二叉树每个节点的左右两个子树的高度差的绝对值不超过 1 。

**示例 1**：
```
输入：root = [3,9,20,null,null,15,7]
输出：true
```

**示例 2**：
```
输入：root = [1,2,2,3,3,null,null,4,4]
输出：false
```

**示例 3**：
```
输入：root = []
输出：true
```

**约束条件**：
- 树中的节点数在范围 [0, 5000] 内
- -10⁴ <= Node.val <= 10⁴

---

## 解题思路

### 方法一：自顶向下（朴素）

**思路**：
对每个节点，计算左右子树高度，判断高度差是否 <= 1。

**复杂度**：
- 时间：O(n²) - 每个节点都要计算高度
- 空间：O(n) - 递归栈

### 方法二：自底向上（推荐）

**思路**：
后序遍历，返回每个节点的深度。如果发现不平衡，返回 -1 作为标记。

**核心思想**：
- 后序遍历：左 → 右 → 根
- 如果左或右子树不平衡（返回 -1），直接返回 -1
- 如果左右高度差 > 1，返回 -1
- 否则返回当前节点高度

**复杂度**：
- 时间：O(n) - 每个节点只访问一次
- 空间：O(n) - 递归栈

---

## 代码实现

### Java - 自顶向下

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if (root == null) return true;

        int leftHeight = height(root.left);
        int rightHeight = height(root.right);

        // 当前节点平衡 && 左子树平衡 && 右子树平衡
        return Math.abs(leftHeight - rightHeight) <= 1
            && isBalanced(root.left)
            && isBalanced(root.right);
    }

    private int height(TreeNode node) {
        if (node == null) return 0;
        return 1 + Math.max(height(node.left), height(node.right));
    }
}
```

### Java - 自底向上（最优）

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return height(root) != -1;
    }

    // 返回高度，不平衡返回 -1
    private int height(TreeNode node) {
        if (node == null) return 0;

        int leftHeight = height(node.left);
        if (leftHeight == -1) return -1;  // 左子树不平衡

        int rightHeight = height(node.right);
        if (rightHeight == -1) return -1;  // 右子树不平衡

        // 当前节点不平衡
        if (Math.abs(leftHeight - rightHeight) > 1) {
            return -1;
        }

        return 1 + Math.max(leftHeight, rightHeight);
    }
}
```

### Python - 自底向上

```python
from typing import Optional

class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        return self.height(root) != -1

    def height(self, node: Optional[TreeNode]) -> int:
        if node is None:
            return 0

        left_height = self.height(node.left)
        if left_height == -1:
            return -1

        right_height = self.height(node.right)
        if right_height == -1:
            return -1

        if abs(left_height - right_height) > 1:
            return -1

        return 1 + max(left_height, right_height)
```

### Python - 自顶向下

```python
from typing import Optional

class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        if root is None:
            return True

        left_height = self.height(root.left)
        right_height = self.height(root.right)

        return (abs(left_height - right_height) <= 1
                and self.isBalanced(root.left)
                and self.isBalanced(root.right))

    def height(self, node: Optional[TreeNode]) -> int:
        if node is None:
            return 0
        return 1 + max(self.height(node.left), self.height(node.right))
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 平衡定义 | 左右高度差 <= 1 |
| 自顶向下 | 对每个节点递归检查，有重复计算 |
| 自底向上 | 后序遍历，返回 -1 表示不平衡 |
| 剪枝 | 发现 -1 立即返回，不继续计算 |
| 时间优化 | O(n²) → O(n) |

---

## 图解

```
示例 1: 平衡二叉树

      3         高度差 |2-1| = 1 ✓
     / \
    9   20      左9高度=1, 右20高度=2
        / \
       15  7     左15高度=1, 右7高度=1
                高度差 |1-1| = 0 ✓

所有节点高度差都 <= 1，是平衡二叉树

---

示例 2: 非平衡二叉树

        1           高度差 |1-1| = 0 ✓
       / \
      2   2         左2高度=2, 右2高度=2
     / \
    3   3           左3高度=1, 右3高度=1
   / \
  4   4             左4高度=1, 右4高度=1
                    高度差 |1-1| = 0 ✓

但是！节点3的左子树（包含4）高度=2
节点3的右子树（null）高度=0
高度差 = |2-0| = 2 > 1 ✗

所以不是平衡二叉树

---

自底向上过程:

      1
     / \
    2   2
   / \
  3   3
 / \
4   4

后序遍历:
height(4) = 1
height(4) = 1
height(3): 左1, 右0, 差1, 返回2 ✓
height(3): 左0, 右0, 差0, 返回1 ✓
height(2): 左2, 右1, 差1, 返回3 ✗ (|2-1|=1<=1, 但注意左子树高度是2)
          实际上 3节点的左是4(高度1)，右是null(高度0)
          差是1，没问题
          但要继续往上检查

实际上问题在节点3的子节点4:
4的左null高度0, 右null高度0, 差0, 返回1 ✓
然后回到3:
3的左(通过4)高度1, 右null高度0, 差1, 返回2 ✓
回到2:
2的左(通过3)高度2, 右3高度1, 差1, 返回3 ✓
回到1:
1的左(通过2)高度3, 右2高度1, 差2 > 1, 返回-1 ✗

发现问题，返回false
```

---

## 变种/延伸

### 变种1：二叉树的最大深度
- **题目**：[lc-104 二叉树的最大深度](./lc-104-maximum-depth-of-binary-tree.md)
- **变化**：只计算深度，不判断平衡
- **解法**：简单的递归

### 变种2：二叉树的直径
- **题目**：[lc-543 二叉树的直径](./)
- **变化**：求任意两节点的最长路径
- **解法**：类似，需要记录最大路径

### 进阶思考

1. **为什么自底向上更高效？**
   - 自顶向下：每个节点都计算一次高度，重复计算
   - 自底向上：每个节点只计算一次，O(n)

2. **-1 作为标记的技巧？**
   - 正常高度 >= 0
   - 用 -1 表示"不平衡"
   - 一旦发现 -1，立即向上传播

3. **空树是平衡的吗？**
   - 是的，空树是平衡的
   - 高度为 0，没有左右子树

4. **如何判断 AVL 树？**
   - AVL 树就是平衡二叉搜索树
   - 需要同时满足 BST 和平衡条件

5. **与完全二叉树的区别？**
   - 平衡：左右高度差 <= 1
   - 完全：只有最底层可能不满，且左对齐

6. **实际应用？**
   - AVL 树、红黑树的基础
   - 保证查找操作的 O(log n) 复杂度
