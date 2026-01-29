# 栈和队列代码模板

栈（Stack）和队列（Queue）是两种基本的线性数据结构，栈是"后进先出"（LIFO），队列是"先进先出"（FIFO）。

---

## 1. 基本定义和初始化

### 1.1 栈 (Stack)

#### Java

```java
import java.util.*;

// 方法1：使用 Deque 作为栈（推荐）
Deque<Integer> stack = new ArrayDeque<>();
// 或
Deque<Integer> stack = new LinkedList<>();

// 方法2：使用 Stack 类（不推荐，已过时）
Stack<Integer> stack = new Stack<>();
```

#### Python

```python
# 使用 list 作为栈
stack = []

# 或使用 collections.deque（更高效）
from collections import deque
stack = deque()
```

---

### 1.2 队列 (Queue)

#### Java

```java
// 使用 Deque 作为队列
Deque<Integer> queue = new ArrayDeque<>();
// 或
Queue<Integer> queue = new LinkedList<>();

// 使用 PriorityQueue（优先队列）
PriorityQueue<Integer> pq = new PriorityQueue<>();
PriorityQueue<Integer> maxPQ = new PriorityQueue<>(Collections.reverseOrder());
```

#### Python

```python
# 使用 collections.deque 作为队列
from collections import deque
queue = deque()

# 使用 queue.Queue（线程安全）
from queue import Queue
q = Queue()

# 使用 heapq 作为优先队列
import heapq
min_heap = []
max_heap = []  # 通过负数实现
```

---

## 2. 栈的基本操作

### 2.1 标准操作

#### Java

```java
Deque<Integer> stack = new ArrayDeque<>();

// 入栈（push）
stack.push(1);
stack.addFirst(1);  // 等价操作

// 出栈（pop）
int top = stack.pop();
// 或
int top = stack.removeFirst();

// 查看栈顶元素（不删除）
int peek = stack.peek();
// 或
int peek = stack.peekFirst();

// 判断栈是否为空
boolean empty = stack.isEmpty();

// 获取栈大小
int size = stack.size();
```

#### Python

```python
stack = []

# 入栈
stack.append(1)

# 出栈
top = stack.pop()

# 查看栈顶元素（不删除）
top = stack[-1]

# 判断栈是否为空
empty = len(stack) == 0

# 获取栈大小
size = len(stack)
```

---

### 2.2 单调栈模板

**单调递增栈**（栈底到栈顶递增）

#### Java

```java
// 单调递增栈：用于找下一个更大元素
int[] nums = {2, 1, 5, 6, 2, 3};
int n = nums.length;
int[] result = new int[n];
Arrays.fill(result, -1);  // 默认值

Deque<Integer> stack = new ArrayDeque<>();  // 存储索引

for (int i = 0; i < n; i++) {
    // 当前元素大于栈顶元素，弹出栈顶
    while (!stack.isEmpty() && nums[i] > nums[stack.peek()]) {
        int top = stack.pop();
        result[top] = nums[i];  // 记录下一个更大元素
    }
    stack.push(i);
}
```

#### Python

```python
# 单调递增栈
nums = [2, 1, 5, 6, 2, 3]
n = len(nums)
result = [-1] * n  # 默认值

stack = []  # 存储索引

for i in range(n):
    # 当前元素大于栈顶元素，弹出栈顶
    while stack and nums[i] > nums[stack[-1]]:
        top = stack.pop()
        result[top] = nums[i]
    stack.append(i)
```

---

**单调递减栈**（栈底到栈顶递减）

#### Java

```java
// 单调递减栈：用于找下一个更小元素
Deque<Integer> stack = new ArrayDeque<>();

for (int i = 0; i < n; i++) {
    // 当前元素小于栈顶元素，弹出栈顶
    while (!stack.isEmpty() && nums[i] < nums[stack.peek()]) {
        int top = stack.pop();
        result[top] = nums[i];  // 记录下一个更小元素
    }
    stack.push(i);
}
```

#### Python

```python
# 单调递减栈
stack = []

for i in range(n):
    while stack and nums[i] < nums[stack[-1]]:
        top = stack.pop()
        result[top] = nums[i]
    stack.append(i)
```

---

