# 236. Lowest Common Ancestor of a Binary Tree / 二叉树的最近公共祖先

[中文链接](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/) | [English](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

**难度**：Medium
**标签**：树 | 深度优先搜索 | 二叉树
**相似题目**：[lc-235](./) | [lc-1650](./) | [lc-1644](./)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

给定一个二叉树, 找到该树中两个指定节点 `p` 和 `q` 的最近公共祖先。

**最近公共祖先**的定义：
- 对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x
- 满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）

**示例 1**：
```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。
```

**示例 2**：
```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出：5
解释：节点 5 和节点 4 的 recent 公共祖先是节点 5 。
因为根据定义最近公共祖先节点可以为节点本身。
```

**约束条件**：
- 树中节点数目在范围 [2, 10⁵] 内
- -10⁹ <= Node.val <= 10⁹
- 所有 Node.val 互不相同
- p != q
- p 和 q 均存在于给定的二叉树中

---

## 解题思路

### 方法一：递归（推荐）

**思路**：
后序遍历，自底向上查找 LCA。

**核心思想**：
1. 如果当前节点是 p 或 q，直接返回当前节点
2. 在左子树找 p、q 的 LCA
3. 在右子树找 p、q 的 LCA
4. 如果左右都找到，当前节点就是 LCA
5. 如果只找到一边，返回那边的结果

**复杂度**：
- 时间：O(n)
- 空间：O(h)

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
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 基准情况
        if (root == null || root == p || root == q) {
            return root;
        }

        // 在左右子树中查找
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        // 左右都找到了，当前节点就是 LCA
        if (left != null && right != null) {
            return root;
        }

        // 返回非空的那边
        return left != null ? left : right;
    }
}
```

### Python

```python
from typing import Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        # 基准情况
        if not root or root == p or root == q:
            return root

        # 在左右子树中查找
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        # 左右都找到了，当前节点就是 LCA
        if left and right:
            return root

        # 返回非空的那边
        return left if left else right
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 后序遍历 | 先处理子树，再处理当前节点 |
| 基准情况 | 当前节点是 p 或 q 时直接返回 |
| 左右都找到 | 当前节点就是 LCA |
| 只一边找到 | 返回那边的查找结果 |
| 自底向上 | 从叶子节点向上查找 |

---

## 图解

```
树结构:
          3
       /     \
      5       1
     / \     / \
    6   2   0   8
       / \
      7   4

示例 1: p = 5, q = 1

LCA(3, 5, 1):
  left = LCA(5, 5, 1) = 5  (找到 p，返回 5)
  right = LCA(1, 5, 1) = 1 (找到 q，返回 1)
  左右都非空，返回 3

示例 2: p = 5, q = 4

LCA(3, 5, 4):
  left = LCA(5, 5, 4)
    = 5 (root == p，返回 5)
  right = LCA(1, 5, 4)
    = LCA(0, 5, 4) = null
    = LCA(8, 5, 4) = null
    = null
  只有左边非空，返回 5

解释: 节点 5 是 p，且 4 在 5 的子树中，所以 5 是 LCA
```

---

## 变种/延伸

### 变种1：二叉搜索树的 LCA
- **题目**：[lc-235 二叉搜索树的最近公共祖先](./)
- **变化**：给定的是 BST
- **解法**：利用 BST 特性优化

### 变种2：两个节点的距离
- **题目**：找出两个节点之间的距离
- **解法**：先找 LCA，再计算各自到 LCA 的距离

### 变种3：LCA 的父节点表示
- **题目**：[lc-1650 两个节点的最近公共祖先 III](./)
- **变化**：给定父节点指针而非根节点
- **解法**：使用哈希表存储祖先路径

### 进阶思考

1. **为什么是后序遍历？**
   - 需要自底向上查找
   - 后序遍历先处理子树，再处理当前节点

2. **BST 的 LCA 如何优化？**
   - 利用 BST 特性
   - p、q 在左子树 -> 往左
   - p、q 在右子树 -> 往右
   - 分别在左右 -> 当前节点就是 LCA

3. **如果多次查询 LCA，如何优化？**
   - 可以用 Tarjan 算法
   - 或倍增法预处理
   - 将单次 O(n) 优化到 O(1) 查询

4. **如何找到 LCA 到 p、q 的路径？**
   - 找到 LCA 后
   - 分别从 LCA 搜索 p 和 q
   - 记录路径上的节点

5. **N 叉树的 LCA 怎么办？**
   - 思路相同
   - 遍历所有子树找 p、q
   - 如果 p、q 分布在不同子树，当前节点就是 LCA
