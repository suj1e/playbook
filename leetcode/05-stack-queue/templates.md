# 栈和队列代码模板

栈是 LIFO（后进先出），队列是 FIFO（先进先出）。

---

## 1. 栈的基本操作

### 1.1 初始化和操作

```
栈操作图解:

push(1):  [1]
          ↑
         top

push(2):  [1, 2]
            ↑
          top

pop():    弹出 2
          [1]
          ↑
         top
```

#### Java
```java
// 推荐使用 Deque
Deque<Integer> stack = new ArrayDeque<>();

// 入栈
stack.push(1);

// 出栈
int top = stack.pop();

// 查看栈顶
int peek = stack.peek();

// 判空
boolean empty = stack.isEmpty();
```

#### Python
```python
# 使用 list 作为栈
stack = []

# 入栈
stack.append(1)

# 出栈
top = stack.pop()

# 查看栈顶
top = stack[-1]
```

---

## 2. 单调栈

### 2.1 单调递增栈（找下一个更大元素）

**适用场景**：下一个更大元素、每日温度

```
数组: [2, 1, 5, 6, 2, 3]

单调递增栈（栈底 -> 栈顶 递增）:

i=0, num=2: stack=[2]
i=1, num=1: stack=[1,2]  (1<2, 直接入栈)
i=2, num=5: 弹出2(5>2), 弹出1(5>1)
           stack=[5]
           result[0]=5, result[1]=5

i=3, num=6: 弹出5(6>5)
           stack=[6]
           result[2]=6

i=4, num=2: stack=[2,6]
i=5, num=3: 弹出2(3>2)
           stack=[3,6]
           result[4]=3
```

#### Java
```java
// 单调递增栈
int[] result = new int[nums.length];
Arrays.fill(result, -1);  // 默认值
Deque<Integer> stack = new ArrayDeque<>();  // 存储索引

for (int i = 0; i < nums.length; i++) {
    // 当前元素大于栈顶元素，弹出栈顶
    while (!stack.isEmpty() && nums[i] > nums[stack.peek()]) {
        int top = stack.pop();
        result[top] = nums[i];
    }
    stack.push(i);
}
```

#### Python
```python
# 单调递增栈
result = [-1] * len(nums)
stack = []

for i in range(len(nums)):
    while stack and nums[i] > nums[stack[-1]]:
        top = stack.pop()
        result[top] = nums[i]
    stack.append(i)
```

---

## 3. 单调队列（滑动窗口最大值）

### 3.1 单调递减队列

**适用场景**：滑动窗口最大值

```
数组: [1, 3, -1, -3, 5, 3, 6, 7], k = 3

单调递减队列（存索引，值递减）:

窗口[0,2]=[1,3,-1]: deque=[1] (值3最大)
窗口[1,3]=[3,-1,-3]: deque=[1,2] (值3,-1)
窗口[2,4]=[-1,-3,5]: 弹出1,2; 入4; deque=[4]
                       result[2]=5

特点: 队首始终是窗口最大值
```

#### Java
```java
public int[] maxSlidingWindow(int[] nums, int k) {
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
    return result;
}
```

#### Python
```python
from collections import deque

def max_sliding_window(nums, k):
    result = []
    dq = deque()  # 存储索引

    for i in range(len(nums)):
        # 移除窗口外的元素
        while dq and dq[0] < i - k + 1:
            dq.popleft()

        # 维护单调性
        while dq and nums[dq[-1]] < nums[i]:
            dq.pop()

        dq.append(i)

        # 记录窗口最大值
        if i >= k - 1:
            result.append(nums[dq[0]])

    return result
```

---

## 4. 括号匹配

### 4.1 栈匹配模板

**适用场景**：有效的括号

```
输入: "{[()]}"

步骤:
1. '{' 入栈: ['{']
2. '[' 入栈: ['{', '[']
3. '(' 入栈: ['{', '[', '(']
4. ')' 匹配 '(': 弹出: ['{', '[']
5. ']' 匹配 '[': 弹出: ['{']
6. '}' 匹配 '{': 弹出: []

栈为空，返回 true
```

#### Java
```java
public boolean isValid(String s) {
    Deque<Character> stack = new ArrayDeque<>();

    for (char c : s.toCharArray()) {
        if (c == '(' || c == '[' || c == '{') {
            stack.push(c);  // 左括号入栈
        } else {
            if (stack.isEmpty()) return false;
            char top = stack.pop();
            // 检查是否匹配
            if (c == ')' && top != '(') return false;
            if (c == ']' && top != '[') return false;
            if (c == '}' && top != '{') return false;
        }
    }
    return stack.isEmpty();
}
```

#### Python
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

## 5. 复杂度总结

| 操作 | 栈 | 队列 | 单调栈/队列 |
|------|-----|------|-----------|
| 入栈/入队 | O(1) | O(1) | O(1) |
| 出栈/出队 | O(1) | O(1) | O(1) |
| 查看栈顶/队首 | O(1) | O(1) | O(1) |
| 单调处理 | O(n) | - | O(n) |
