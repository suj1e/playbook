# 144. Binary Tree Preorder Traversal / 二叉树的前序遍历

[中文链接](https://leetcode.cn/problems/binary-tree-preorder-traversal/) | [English](https://leetcode.com/problems/binary-tree-preorder-traversal/)

**难度**：Easy
**标签**：栈 | 树 | 深度优先搜索
**相似题目**：[lc-94](./lc-94-binary-tree-inorder-traversal.md) | [lc-145](./lc-145-binary-tree-postorder-traversal.md) | [lc-589](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给你二叉树的根节点 `root` ，返回它节点值的 **前序遍历** 。

**示例 1**：
```
输入：root = [1,null,2,3]
输出：[1,2,3]
```

**示例 2**：
```
输入：root = []
输出：[]
```

**示例 3**：
```
输入：root = [1]
输出：[1]
```

**约束条件**：
- 树中节点数目在范围 [0, 100] 内
- -100 <= Node.val <= 100

**进阶**：递归算法很简单，你可以通过迭代算法完成吗？

---

## 解题思路

### 方法一：递归

**思路**：
按照 "根 -> 左 -> 右" 的顺序递归遍历。

**复杂度**：
- 时间：O(n)
- 空间：O(h)，h 为树高

**步骤**：
1. 访问根节点
2. 递归遍历左子树
3. 递归遍历右子树

---

### 方法二：迭代（栈）

**思路**：
用栈模拟递归过程。

**复杂度**：
- 时间：O(n)
- 空间：O(h)

**步骤**：
1. 根节点入栈
2. 循环处理栈：
   - 弹出节点，访问
   - 右子节点入栈（先处理）
   - 左子节点入栈（后处理）

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
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        preorder(root, result);
        return result;
    }

    private void preorder(TreeNode node, List<Integer> result) {
        if (node == null) return;

        result.add(node.val);      // 根
        preorder(node.left, result);  // 左
        preorder(node.right, result); // 右
    }

    // 方法二：迭代
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;

        Deque<TreeNode> stack = new ArrayDeque<>();
        stack.push(root);

        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            result.add(node.val);  // 访问

            // 右子节点先入栈（后处理）
            if (node.right != null) {
                stack.push(node.right);
            }
            // 左子节点后入栈（先处理）
            if (node.left != null) {
                stack.push(node.left);
            }
        }

        return result;
    }
}
```

### Python

```python
from typing import List, Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    # 方法一：递归
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        result = []

        def preorder(node):
            if not node:
                return

            result.append(node.val)   # 根
            preorder(node.left)        # 左
            preorder(node.right)       # 右

        preorder(root)
        return result

    # 方法二：迭代
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        result = []
        if not root:
            return result

        stack = [root]

        while stack:
            node = stack.pop()
            result.append(node.val)  # 访问

            # 右子节点先入栈
            if node.right:
                stack.append(node.right)
            # 左子节点后入栈
            if node.left:
                stack.append(node.left)

        return result
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 遍历顺序 | 根 -> 左 -> 右 |
| 递归基准 | node == null 时返回 |
| 迭代栈 | 右子节点先入栈，左子节点后入栈 |
| 时间复杂度 | 每个节点访问一次，O(n) |
| 空间复杂度 | 栈深度最坏为 n，平均为 log n |

---

## 图解

```
二叉树:
      1
     / \
    2   3
   / \
  4   5

前序遍历: [1, 2, 4, 5, 3]

递归过程:
preorder(1) -> visit 1
  -> preorder(2) -> visit 2
    -> preorder(4) -> visit 4
    -> preorder(5) -> visit 5
  -> preorder(3) -> visit 3

迭代过程（栈）:
初始: stack = [1]

pop 1, visit 1, push 3, push 2
stack = [3, 2]

pop 2, visit 2, push 5, push 4
stack = [3, 5, 4]

pop 4, visit 4, 无子节点
stack = [3, 5]

pop 5, visit 5, 无子节点
stack = [3]

pop 3, visit 3, 无子节点
stack = []

结果: [1, 2, 4, 5, 3]
```

---

## 变种/延伸

### 变种1：中序遍历
- **题目**：[lc-94 二叉树的中序遍历](./lc-94-binary-tree-inorder-traversal.md)
- **变化**：左 -> 根 -> 右

### 变种2：后序遍历
- **题目**：[lc-145 二叉树的后序遍历](./lc-145-binary-tree-postorder-traversal.md)
- **变化**：左 -> 右 -> 根

### 变种3：N 叉树的前序遍历
- **题目**：[lc-589 N 叉树的前序遍历](./)
- **变化**：每个节点有多个子节点
- **解法**：类似，只是循环处理所有子节点

### 变种4：Morris 遍历
- **变化**：O(1) 空间复杂度的遍历
- **解法**：利用线索二叉树

### 进阶思考

1. **前序、中序、后序的区别？**
   - 前序：根节点最先访问
   - 中序：根节点中间访问
   - 后序：根节点最后访问

2. **如何根据前序和中序重建二叉树？**
   - 前序第一个是根节点
   - 在中序中找到根节点，左边是左子树，右边是右子树
   - 递归构建

3. **迭代法中为什么右子节点先入栈？**
   - 栈是后进先出
   - 要先处理左子节点，所以左子节点后入栈

4. **如何实现 Morris 前序遍历？**
   - O(1) 空间
   - 利用前驱节点的 right 指针
   - 比较复杂，面试一般不要求

5. **前序遍历有哪些应用？**
   - 打印树的结构
   - 复制二叉树
   - 获取前缀表达式
