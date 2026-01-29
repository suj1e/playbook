# 二叉树代码模板

二叉树是力扣中最常见的数据结构之一，掌握二叉树的遍历和操作模板非常重要。

---

## 1. 树节点定义

### Java

```java
// 二叉树节点
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

// N 叉树节点
public class Node {
    public int val;
    public List<Node> children;

    public Node() {}
    public Node(int _val) { val = _val; }
    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
}
```

### Python

```python
# 二叉树节点
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

# N 叉树节点
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children if children is not None else []
```

---

## 2. 二叉树遍历

### 2.1 前序遍历（根 → 左 → 右）

#### 递归写法

**Java**

```java
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
```

**Python**

```python
def preorder_traversal(root):
    result = []

    def preorder(node):
        if not node:
            return
        result.append(node.val)   # 根
        preorder(node.left)       # 左
        preorder(node.right)      # 右

    preorder(root)
    return result
```

---

#### 迭代写法

**Java**

```java
public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) return result;

    Deque<TreeNode> stack = new ArrayDeque<>();
    stack.push(root);

    while (!stack.isEmpty()) {
        TreeNode node = stack.pop();
        result.add(node.val);  // 处理根节点

        // 右子树先入栈（后处理）
        if (node.right != null) stack.push(node.right);
        // 左子树后入栈（先处理）
        if (node.left != null) stack.push(node.left);
    }

    return result;
}
```

**Python**

```python
def preorder_traversal(root):
    if not root:
        return []

    result = []
    stack = [root]

    while stack:
        node = stack.pop()
        result.append(node.val)  # 处理根节点

        # 右子树先入栈（后处理）
        if node.right:
            stack.append(node.right)
        # 左子树后入栈（先处理）
        if node.left:
            stack.append(node.left)

    return result
```

---

### 2.2 中序遍历（左 → 根 → 右）

#### 递归写法

**Java**

```java
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
```

**Python**

```python
def inorder_traversal(root):
    result = []

    def inorder(node):
        if not node:
            return
        inorder(node.left)        # 左
        result.append(node.val)   # 根
        inorder(node.right)       # 右

    inorder(root)
    return result
```

---

#### 迭代写法

**Java**

```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode curr = root;

    while (curr != null || !stack.isEmpty()) {
        // 一直向左走，将路径上的节点入栈
        while (curr != null) {
            stack.push(curr);
            curr = curr.left;
        }

        // 弹出栈顶节点
        curr = stack.pop();
        result.add(curr.val);  // 处理当前节点

        // 转向右子树
        curr = curr.right;
    }

    return result;
}
```

**Python**

```python
def inorder_traversal(root):
    result = []
    stack = []
    curr = root

    while curr or stack:
        # 一直向左走
        while curr:
            stack.append(curr)
            curr = curr.left

        # 弹出栈顶节点
        curr = stack.pop()
        result.append(curr.val)

        # 转向右子树
        curr = curr.right

    return result
```

---

### 2.3 后序遍历（左 → 右 → 根）

#### 递归写法

**Java**

```java
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
```

**Python**

```python
def postorder_traversal(root):
    result = []

    def postorder(node):
        if not node:
            return
        postorder(node.left)        # 左
        postorder(node.right)       # 右
        result.append(node.val)     # 根

    postorder(root)
    return result
```

---

#### 迭代写法（反转法）

**Java**

```java
public List<Integer> postorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) return result;

    Deque<TreeNode> stack = new ArrayDeque<>();
    stack.push(root);

    while (!stack.isEmpty()) {
        TreeNode node = stack.pop();
        result.add(node.val);

        // 左子树先入栈（后处理）
        if (node.left != null) stack.push(node.left);
        // 右子树后入栈（先处理）
        if (node.right != null) stack.push(node.right);
    }

    // 反转结果
    Collections.reverse(result);
    return result;
}
```

**Python**

```python
def postorder_traversal(root):
    if not root:
        return []

    result = []
    stack = [root]

    while stack:
        node = stack.pop()
        result.append(node.val)

        # 左子树先入栈（后处理）
        if node.left:
            stack.append(node.left)
        # 右子树后入栈（先处理）
        if node.right:
            stack.append(node.right)

    # 反转结果
    return result[::-1]
```

---

### 2.4 层序遍历（BFS）

**Java**

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

**Python**

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

## 3. 树的属性计算

### 3.1 树的深度（最大深度）

#### DFS（递归）

**Java**

```java
public int maxDepth(TreeNode root) {
    if (root == null) return 0;

    int leftDepth = maxDepth(root.left);
    int rightDepth = maxDepth(root.right);

    return Math.max(leftDepth, rightDepth) + 1;
}
```

**Python**

```python
def max_depth(root):
    if not root:
        return 0

    left_depth = max_depth(root.left)
    right_depth = max_depth(root.right)

    return max(left_depth, right_depth) + 1
```

