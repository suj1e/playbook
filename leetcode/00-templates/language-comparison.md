# Java vs Python 语法对照

力扣刷题中 Java 和 Python 的常用语法对照。

---

## 1. 基本类型

| Java | Python | 说明 |
|------|--------|------|
| `int` | `int` | 整数 |
| `long` | `int` | 长整数（Python 自动处理大整数） |
| `double` | `float` | 浮点数 |
| `boolean` | `bool` | 布尔值 |
| `char` | `str` | 字符 |
| `String` | `str` | 字符串 |

---

## 2. 数组/列表

### 创建

```java
// Java
int[] arr = new int[10];
int[] arr = {1, 2, 3};
int[][] matrix = new int[3][4];
```

```python
# Python
arr = [0] * 10
arr = [1, 2, 3]
matrix = [[0] * 4 for _ in range(3)]
```

### 长度

```java
// Java
int n = arr.length;
int rows = matrix.length;
int cols = matrix[0].length;
```

```python
# Python
n = len(arr)
rows = len(matrix)
cols = len(matrix[0])
```

### 访问

```java
// Java
int val = arr[i];
arr[i] = 10;
```

```python
# Python
val = arr[i]
arr[i] = 10
```

### 切片

```java
// Java - 没有切片，需要复制
int[] copy = Arrays.copyOfRange(arr, 0, 5);
```

```python
# Python - 支持切片
sub = arr[0:5]   # [0, 5)
sub = arr[:5]    # [0, 5)
sub = arr[2:]    # [2, end)
sub = arr[::-1]  # 反转
```

---

## 3. 字符串

### 创建

```java
// Java
String s = "hello";
String s = new String("hello");
```

```python
# Python
s = "hello"
s = str(123)
```

### 拼接

```java
// Java
String s = "hello" + " world";
String s = String.format("%d %s", 123, "abc");
StringBuilder sb = new StringBuilder();
sb.append("hello").append(123);
String s = sb.toString();
```

```python
# Python
s = "hello" + " world"
s = f"{123} {abc}"
s = "{} {}".format(123, "abc")
```

### 字符访问

```java
// Java
char c = s.charAt(i);
```

```python
# Python
c = s[i]
```

### 遍历

```java
// Java
for (int i = 0; i < s.length(); i++) {
    char c = s.charAt(i);
}
for (char c : s.toCharArray()) {
    // ...
}
```

```python
# Python
for c in s:
    # ...
for i, c in enumerate(s):
    # ...
```

### 常用方法对照

| Java | Python | 说明 |
|------|--------|------|
| `s.length()` | `len(s)` | 长度 |
| `s.substring(0, 5)` | `s[0:5]` | 子串 |
| `s.indexOf("abc")` | `s.find("abc")` | 查找 |
| `s.contains("abc")` | `"abc" in s` | 包含 |
| `s.split(",")` | `s.split(",")` | 分割 |
| `s.replace("a", "b")` | `s.replace("a", "b")` | 替换 |
| `s.toLowerCase()` | `s.lower()` | 小写 |
| `s.toUpperCase()` | `s.upper()` | 大写 |
| `s.trim()` | `s.strip()` | 去空格 |
| `s.isEmpty()` | `len(s) == 0` | 为空 |

---

## 4. 列表（动态数组）

### Java ArrayList

```java
// 创建
List<Integer> list = new ArrayList<>();
List<Integer> list = new ArrayList<>(100);

// 添加
list.add(1);
list.add(0, 2);  // 指定位置

// 访问
int val = list.get(i);

// 修改
list.set(i, 10);

// 删除
list.remove(i);
list.remove(Integer.valueOf(5));

// 大小
int n = list.size();

// 排序
Collections.sort(list);
Collections.sort(list, (a, b) -> b - a);
```

### Python List

```python
# 创建
list = []
list = [0] * 100

# 添加
list.append(1)
list.insert(0, 2)

# 访问
val = list[i]

# 修改
list[i] = 10

# 删除
list.pop(i)
list.remove(5)
del list[i]

# 大小
n = len(list)

# 排序
list.sort()
list.sort(reverse=True)
```

---

## 5. 哈希表

### Java HashMap

```java
// 创建
Map<String, Integer> map = new HashMap<>();

// 添加
map.put("a", 1);

// 获取
Integer val = map.get("a");
Integer val = map.getOrDefault("a", 0);

// 删除
map.remove("a");

// 检查
boolean has = map.containsKey("a");

// 遍历
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    String key = entry.getKey();
    Integer val = entry.getValue();
}

for (String key : map.keySet()) { }
for (Integer val : map.values()) { }
```

### Python Dict

```python
# 创建
d = {}
d = {'a': 1}
from collections import defaultdict
d = defaultdict(int)

# 添加
d['a'] = 1

# 获取
val = d['a']
val = d.get('a', 0)

# 删除
del d['a']
val = d.pop('a', None)

# 检查
has = 'a' in d

# 遍历
for key, val in d.items():
    pass

for key in d.keys():
    pass

for val in d.values():
    pass
```

---

## 6. 集合

### Java HashSet

```java
// 创建
Set<Integer> set = new HashSet<>();

// 添加
set.add(1);

// 删除
set.remove(1);

// 检查
boolean has = set.contains(1);

// 遍历
for (int val : set) { }
```

