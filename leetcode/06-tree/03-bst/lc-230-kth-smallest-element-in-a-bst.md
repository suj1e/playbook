# 230. Kth Smallest Element in a BST / 二叉搜索树中第K小的元素

[中文链接](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/) | [English](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

**难度**：Medium
**标签**：树 | 深度优先搜索 | 二叉搜索树 | 二叉树
**相似题目**：[lc-94](../01-traversal/lc-94-binary-tree-inorder-traversal.md) | [lc-538](./)
**面试频次**：⭐⭐⭐⭐

---

## 题目描述

给定一个二叉搜索树的根节点 `root` ，和一个整数 `k` ，请你设计一个算法查找其中第 `k` 小的元素（从 1 开始计数）。

**示例 1**：
```
输入：root = [3,1,4,null,2], k = 1
输出：1
```

**示例 2**：
```
输入：root = [5,3,6,2,4,null,null,1], k = 3
输出：3
```

**约束条件**：
- 树中的节点数为 n
- 1 <= k <= n <= 10⁴
- 0 <= Node.val <= 10⁴

---

## 解题思路

### 方法一：中序遍历（推荐）

**思路**：
BST 的中序遍历是升序序列，遍历到第 k 个节点即可。

**核心思想**：
- BST 性质：左 < 根 < 右
- 中序遍历：左 → 根 → 右 = 升序
- 计数器记录访问的节点数

**复杂度**：
- 时间：O(H + k)，H 是树高，最坏 O(n)
- 空间：O(H) - 递归栈

**步骤**：
1. 中序遍历 BST
2. 维护计数器 count
3. 当 count == k 时，返回当前节点值

### 方法二：迭代中序遍历

**思路**：
使用栈进行中序遍历，可以提前终止。

**复杂度**：
- 时间：O(H + k)
- 空间：O(H)

---

## 代码实现

### Java - 递归中序遍历

```java
class Solution {
    private int count = 0;
    private int result = 0;

    public int kthSmallest(TreeNode root, int k) {
        inorder(root, k);
        return result;
    }

    private void inorder(TreeNode node, int k) {
        if (node == null) return;

        // 左
        inorder(node.left, k);

        // 根
        count++;
        if (count == k) {
            result = node.val;
            return;
        }

        // 右
        inorder(node.right, k);
    }
}
```

### Java - 迭代中序遍历（可提前终止）

```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        Deque<TreeNode> stack = new LinkedList<>();
        TreeNode curr = root;
        int count = 0;

        while (curr != null || !stack.isEmpty()) {
            // 一直往左走
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }

            // 处理当前节点
            curr = stack.pop();
            count++;
            if (count == k) {
                return curr.val;
            }

            // 转向右子树
            curr = curr.right;
        }

        return -1;  // 不会到达这里
    }
}
```

### Python - 递归中序遍历

```python
from typing import Optional

class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        self.count = 0
        self.result = 0

        def inorder(node):
            if node is None:
                return

            inorder(node.left)

            self.count += 1
            if self.count == k:
                self.result = node.val
                return

            inorder(node.right)

        inorder(root)
        return self.result
```

### Python - 迭代中序遍历

```python
from typing import Optional

class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        stack = []
        curr = root
        count = 0

        while curr or stack:
            while curr:
                stack.append(curr)
                curr = curr.left

            curr = stack.pop()
            count += 1
            if count == k:
                return curr.val

            curr = curr.right

        return -1
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| BST 性质 | 中序遍历是升序序列 |
| 中序遍历 | 左 → 根 → 右 |
| 提前终止 | 找到第 k 个立即返回 |
| 计数器 | 记录已访问节点数 |
| 时间复杂度 | 最坏 O(n)，最好 O(log n + k) |

---

## 图解

```
示例 1: root = [3,1,4,null,2], k = 1

      3
     / \
    1   4
     \
      2

中序遍历: 1 → 2 → 3 → 4
         ↑
        第1小

输出: 1

---

示例 2: root = [5,3,6,2,4,null,null,1], k = 3

        5
       / \
      3   6
     / \
    2   4
   /
  1

中序遍历: 1 → 2 → 3 → 4 → 5 → 6
                   ↑
                  第3小

输出: 3

---

中序遍历过程:

        5
       / \
      3   6
     / \
    2   4
   /
  1

递归过程:
inorder(5):
  inorder(3):
    inorder(2):
      inorder(1):
        inorder(null) 左
        count=1, return 1 ✓ (k=1时)
        inorder(null) 右
      count=2, val=2 (k=2时)
      inorder(null) 右
    count=3, val=3 ✓ (k=3时)
    inorder(4):
      inorder(null) 左
      count=4, val=4 (k=4时)
      inorder(null) 右
  count=5, val=5 (k=5时)
  inorder(6):
    inorder(null) 左
    count=6, val=6 (k=6时)
    inorder(null) 右
```

---

## 变种/延伸

### 变种1：二叉树的中序遍历
- **题目**：[lc-94 二叉树的中序遍历](../01-traversal/lc-94-binary-tree-inorder-traversal.md)
- **变化**：返回整个遍历序列
- **解法**：类似，不提前终止

### 变种2：验证二叉搜索树
- **题目**：[lc-98 验证二叉搜索树](./lc-98-validate-binary-search-tree.md)
- **变化**：判断是否是 BST
- **解法**：中序遍历检查是否递增

### 变种3：第 K 大的元素
- **变化**：找第 k 大而非第 k 小
- **解法**：右 → 根 → 左 的遍历顺序

### 进阶思考

1. **为什么中序遍历是升序？**
   - BST 定义：左子树所有节点 < 根 < 右子树所有节点
   - 中序遍历先左后右，自然得到升序

2. **如何找第 k 大？**
   - 反向中序遍历：右 → 根 → 左
   - 或者先遍历完，取倒数第 k 个

3. **如果频繁查询第 k 小？**
   - 可以在节点中维护左子树大小
   - 查询复杂度 O(log n)

4. **如何处理重复元素？**
   - 本题假设元素唯一
   - 有重复时，需要明确定义"第k小"

5. **与快速选择的关系？**
   - 数组找第 k 小可以用快速选择
   - BST 利用有序性质，更高效

6. **如何优化空间？**
   - Morris 中序遍历：O(1) 空间
   - 通过修改指针实现
