# 二叉树代码模板

二叉树是力扣中最常见的数据结构之一。

---

## 1. 树节点定义

### 1.1 节点结构

```
二叉树节点:
┌──────┬─────┬─────┐
│ val │left │right│
└──────┴─────┴─────┘

示例二叉树:
      1
     / \
    2   3
   / \
  4   5
```

#### Java
```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```

#### Python
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

---

## 2. 树的遍历

### 2.1 前序遍历（根 -> 左 -> 右）

```
遍历顺序:
      1           访问: 1 -> 2 -> 4 -> 5 -> 3
     / \
    2   3
   / \
  4   5

递归栈过程:
preorder(1) -> visit 1
  -> preorder(2) -> visit 2
    -> preorder(4) -> visit 4
    -> preorder(5) -> visit 5
  -> preorder(3) -> visit 3
```

#### Java
```java
// 递归
public void preorder(TreeNode root) {
    if (root == null) return;
    result.add(root.val);      // 根
    preorder(root.left);        // 左
    preorder(root.right);       // 右
}

// 迭代
public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Deque<TreeNode> stack = new ArrayDeque<>();
    stack.push(root);

    while (!stack.isEmpty()) {
        TreeNode node = stack.pop();
        if (node != null) {
            result.add(node.val);
            stack.push(node.right);  // 右先入栈（后处理）
            stack.push(node.left);   // 左后入栈（先处理）
        }
    }
    return result;
}
```

#### Python
```python
# 递归
def preorder(root):
    if not root:
        return
    result.append(root.val)  # 根
    preorder(root.left)      # 左
    preorder(root.right)     # 右

# 迭代
def preorder_traversal(root):
    result = []
    stack = [root]

    while stack:
        node = stack.pop()
        if node:
            result.append(node.val)
            stack.append(node.right)
            stack.append(node.left)
    return result
```

---

### 2.2 中序遍历（左 -> 根 -> 右）

```
遍历顺序:
      1           访问: 4 -> 2 -> 5 -> 1 -> 3
     / \
    2   3
   / \
  4   5

特点: BST 的中序遍历是递增序列
```

#### Java
```java
// 递归
public void inorder(TreeNode root) {
    if (root == null) return;
    inorder(root.left);
    result.add(root.val);      // 根
    inorder(root.right);
}

// 迭代
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

        curr = stack.pop();
        result.add(curr.val);
        curr = curr.right;  // 转向右子树
    }
    return result;
}
```

#### Python
```python
# 递归
def inorder(root):
    if not root:
        return
    inorder(root.left)
    result.append(root.val)
    inorder(root.right)

# 迭代
def inorder_traversal(root):
    result = []
    stack = []
    curr = root

    while curr or stack:
        while curr:
            stack.append(curr)
            curr = curr.left

        curr = stack.pop()
        result.append(curr.val)
        curr = curr.right

    return result
```

---

### 2.3 层序遍历（BFS）

```
遍历顺序:
      1           层序: [1] -> [2,3] -> [4,5]
     / \
    2   3
   / \
  4   5

队列过程:
queue: [1]      -> 层1: [1]
      -> [2,3]   -> 层2: [2,3]
      -> [4,5]   -> 层3: [4,5]
```

#### Java
```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);

    while (!queue.isEmpty()) {
        int levelSize = queue.size();
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
```

#### Python
```python
from collections import deque

def level_order(root):
    if not root:
        return []

    result = []
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
```

---

## 3. 树的属性

### 3.1 最大深度

```
      1           深度: 3
     / \
    2   3
   / \
  4   5

递归思路:
depth(1) = 1 + max(depth(2), depth(3))
depth(2) = 1 + max(depth(4), depth(5))
depth(4) = 1
depth(5) = 1
```

#### Java
```java
public int maxDepth(TreeNode root) {
    if (root == null) return 0;

    int leftDepth = maxDepth(root.left);
    int rightDepth = maxDepth(root.right);

    return Math.max(leftDepth, rightDepth) + 1;
}
```

#### Python
```python
def max_depth(root):
    if not root:
        return 0

    left_depth = max_depth(root.left)
    right_depth = max_depth(root.right)

    return max(left_depth, right_depth) + 1
```

---

## 4. BST（二叉搜索树）

### 4.1 验证 BST

```
BST 特点: 左子树 < 根 < 右子树

有效 BST:      5
             / \
            3   8
           / \   \
          1   4   9

无效 BST:      5
             / \
            6   8    (6 > 5，不满足)
```

#### Java
```java
public boolean isValidBST(TreeNode root) {
    return validate(root, null, null);
}

private boolean validate(TreeNode node, Integer min, Integer max) {
    if (node == null) return true;

    if (min != null && node.val <= min) return false;
    if (max != null && node.val >= max) return false;

    return validate(node.left, min, node.val) &&
           validate(node.right, node.val, max);
}
```

#### Python
```python
def is_valid_bst(root):
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
```

---

## 5. 复杂度总结

| 操作 | 时间复杂度 | 空间复杂度 |
|------|-----------|-----------|
| 前中后序遍历（递归） | O(n) | O(h) |
| 层序遍历 | O(n) | O(w) |
| 树的深度 | O(n) | O(h) |
| 验证 BST | O(n) | O(h) |

**说明**：h 为树高，平衡树 h = log n，最坏 h = n；w 为树的最大宽度