### Python Set

```python
# 创建
s = set()

# 添加
s.add(1)

# 删除
s.remove(1)
s.discard(1)

# 检查
has = 1 in s

# 遍历
for val in s:
    pass
```

---

## 7. 栈

### Java（推荐 Deque）

```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(1);
int val = stack.pop();
int val = stack.peek();
boolean empty = stack.isEmpty();
```

### Python（用 List）

```python
stack = []
stack.append(1)
val = stack.pop()
val = stack[-1]
empty = len(stack) == 0
```

---

## 8. 队列

### Java

```java
Queue<Integer> queue = new LinkedList<>();
queue.offer(1);
int val = queue.poll();
int val = queue.peek();
```

### Python

```python
from collections import deque
queue = deque()
queue.append(1)
val = queue.popleft()
val = queue[0]
```

---

## 9. 堆

### Java（优先队列）

```java
// 最小堆
Queue<Integer> pq = new PriorityQueue<>();

// 最大堆
Queue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);

pq.offer(1);
int val = pq.poll();
int val = pq.peek();
```

### Python（heapq）

```python
import heapq

# 最小堆
heap = []
heapq.heappush(heap, 1)
val = heapq.heappop(heap)
val = heap[0]

# 最大堆（技巧：存负数）
heapq.heappush(heap, -1)
val = -heapq.heappop(heap)
```

---

## 10. 循环

### For 循环

```java
// Java - 索引循环
for (int i = 0; i < n; i++) {
    // arr[i]
}

// Java - 增强 for
for (int val : arr) {
    // val
}

// Java - 带索引
for (int i = 0; i < arr.length; i++) {
    int val = arr[i];
    // i 是索引
}
```

```python
# Python - 索引循环
for i in range(n):
    # arr[i]

# Python - 直接遍历
for val in arr:
    # val

# Python - 带索引
for i, val in enumerate(arr):
    # i 是索引, val 是值
```

### While 循环

```java
// Java
while (condition) {
    // ...
}
```

```python
# Python
while condition:
    # ...
```

---

## 11. 条件判断

```java
// Java
if (condition) {
    // ...
} else if (condition2) {
    // ...
} else {
    // ...
}

// 三元运算符
int max = (a > b) ? a : b;
```

```python
# Python
if condition:
    # ...
elif condition2:
    # ...
else:
    # ...

# 三元运算符
max = a if a > b else b
```

---

## 12. 函数/方法

### Java

```java
// 方法
public int methodName(int[] nums, String s) {
    return 0;
}

// 可变参数
public int sum(int... nums) {
    int total = 0;
    for (int num : nums) {
        total += num;
    }
    return total;
}
```

### Python

```python
# 函数
def method_name(nums: List[int], s: str) -> int:
    return 0

# 可变参数
def sum(*nums):
    total = 0
    for num in nums:
        total += num
    return total
```

---

## 13. 类

### Java

```java
class Solution {
    private int val;

    public Solution() {
        // 构造函数
    }

    public int method() {
        return this.val;
    }

    public static void staticMethod() {
        // 静态方法
    }
}
```

### Python

```python
class Solution:
    def __init__(self):
        # 构造函数
        self.val = 0

    def method(self):
        return self.val

    @staticmethod
    def static_method():
        # 静态方法
        pass
```

---

## 14. 输入输出

### Java

```java
import java.util.Scanner;

Scanner sc = new Scanner(System.in);
int n = sc.nextInt();
String s = sc.nextLine();
sc.close();

System.out.println("Hello");
System.out.printf("%d %s\n", 123, "abc");
```

### Python

```python
n = int(input())
s = input().strip()

print("Hello")
print(f"{123} {abc}")
```

---

## 15. 常用工具函数对照

| 功能 | Java | Python |
|------|------|--------|
| 最大值 | `Math.max(a, b)` | `max(a, b)` |
| 最小值 | `Math.min(a, b)` | `min(a, b)` |
| 绝对值 | `Math.abs(x)` | `abs(x)` |
| 幂运算 | `Math.pow(a, b)` | `a ** b` 或 `pow(a, b)` |
| 平方根 | `Math.sqrt(x)` | `x ** 0.5` 或 `math.sqrt(x)` |
| 向下取整 | `(int)Math.floor(x)` | `int(x)` 或 `math.floor(x)` |
| 向上取整 | `(int)Math.ceil(x)` | `math.ceil(x)` |
| 四舍五入 | `Math.round(x)` | `round(x)` |
| 随机数 | `Math.random()` | `random.random()` |

---

## 16. 快捷差异总结

| 特性 | Java | Python |
|------|------|--------|
| 类型声明 | 必须 | 可选（类型提示） |
| 语句结束 | 分号 `;` | 换行 |
| 代码块 | 大括号 `{}` | 缩进 |
| 数组越界 | 抛出异常 | 自动切片处理 |
| 字符串不可变 | 是 | 是 |
| 列表切片 | 不支持 | 支持 |
| 链式比较 | 不支持 | 支持 (`1 < x < 5`) |
| 多返回值 | 需要封装对象 | 原生支持 `return a, b` |
| 运算符重载 | 不支持 | 支持 |