## 3. 队列的基本操作

### 3.1 标准操作

#### Java

```java
Queue<Integer> queue = new LinkedList<>();

// 入队
queue.offer(1);

// 出队
int front = queue.poll();

// 查看队首元素（不删除）
int peek = queue.peek();

// 判断队列是否为空
boolean empty = queue.isEmpty();

// 获取队列大小
int size = queue.size();
```

#### Python

```python
from collections import deque
queue = deque()

# 入队
queue.append(1)

# 出队
front = queue.popleft()

# 查看队首元素（不删除）
front = queue[0]

# 判断队列是否为空
empty = len(queue) == 0

# 获取队列大小
size = len(queue)
```

---

### 3.2 单调队列模板（滑动窗口最大值）

#### Java

```java
// 单调递减队列：用于滑动窗口最大值
int[] nums = {1, 3, -1, -3, 5, 3, 6, 7};
int k = 3;

int[] result = new int[nums.length - k + 1];
Deque<Integer> deque = new ArrayDeque<>();  // 存储索引

for (int i = 0; i < nums.length; i++) {
    // 移除窗口外的元素
    while (!deque.isEmpty() && deque.peekFirst() < i - k + 1) {
        deque.pollFirst();
    }

    // 维护单调性：移除比当前元素小的
    while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
        deque.pollLast();
    }

    deque.offerLast(i);

    // 记录窗口最大值
    if (i >= k - 1) {
        result[i - k + 1] = nums[deque.peekFirst()];
    }
}
```

#### Python

```python
# 单调递减队列
nums = [1, 3, -1, -3, 5, 3, 6, 7]
k = 3

result = []
from collections import deque
deque_window = deque()  # 存储索引

for i in range(len(nums)):
    # 移除窗口外的元素
    while deque_window and deque_window[0] < i - k + 1:
        deque_window.popleft()

    # 维护单调性：移除比当前元素小的
    while deque_window and nums[deque_window[-1]] < nums[i]:
        deque_window.pop()

    deque_window.append(i)

    # 记录窗口最大值
    if i >= k - 1:
        result.append(nums[deque_window[0]])
```

---

## 4. 括号匹配模板

### Java

```java
public boolean isValid(String s) {
    Deque<Character> stack = new ArrayDeque<>();

    for (char c : s.toCharArray()) {
        if (c == '(' || c == '[' || c == '{') {
            stack.push(c);  // 左括号入栈
        } else {
            if (stack.isEmpty()) return false;  // 栈为空，无法匹配

            char top = stack.pop();
            // 检查是否匹配
            if (c == ')' && top != '(') return false;
            if (c == ']' && top != '[') return false;
            if (c == '}' && top != '{') return false;
        }
    }

    return stack.isEmpty();  // 栈必须为空
}
```

### Python

```python
def is_valid(s):
    stack = []
    mapping = {')': '(', ']': '[', '}': '{'}

    for c in s:
        if c in mapping.values():  # 左括号
            stack.append(c)
        else:  # 右括号
            if not stack or stack[-1] != mapping[c]:
                return False
            stack.pop()

    return len(stack) == 0
```

---

## 5. 逆波兰表达式求值

### Java

```java
public int evalRPN(String[] tokens) {
    Deque<Integer> stack = new ArrayDeque<>();

    for (String token : tokens) {
        if (isOperator(token)) {
            // 弹出两个操作数
            int b = stack.pop();
            int a = stack.pop();
            // 计算结果并入栈
            stack.push(apply(a, b, token));
        } else {
            stack.push(Integer.parseInt(token));
        }
    }

    return stack.pop();
}

private boolean isOperator(String token) {
    return token.equals("+") || token.equals("-") ||
           token.equals("*") || token.equals("/");
}

private int apply(int a, int b, String op) {
    switch (op) {
        case "+": return a + b;
        case "-": return a - b;
        case "*": return a * b;
        case "/": return a / b;
    }
    return 0;
}
```

### Python

```python
def eval_rpn(tokens):
    stack = []

    for token in tokens:
        if token in '+-*/':
            b = stack.pop()
            a = stack.pop()
            if token == '+':
                stack.append(a + b)
            elif token == '-':
                stack.append(a - b)
            elif token == '*':
                stack.append(a * b)
            else:
                stack.append(int(a / b))
        else:
            stack.append(int(token))

    return stack[0]
```