---

#### BFS（迭代）

**Java**

```java
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
```

**Python**

```python
from collections import deque

def max_depth(root):
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

### 3.2 平衡二叉树判断

**Java**

```java
public boolean isBalanced(TreeNode root) {
    return getHeight(root) != -1;
}

// 返回高度，不平衡时返回 -1
private int getHeight(TreeNode node) {
    if (node == null) return 0;

    int leftHeight = getHeight(node.left);
    if (leftHeight == -1) return -1;  // 左子树不平衡

    int rightHeight = getHeight(node.right);
    if (rightHeight == -1) return -1;  // 右子树不平衡

    // 检查当前节点是否平衡
    if (Math.abs(leftHeight - rightHeight) > 1) return -1;

    return Math.max(leftHeight, rightHeight) + 1;
}
```

**Python**

```python
def is_balanced(root):
    def get_height(node):
        if not node:
            return 0

        left_height = get_height(node.left)
        if left_height == -1:
            return -1

        right_height = get_height(node.right)
        if right_height == -1:
            return -1

        if abs(left_height - right_height) > 1:
            return -1

        return max(left_height, right_height) + 1

    return get_height(root) != -1
```

---

### 3.3 二叉树直径

**Java**

```java
private int maxDiameter = 0;

public int diameterOfBinaryTree(TreeNode root) {
    maxDepth(root);
    return maxDiameter;
}

private int maxDepth(TreeNode node) {
    if (node == null) return 0;

    int leftDepth = maxDepth(node.left);
    int rightDepth = maxDepth(node.right);

    // 更新直径：经过当前节点的最长路径
    maxDiameter = Math.max(maxDiameter, leftDepth + rightDepth);

    return Math.max(leftDepth, rightDepth) + 1;
}
```

**Python**

```python
class Solution:
    def diameterOfBinaryTree(self, root):
        self.max_diameter = 0
        self.max_depth(root)
        return self.max_diameter

    def max_depth(self, node):
        if not node:
            return 0

        left_depth = self.max_depth(node.left)
        right_depth = self.max_depth(node.right)

        # 更新直径
        self.max_diameter = max(self.max_diameter, left_depth + right_depth)

        return max(left_depth, right_depth) + 1
```

---

## 4. 二叉搜索树（BST）

### 4.1 验证 BST

#### 中序遍历法

**Java**

```java
// 方法1：中序遍历（BST 的中序遍历是递增的）
private long prev = Long.MIN_VALUE;

public boolean isValidBST(TreeNode root) {
    if (root == null) return true;

    // 检查左子树
    if (!isValidBST(root.left)) return false;

    // 检查当前节点
    if (root.val <= prev) return false;
    prev = root.val;

    // 检查右子树
    return isValidBST(root.right);
}
```

**Python**

```python
class Solution:
    def isValidBST(self, root):
        self.prev = float('-inf')
        return self.inorder(root)

    def inorder(self, node):
        if not node:
            return True

        if not self.inorder(node.left):
            return False

        if node.val <= self.prev:
            return False
        self.prev = node.val

        return self.inorder(node.right)
```

---

#### 递归范围法

**Java**

```java
public boolean isValidBST(TreeNode root) {
    return validate(root, null, null);
}

private boolean validate(TreeNode node, Integer min, Integer max) {
    if (node == null) return true;

    // 检查是否在范围内
    if (min != null && node.val <= min) return false;
    if (max != null && node.val >= max) return false;

    // 递归检查子树
    return validate(node.left, min, node.val) &&
           validate(node.right, node.val, max);
}
```

**Python**

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

### 4.2 BST 第 k 小元素

**Java**

```java
private int count = 0;
private int result = 0;

public int kthSmallest(TreeNode root, int k) {
    inorder(root, k);
    return result;
}

private void inorder(TreeNode node, int k) {
    if (node == null) return;

    inorder(node.left, k);

    count++;
    if (count == k) {
        result = node.val;
        return;
    }

    inorder(node.right, k);
}
```

**Python**

```python
def kth_smallest(root, k):
    count = 0
    result = None

    def inorder(node):
        nonlocal count, result
        if not node or result is not None:
            return

        inorder(node.left)

        count += 1
        if count == k:
            result = node.val
            return

        inorder(node.right)

    inorder(root)
    return result
```

---

## 5. 最近公共祖先（LCA）

**Java**

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    // 递归终止条件
    if (root == null || root == p || root == q) {
        return root;
    }

    // 在左右子树中查找
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);

    // p 和 q 分别在左右子树中
    if (left != null && right != null) {
        return root;
    }

    // 返回非空的那一边
    return left != null ? left : right;
}
```

**Python**

