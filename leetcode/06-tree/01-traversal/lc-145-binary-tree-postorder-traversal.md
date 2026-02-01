# 145. Binary Tree Postorder Traversal / 二叉树的后序遍历

[中文链接](https://leetcode.cn/problems/binary-tree-postorder-traversal/) | [English](https://leetcode.com/problems/binary-tree-postorder-traversal/)

**难度**：Easy
**标签**：栈 | 树 | 深度优先搜索
**相似题目**：[lc-94](./lc-94-binary-tree-inorder-traversal.md) | [lc-144](./lc-144-binary-tree-preorder-traversal.md)
**面试频次**：⭐⭐⭐⭐⭐

---

## 题目描述

给你一棵二叉树的根节点 `root` ，返回其节点值的 **后序遍历** 。

**示例 1**：
```
输入：root = [1,null,2,3]
输出：[3,2,1]
```

**示例 2**：
```
输入：root = [1,2,3,4,5,null,8,null,null,6,7,9]
输出：[4,6,7,5,2,9,8,3,1]
```

**示例 3**：
```
输入：root = []
输出：[]
```

**示例 4**：
```
输入：root = [1]
输出：[1]
```

**约束条件**：
- 树中节点数目在范围 [0, 100] 内
- -100 <= Node.val <= 100

---

## 解题思路

### 方法一：递归（推荐）

**思路**：
后序遍历：左子树 → 右子树 → 根节点。

**复杂度**：
- 时间：O(n)
- 空间：O(n) - 递归栈空间

### 方法二：迭代

**思路**：
使用栈模拟递归。后序遍历的迭代实现比较特殊，需要记录上一个访问的节点。

**核心思想**：
- 从根节点开始
- 如果有左子树，一直往左走
- 如果没有左子树或左子树已访问，检查右子树
- 如果右子树也访问了，则处理当前节点

**复杂度**：
- 时间：O(n)
- 空间：O(n)

### 方法三：反转法

**思路**：
前序遍历是"根-左-右"，将其改为"根-右-左"，然后反转结果就是后序遍历"左-右-根"。

**复杂度**：
- 时间：O(n)
- 空间：O(n)

---

## 代码实现

### Java - 递归

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        postorder(root, result);
        return result;
    }

    private void postorder(TreeNode node, List<Integer> result) {
        if (node == null) return;

        postorder(node.left, result);   // 左
        postorder(node.right, result);  // 右
        result.add(node.val);           // 根
    }
}
```

### Java - 迭代（标准版）

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;

        Deque<TreeNode> stack = new LinkedList<>();
        TreeNode prev = null;  // 上一个访问的节点
        TreeNode curr = root;

        while (curr != null || !stack.isEmpty()) {
            // 一直往左走
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }

            curr = stack.peek();

            // 如果右子树为空或已访问，则处理当前节点
            if (curr.right == null || curr.right == prev) {
                result.add(curr.val);
                stack.pop();
                prev = curr;
                curr = null;  // 重要：设置为 null，避免重复处理左子树
            } else {
                // 否则处理右子树
                curr = curr.right;
            }
        }

        return result;
    }
}
```

### Java - 反转法

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        LinkedList<Integer> result = new LinkedList<>();
        if (root == null) return result;

        Deque<TreeNode> stack = new LinkedList<>();
        stack.push(root);

        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            result.addFirst(node.val);  // 头插法，实现反转

            // 先左后右，这样处理时就是先右后左
            if (node.left != null) stack.push(node.left);
            if (node.right != null) stack.push(node.right);
        }

        return result;
    }
}
```

### Python - 递归

```python
from typing import List, Optional

class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        result = []

        def postorder(node):
            if node is None:
                return

            postorder(node.left)
            postorder(node.right)
            result.append(node.val)

        postorder(root)
        return result
```

### Python - 迭代（反转法）

```python
from typing import List, Optional

class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        result = []
        if not root:
            return result

        stack = [root]

        while stack:
            node = stack.pop()
            result.append(node.val)

            if node.left:
                stack.append(node.left)
            if node.right:
                stack.append(node.right)

        return result[::-1]  # 反转
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 后序顺序 | 左 → 右 → 根 |
| 递归最简单 | 先左后右再根 |
| 迭代技巧 | 使用 prev 记录上一个访问节点 |
| 反转法 | 修改前序为"根-右-左"后反转 |
| curr = null | 迭代中重要，避免重复访问 |

---

## 图解

```
     1
    / \
   2   3
  / \
 4   5

后序遍历: 左 → 右 → 根

递归过程:
postorder(1):
  postorder(2):
    postorder(4):
      4 左子节点为 null，返回
      4 右子节点为 null，返回
      访问 4 ✓
    5 左子节点为 null，返回
    5 右子节点为 null，返回
    访问 5 ✓
    访问 2 ✓
  3 左子节点为 null，返回
  3 右子节点为 null，返回
  访问 3 ✓
  访问 1 ✓

结果: [4, 5, 2, 3, 1]

---

迭代过程（反转法）:

初始: stack = [1], result = []

第1轮:
  pop 1, result = [1]
  push 左2, push 右3
  stack = [2, 3]

第2轮:
  pop 3, result = [1, 3]
  push 左(null), push 右(null)
  stack = [2]

第3轮:
  pop 2, result = [1, 3, 2]
  push 左4, push 右5
  stack = [4, 5]

第4轮:
  pop 5, result = [1, 3, 2, 5]
  stack = [4]

第5轮:
  pop 4, result = [1, 3, 2, 5, 4]
  stack = []

反转 result: [4, 5, 2, 3, 1]
```

---

## 变种/延伸

### 变种1：前序遍历
- **题目**：[lc-144 二叉树的前序遍历](./lc-144-binary-tree-preorder-traversal.md)
- **变化**：根 → 左 → 右
- **解法**：类似，只需调整访问顺序

### 变种2：中序遍历
- **题目**：[lc-94 二叉树的中序遍历](./lc-94-binary-tree-inorder-traversal.md)
- **变化**：左 → 根 → 右
- **解法**：迭代略有不同

### 变种3：N叉树的后序遍历
- **题目**：[lc-590 N叉树的后序遍历](./)
- **变化**：N 个子节点
- **解法**：类似，递归所有子节点

### 进阶思考

1. **为什么后序遍历迭代最复杂？**
   - 需要同时考虑左右子树的访问状态
   - 不能像中序那样简单判断

2. **反转法的原理？**
   - 前序：根-左-右
   - 修改前序：根-右-左
   - 反转：左-右-根 = 后序

3. **何时用递归 vs 迭代？**
   - 递归：简洁易懂
   - 迭代：避免栈溢出，面试常问

4. **如何验证遍历正确性？**
   - 画图模拟递归过程
   - 注意左右子树的顺序

5. **Morris 遍历？**
   - O(1) 空间的遍历方法
   - 通过修改树的结构实现
   - 后序最复杂，较少使用

6. **三种遍历的应用？**
   - 前序：复制树、前缀表达式
   - 中序：BST 有序序列、中缀表达式
   - 后序：删除树、后缀表达式
