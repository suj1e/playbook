# 94. Binary Tree Inorder Traversal / 二叉树的中序遍历

[中文链接](https://leetcode.cn/problems/binary-tree-inorder-traversal/) | [English](https://leetcode.com/problems/binary-tree-inorder-traversal/)

**难度**：Easy
**标签**：栈 | 树 | 深度优先搜索
**相似题目**：[lc-144](./lc-144-binary-tree-preorder-traversal.md) | [lc-145](./lc-145-binary-tree-postorder-traversal.md) | [lc-98](../03-bst/lc-98-validate-binary-search-tree.md)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给定一个二叉树的根节点 `root` ，返回它的 **中序遍历** 。

**示例 1**：
```
输入：root = [1,null,2,3]
输出：[1,3,2]
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
按照 "左 -> 根 -> 右" 的顺序递归遍历。

**复杂度**：
- 时间：O(n)
- 空间：O(h)

**步骤**：
1. 递归遍历左子树
2. 访问根节点
3. 递归遍历右子树

---

### 方法二：迭代（栈）

**思路**：
一路向左，将路径节点入栈；到达最左后，弹出访问，转向右子树。

**复杂度**：
- 时间：O(n)
- 空间：O(h)

**步骤**：
1. 从根节点开始，一路向左入栈
2. 弹出栈顶，访问
3. 转向右子树，重复步骤 1

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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        inorder(root, result);
        return result;
    }

    private void inorder(TreeNode node, List<Integer> result) {
        if (node == null) return;

        inorder(node.left, result);   // 左
        result.add(node.val);         // 根
        inorder(node.right, result);  // 右
    }

    // 方法二：迭代
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode curr = root;

        while (curr != null || !stack.isEmpty()) {
            // 一路向左，将路径节点入栈
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }

            // 弹出并访问
            curr = stack.pop();
            result.add(curr.val);

            // 转向右子树
            curr = curr.right;
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
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        result = []

        def inorder(node):
            if not node:
                return

            inorder(node.left)        # 左
            result.append(node.val)   # 根
            inorder(node.right)       # 右

        inorder(root)
        return result

    # 方法二：迭代
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        result = []
        stack = []
        curr = root

        while curr or stack:
            # 一路向左
            while curr:
                stack.append(curr)
                curr = curr.left

            # 弹出并访问
            curr = stack.pop()
            result.append(curr.val)

            # 转向右子树
            curr = curr.right

        return result
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 遍历顺序 | 左 -> 根 -> 右 |
| 迭代核心 | 一路向左入栈，弹出后转向右子树 |
| BST 特性 | BST 的中序遍历是递增序列 |
| 双重 while | 外 while 控制整体，内 while 一路向左 |
| 转向时机 | 访问节点后，转向其右子树 |

---

## 图解

```
二叉树:
      1
     / \
    2   3
   / \
  4   5

中序遍历: [4, 2, 5, 1, 3]

递归过程:
inorder(1)
  -> inorder(2)
    -> inorder(4) -> visit 4
    -> visit 2
    -> inorder(5) -> visit 5
  -> visit 1
  -> inorder(3) -> visit 3

迭代过程（栈）:
初始: curr = 1, stack = []

内循环一路向左:
  push 1, curr = 2
  push 2, curr = 4
  push 4, curr = null

stack = [1, 2, 4]

pop 4, visit 4
curr = 4.right = null

stack = [1, 2]

pop 2, visit 2
curr = 2.right = 5

内循环一路向左:
  push 5, curr = null

stack = [1, 5]

pop 5, visit 5
curr = 5.right = null

stack = [1]

pop 1, visit 1
curr = 1.right = 3

内循环一路向左:
  push 3, curr = null

stack = [3]

pop 3, visit 3
curr = 3.right = null

stack = []

结果: [4, 2, 5, 1, 3]
```

---

## 变种/延伸

### 变种1：验证二叉搜索树
- **题目**：[lc-98 验证二叉搜索树](../03-bst/lc-98-validate-binary-search-tree.md)
- **变化**：判断是否为 BST
- **解法**：中序遍历检查是否递增

### 变种2：二叉搜索树中第K小的元素
- **题目**：[lc-230 二叉搜索树中第K小的元素](../03-bst/lc-230-kth-smallest-element-in-a-bst.md)
- **变化**：找第 k 小的元素
- **解法**：中序遍历，第 k 个就是答案

### 变种3：恢复二叉搜索树
- **题目**：[lc-99 恢复二叉搜索树](./)
- **变化**：BST 中两个节点被交换，恢复它们
- **解法**：中序遍历找异常点

### 进阶思考

1. **为什么 BST 的中序遍历是递增的？**
   - BST 定义：左 < 根 < 右
   - 中序遍历：左 -> 根 -> 右
   - 正好是从小到大的顺序

2. **如何用中序遍历验证 BST？**
   - 遍历时记录前一个节点值
   - 当前值必须 > 前一个值

3. **迭代法中为什么是一路向左？**
   - 中序遍历先访问左子树
   - 一路向左找到最左节点（第一个访问的）

4. **如何用 O(1) 空间实现中序遍历？**
   - Morris 遍历
   - 利用前驱节点的 right 指针

5. **中序遍历有哪些应用？**
   - BST 排序输出
   - 验证 BST
   - 找 BST 中第 k 小的元素
