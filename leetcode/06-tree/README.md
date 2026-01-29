# 二叉树专题

## 专题概述

二叉树是最重要的树形数据结构之一，本专题涵盖：

- **遍历**：前中后序遍历（递归和迭代）
- **性质**：深度、平衡性、路径问题
- **BST**：二叉搜索树的特性
- **LCA**：最近公共祖先

### 核心知识点

1. 二叉树的递归定义
2. DFS（深度优先搜索）和 BFS（广度优先搜索）
3. BST 的性质：左 < 根 < 右

---

## 学习路径

1. 先阅读 [templates.md](./templates.md) 熟悉二叉树操作模板
2. 按顺序完成各分类题目：
   - [ ] 01-traversal（遍历，基础必做）
   - [ ] 02-properties（树的性质）
   - [ ] 03-bst（二叉搜索树）
   - [ ] 04-lca（最近公共祖先）

---

## 题目分类

| 分类 | 题目数 | 难度 | 说明 |
|------|-------|------|------|
| 01-traversal | 4 | ⭐⭐ | 前中后序、层序遍历 |
| 02-properties | 3 | ⭐⭐ | 深度、平衡性 |
| 03-bst | 2 | ⭐⭐⭐ | BST 特性应用 |
| 04-lca | 1 | ⭐⭐⭐ | 最近公共祖先 |

---

## 进度追踪

### 01-traversal
- [ ] lc-144-binary-tree-preorder-traversal (Easy)
- [ ] lc-94-binary-tree-inorder-traversal (Easy)
- [ ] lc-145-binary-tree-postorder-traversal (Easy)
- [ ] lc-102-binary-tree-level-order-traversal (Medium)

### 02-properties
- [ ] lc-104-maximum-depth-of-binary-tree (Easy)
- [ ] lc-110-balanced-binary-tree (Easy)
- [ ] lc-236-lowest-common-ancestor-of-a-binary-tree (Medium)

### 03-bst
- [ ] lc-98-validate-binary-search-tree (Medium)
- [ ] lc-230-kth-smallest-element-in-a-bst (Medium)

### 04-lca
- [ ] lc-236-lowest-common-ancestor-of-a-binary-tree (Medium)

---

## 解题模板总结

### 二叉树定义
```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
}
```

### 递归遍历模板
```java
public void traverse(TreeNode root) {
    if (root == null) return;

    // 前序位置
    traverse(root.left);
    // 中序位置
    traverse(root.right);
    // 后序位置
}
```

### 层序遍历模板
```java
public void levelOrder(TreeNode root) {
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);

    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            TreeNode node = queue.poll();
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
    }
}
```

---

## 参考资源

- [LeetCode 树标签](https://leetcode.cn/tag/tree/)
