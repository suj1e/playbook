# 98. Validate Binary Search Tree / 验证二叉搜索树

[中文链接](https://leetcode.cn/problems/validate-binary-search-tree/) | [English](https://leetcode.com/problems/validate-binary-search-tree/)

**难度**：Medium
**标签**：树 | 深度优先搜索 | 二叉搜索树 | 二叉树
**相似题目**：[lc-94](../01-traversal/lc-94-binary-tree-inorder-traversal.md) | [lc-700](./) | [lc-701](./)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

给你一个二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。

**有效** 二叉搜索树定义如下：
- 节点的左子树只包含 **小于** 当前节点的数。
- 节点的右子树只包含 **大于** 当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

**示例 1**：
```
输入：root = [2,1,3]
输出：true
```

**示例 2**：
```
输入：root = [5,1,4,null,null,3,6]
输出：false
解释：根节点的值是 5 ，但是右子节点的值是 4 。
```

**约束条件**：
- 树中节点数目在范围 [1, 10⁴] 内
- -2³¹ <= Node.val <= 2³¹ - 1

---

## 解题思路

### 方法一：递归（范围验证）

**思路**：
对于每个节点，检查其值是否在合法范围内 (min, max)。

**复杂度**：
- 时间：O(n)
- 空间：O(h)

**步骤**：
1. 根节点范围是 (-∞, +∞)
2. 左子节点范围是 (min, node.val)
3. 右子节点范围是 (node.val, max)

---

### 方法二：中序遍历

**思路**：
BST 的中序遍历是递增序列，检查是否严格递增。

**复杂度**：
- 时间：O(n)
- 空间：O(h)

**步骤**：
1. 中序遍历
2. 记录前一个节点值
3. 当前值必须 > 前一个值

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
    // 方法一：递归范围验证
    public boolean isValidBST(TreeNode root) {
        return validate(root, null, null);
    }

    private boolean validate(TreeNode node, Integer min, Integer max) {
        if (node == null) return true;

        // 检查当前节点值是否在范围内
        if (min != null && node.val <= min) return false;
        if (max != null && node.val >= max) return false;

        // 递归检查子树
        return validate(node.left, min, node.val) &&
               validate(node.right, node.val, max);
    }

    // 方法二：中序遍历
    private Integer prev = null;

    public boolean isValidBST(TreeNode root) {
        return inorder(root);
    }

    private boolean inorder(TreeNode node) {
        if (node == null) return true;

        // 遍历左子树
        if (!inorder(node.left)) return false;

        // 检查当前节点
        if (prev != null && node.val <= prev) return false;
        prev = node.val;

        // 遍历右子树
        return inorder(node.right);
    }
}
```

### Python

```python
from typing import Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    # 方法一：递归范围验证
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        def validate(node, min_val, max_val):
            if not node:
                return True

            if min_val is not None and node.val <= min_val:
                return False
            if max_val is not None and node.val >= max_val:
                return False

            return validate(node.left, min_val, node.val) and \
                   validate(node.right, node.val, max_val)

        return validate(root, None, None)

    # 方法二：中序遍历
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        prev = None

        def inorder(node):
            nonlocal prev
            if not node:
                return True

            if not inorder(node.left):
                return False

            if prev is not None and node.val <= prev:
                return False
            prev = node.val

            return inorder(node.right)

        return inorder(root)
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| BST 定义 | 左 < 根 < 右，必须严格小于/大于 |
| 传递范围 | 父节点的值成为子节点的边界 |
| 中序特性 | BST 中序遍历严格递增 |
| Integer null | Java 用 null 表示无边界 |
| 严格递增 | 必须是 > 和 <，不能有等于 |

---

## 图解

```
有效 BST:
      5
     / \
    3   7
   / \ / \
  2  4 6  8

validate(5, -∞, +∞): 5 ∈ (-∞, +∞) ✓
  validate(3, -∞, 5): 3 ∈ (-∞, 5) ✓
    validate(2, -∞, 3): 2 ∈ (-∞, 3) ✓
    validate(4, 3, 5): 4 ∈ (3, 5) ✓
  validate(7, 5, +∞): 7 ∈ (5, +∞) ✓
    validate(6, 5, 7): 6 ∈ (5, 7) ✓
    validate(8, 7, +∞): 8 ∈ (7, +∞) ✓

无效 BST:
      5
     / \
    1   4
       / \
      3   6

validate(5, -∞, +∞): 5 ∈ (-∞, +∞) ✓
  validate(1, -∞, 5): 1 ∈ (-∞, 5) ✓
  validate(4, 5, +∞): 4 ∉ (5, +∞) ✗
  返回 false
```

---

## 变种/延伸

### 变种1：二叉搜索树中搜索
- **题目**：[lc-700 二叉搜索树中的搜索](./)
- **变化**：在 BST 中查找目标值
- **解法**：根据 BST 特性单向搜索

### 变种2：二叉搜索树的插入操作
- **题目**：[lc-701 二叉搜索树中的插入操作](./)
- **变化**：向 BST 插入值
- **解法**：找到合适位置插入

### 变种3：删除二叉搜索树中的节点
- **题目**：[lc-450 删除二叉搜索树中的节点](./)
- **变化**：删除节点并保持 BST 性质
- **解法**：分三种情况处理

### 进阶思考

1. **为什么必须严格小于/大于？**
   - BST 定义要求左 < 根 < 右
   - 如果允许等于，就不是标准 BST

2. **如何处理整数边界溢出？**
   - Java 用 Integer/Long 表示无边界
   - Python 用 None 表示无边界

3. **中序遍历方法的优缺点？**
   - 优点：直观，利用 BST 特性
   - 缺点：需要全局变量或类成员

4. **如何验证 N 叉搜索树？**
   - 每个子树有对应的范围
   - 第 i 个子树范围是 (parent, bound[i+1])

5. **BST 和堆的区别？**
   - BST：左 < 根 < 右，有序
   - 堆：父节点 >= (或 <=) 子节点，不保证左右顺序