```python
def lowest_common_ancestor(root, p, q):
    # 递归终止条件
    if not root or root is p or root is q:
        return root

    # 在左右子树中查找
    left = lowest_common_ancestor(root.left, p, q)
    right = lowest_common_ancestor(root.right, p, q)

    # p 和 q 分别在左右子树中
    if left and right:
        return root

    # 返回非空的那一边
    return left if left else right
```

---

## 6. 路径问题

### 6.1 路径总和

**Java**

```java
public boolean hasPathSum(TreeNode root, int targetSum) {
    if (root == null) return false;

    // 叶子节点
    if (root.left == null && root.right == null) {
        return root.val == targetSum;
    }

    // 递归检查左右子树
    return hasPathSum(root.left, targetSum - root.val) ||
           hasPathSum(root.right, targetSum - root.val);
}
```

**Python**

```python
def has_path_sum(root, target_sum):
    if not root:
        return False

    # 叶子节点
    if not root.left and not root.right:
        return root.val == target_sum

    # 递归检查左右子树
    return has_path_sum(root.left, target_sum - root.val) or \
           has_path_sum(root.right, target_sum - root.val)
```

---

### 6.2 路径总和 II（返回所有路径）

**Java**

```java
public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    backtrack(root, targetSum, path, result);
    return result;
}

private void backtrack(TreeNode node, int target, List<Integer> path, List<List<Integer>> result) {
    if (node == null) return;

    path.add(node.val);
    target -= node.val;

    // 叶子节点且目标为 0
    if (node.left == null && node.right == null && target == 0) {
        result.add(new ArrayList<>(path));
    }

    backtrack(node.left, target, path, result);
    backtrack(node.right, target, path, result);

    path.remove(path.size() - 1);  // 回溯
}
```

**Python**

```python
def path_sum(root, target_sum):
    result = []
    path = []

    def backtrack(node, target):
        if not node:
            return

        path.append(node.val)
        target -= node.val

        # 叶子节点且目标为 0
        if not node.left and not node.right and target == 0:
            result.append(path.copy())

        backtrack(node.left, target)
        backtrack(node.right, target)

        path.pop()  # 回溯

    backtrack(root, target_sum)
    return result
```

---

## 7. 序列化和反序列化

### 前序序列化（DFS）

**Java**

```java
// 序列化
public String serialize(TreeNode root) {
    StringBuilder sb = new StringBuilder();
    serialize(root, sb);
    return sb.toString();
}

private void serialize(TreeNode node, StringBuilder sb) {
    if (node == null) {
        sb.append("#").append(",");
        return;
    }

    sb.append(node.val).append(",");
    serialize(node.left, sb);
    serialize(node.right, sb);
}

// 反序列化
public TreeNode deserialize(String data) {
    String[] nodes = data.split(",");
    Queue<String> queue = new LinkedList<>(Arrays.asList(nodes));
    return deserialize(queue);
}

private TreeNode deserialize(Queue<String> queue) {
    String val = queue.poll();
    if (val.equals("#")) return null;

    TreeNode node = new TreeNode(Integer.parseInt(val));
    node.left = deserialize(queue);
    node.right = deserialize(queue);
    return node;
}
```

**Python**

```python
class Codec:

    def serialize(self, root):
        def preorder(node):
            if not node:
                return '#,'
            return str(node.val) + ',' + preorder(node.left) + preorder(node.right)
        return preorder(root)

    def deserialize(self, data):
        def preorder(nodes):
            val = next(nodes)
            if val == '#':
                return None
            node = TreeNode(int(val))
            node.left = preorder(nodes)
            node.right = preorder(nodes)
            return node

        nodes = iter(data.split(','))
        return preorder(nodes)
```

---

## 8. 复杂度总结

| 操作 | 时间复杂度 | 空间复杂度 |
|------|-----------|-----------|
| 前序/中序/后序遍历（递归） | O(n) | O(h)，h 为树高 |
| 前序/中序/后序遍历（迭代） | O(n) | O(h) |
| 层序遍历 | O(n) | O(w)，w 为最大宽度 |
| 树的深度 | O(n) | O(h) |
| 验证 BST | O(n) | O(h) |
| LCA | O(n) | O(h) |

**说明**：
- h 为树的高度，平衡树 h = log n，最坏情况 h = n
- 递归解法的空间复杂度主要来自递归调用栈

---

## 9. 常见陷阱

| 陷阱 | 说明 | 解决方案 |
|------|------|----------|
| 空指针 | 忘记检查 node == null | 在递归/迭代开始前检查 |
| 全局变量 | 递归中使用全局变量容易出错 | 使用类成员变量或返回值传递 |
| BST 验证 | 只比较父子节点会出错 | 使用有效范围 (min, max) |
| 路径问题 | 需要回溯恢复状态 | 添加元素后记得移除 |
| 层序遍历 | 需要按层分组 | 使用 for 循环处理当前层的所有节点 |
| 中序遍历迭代 | 指针移动逻辑复杂 | 记住"一路向左"的思路 |