---

## 6. 最小栈（O(1) 获取最小值）

### Java

```java
class MinStack {
    private Deque<int[]> stack;  // [val, min]

    public MinStack() {
        stack = new ArrayDeque<>();
    }

    public void push(int val) {
        if (stack.isEmpty()) {
            stack.push(new int[]{val, val});
        } else {
            int min = Math.min(val, stack.peek()[1]);
            stack.push(new int[]{val, min});
        }
    }

    public void pop() {
        stack.pop();
    }

    public int top() {
        return stack.peek()[0];
    }

    public int getMin() {
        return stack.peek()[1];
    }
}
```

### Python

```python
class MinStack:
    def __init__(self):
        self.stack = []  # [(val, min)]

    def push(self, val: int):
        if not self.stack:
            self.stack.append((val, val))
        else:
            min_val = min(val, self.stack[-1][1])
            self.stack.append((val, min_val))

    def pop(self):
        self.stack.pop()

    def top(self) -> int:
        return self.stack[-1][0]

    def getMin(self) -> int:
        return self.stack[-1][1]
```

---

## 7. BFS 队列模板

### Java

```java
// 图的 BFS 遍历
public void bfs(int[][] graph, int start) {
    int n = graph.length;
    boolean[] visited = new boolean[n];
    Queue<Integer> queue = new LinkedList<>();

    queue.offer(start);
    visited[start] = true;

    while (!queue.isEmpty()) {
        int node = queue.poll();
        // 处理当前节点
        System.out.println(node);

        // 遍历邻接节点
        for (int neighbor : graph[node]) {
            if (!visited[neighbor]) {
                queue.offer(neighbor);
                visited[neighbor] = true;
            }
        }
    }
}
```

### Python

```python
from collections import deque

def bfs(graph, start):
    n = len(graph)
    visited = [False] * n
    queue = deque([start])
    visited[start] = True

    while queue:
        node = queue.popleft()
        # 处理当前节点
        print(node)

        # 遍历邻接节点
        for neighbor in graph[node]:
            if not visited[neighbor]:
                queue.append(neighbor)
                visited[neighbor] = True
```

---

## 8. 复杂度总结

| 操作 | 栈 | 队列 | 优先队列 |
|------|-----|------|---------|
| 入栈/入队 | O(1) | O(1) | O(log n) |
| 出栈/出队 | O(1) | O(1) | O(log n) |
| 查看栈顶/队首 | O(1) | O(1) | O(1) |
| 单调栈处理 | O(n) | - | - |
| 单调队列处理 | - | O(n) | - |

---

## 9. 常见应用场景

| 场景 | 使用数据结构 | 典型题目 |
|------|-------------|---------|
| 括号匹配 | 栈 | lc-20 有效的括号 |
| 下一个更大元素 | 单调递增栈 | lc-496, lc-739 |
| 柱状图最大矩形 | 单调递增栈 | lc-84 |
| 滑动窗口最大值 | 单调递减队列 | lc-239 |
| 表达式求值 | 栈 | lc-150, lc-224 |
| 最小栈 | 辅助栈 | lc-155 |
| 图的遍历 | 队列 | lc-200, lc-207 |
| TopK 问题 | 优先队列 | lc-215, lc-347 |
| 生成括号 | 栈（回溯） | lc-22 |

---

## 10. 常见陷阱

| 陷阱 | 说明 | 解决方案 |
|------|------|----------|
| 栈空操作 | 栈为空时 peek/pop 会抛异常 | 使用 isEmpty() 检查 |
| 队列空操作 | 队列为空时 poll 返回 null，peek 返回 null | 注意 null 判断 |
| 单调栈存储内容 | 存储值还是索引 | 通常存储索引便于计算距离 |
| 优先队列顺序 | 默认小根堆 | 需要大根堆时使用 Comparator 或负数 |
| 队列实现 | Java 不要用 Stack，用 Deque | Stack 是遗留类，性能较差 |
| Python 队列 | 不要用 list 做队列（头部操作 O(n)） | 使用 deque 的 popleft() |
