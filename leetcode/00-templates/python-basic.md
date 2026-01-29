# Python 基础模板

力扣刷题 Python 常用代码片段和技巧。

---

## 1. 基本框架

### 力扣方法签名

```python
from typing import List

class Solution:
    # 返回单个值
    def method_name(self, nums: List[int]) -> int:
        return 0

    # 返回列表
    def method_name(self, n: int) -> List[int]:
        return []

    # 返回二维列表
    def method_name(self, n: int) -> List[List[int]]:
        return []
```

---

## 2. 数组（列表）操作

### 2.1 创建与初始化

```python
# 创建列表
arr = [1, 2, 3]
arr = list(range(10))
arr = [0] * 10           # [0, 0, 0, ...]
arr = [i for i in range(10)]  # 列表推导式

# 二维列表（注意不要用 [[0]*n]*m，会有引用问题）
matrix = [[0] * n for _ in range(m)]

# 列表转数组（需要 numpy）
# import numpy as np
# arr = np.array(list1)
```

### 2.2 常用操作

```python
# 长度
n = len(arr)

# 访问和修改
val = arr[i]
arr[i] = 10

# 切片（左闭右开）
arr[start:end]         # [start, end)
arr[start:]            # [start, 最后]
arr[:end]              # [0, end)
arr[:]                 # 全部（用于复制）
arr[::-1]              # 反转

# 添加元素
arr.append(5)          # 末尾添加
arr.insert(0, 5)       # 指定位置插入
arr.extend([4, 5])     # 扩展列表

# 删除元素
arr.pop()              # 弹出最后一个
arr.pop(i)             # 弹出指定位置
arr.remove(5)          # 删除第一个值为5的元素
del arr[i]             # 删除指定位置

# 查找
idx = arr.index(5)     # 查找元素索引，不存在会报错
val = arr.count(5)     # 统计出现次数
has = 5 in arr         # 判断是否存在

# 排序
arr.sort()             # 原地排序（升序）
arr.sort(reverse=True) # 原地排序（降序）
sorted_arr = sorted(arr)  # 返回新列表

# 反转
arr.reverse()          # 原地反转
reversed_arr = arr[::-1]  # 返回新列表
```

### 2.3 二维列表

```python
# 行列数
rows = len(matrix)
cols = len(matrix[0])

# 遍历
for i in range(rows):
    for j in range(cols):
        val = matrix[i][j]

# 按某一列排序
matrix.sort(key=lambda x: x[0])

# 获取列
col = [row[i] for row in matrix]
```

---

## 3. 字符串操作

### 3.1 创建与基本操作

```python
# 创建
s = "hello"
s = str(123)

# 长度
n = len(s)

# 访问字符
c = s[i]

# 切片
sub = s[start:end]
sub = s[start:]
sub = s[:end]

# 转列表
chars = list(s)

# 列表转字符串
s = ''.join(chars)
s = ''.join(['a', 'b', 'c'])  # "abc"
s = ','.join(['a', 'b', 'c']) # "a,b,c"
```

### 3.2 常用方法

```python
# 查找
idx = s.find("abc")      # 首次出现位置，不存在返回 -1
idx = s.rfind("abc")     # 最后出现位置
idx = s.index("abc")     # 首次出现位置，不存在抛出异常

# 判断
has = "abc" in s
starts = s.startswith("abc")
ends = s.endswith("abc")
empty = len(s) == 0
is_empty = not s  # 空字符串为 False

# 替换
s2 = s.replace('a', 'b')
s2 = s.replace(" ", "")   # 删除空格

# 分割
parts = s.split(',')
parts = s.split()         # 按空白字符分割
lines = s.split('\n')

# 大小写
s2 = s.lower()
s2 = s.upper()
s2 = s.capitalize()       # 首字母大写
s2 = s.title()            # 每个单词首字母大写

# 去除首尾空格
s2 = s.strip()
s2 = s.lstrip()           # 去除左侧
s2 = s.rstrip()           # 去除右侧

# 判断类型
s.isalpha()               # 是否全是字母
s.isdigit()               # 是否全是数字
s.isalnum()               # 是否是字母或数字
s.isspace()               # 是否是空格

# 对齐
s2 = s.ljust(10, ' ')     # 左对齐
s2 = s.rjust(10, ' ')     # 右对齐
s2 = s.center(10, ' ')    # 居中
```

### 3.3 正则表达式

```python
import re

# 匹配
pattern = r'\d+'
matches = re.findall(pattern, s)  # 返回所有匹配
match = re.search(pattern, s)     # 返回第一个匹配对象

# 替换
s2 = re.sub(r'\s+', '', s)        # 删除所有空白

# 分割
parts = re.split(r'[,\s]+', s)    # 按逗号或空格分割
```

---

## 4. 字典

### 4.1 创建与基本操作

```python
# 创建
d = {}
d = {'a': 1, 'b': 2}
d = dict(a=1, b=2)

# 带默认值的字典
from collections import defaultdict
d = defaultdict(int)      # 默认值为 0
d = defaultdict(list)     # 默认值为 []
d = defaultdict(lambda: 0)  # 自定义默认值

# 访问
val = d['key']
val = d.get('key', 0)     # 不存在返回默认值

# 添加/修改
d['key'] = value
d.update({'a': 1, 'b': 2})

# 删除
del d['key']
val = d.pop('key')        # 删除并返回值
val = d.pop('key', 0)     # 不存在返回默认值
d.clear()                 # 清空

# 检查
has = 'key' in d
has = 'key' in d.keys()
has = value in d.values()

# 遍历
for key in d:
    pass

for key, value in d.items():
    pass

for key in d.keys():
    pass

for value in d.values():
    pass

# 大小
n = len(d)
```

### 4.2 Counter（计数器）

```python
from collections import Counter

# 统计元素频率
counter = Counter([1, 2, 2, 3, 3, 3])
# Counter({3: 3, 2: 2, 1: 1})

# 获取最常见的元素
counter.most_common(2)  # [(3, 3), (2, 2)]

# 统计字符串字符频率
counter = Counter("hello")
# Counter({'l': 2, 'h': 1, 'e': 1, 'o': 1})
```

---

## 5. 集合

### 5.1 创建与基本操作

```python
# 创建
s = set()
s = {1, 2, 3}

# 添加
s.add(1)
s.update([2, 3, 4])

# 删除
s.remove(1)             # 不存在会报错
s.discard(1)            # 不存在不报错
val = s.pop()           # 弹出任意元素
s.clear()

# 检查
has = 1 in s

# 遍历
for val in s:
    pass

# 大小
n = len(s)
```

### 5.2 集合运算

```python
a = {1, 2, 3}
b = {3, 4, 5}

# 并集
union = a | b
union = a.union(b)

# 交集
inter = a & b
inter = a.intersection(b)

# 差集
diff = a - b
diff = a.difference(b)

# 对称差集
sym = a ^ b
sym = a.symmetric_difference(b)

# 子集/超集
is_subset = a.issubset(b)     # a <= b
is_superset = a.issuperset(b) # a >= b
```

---

## 6. 栈与队列

### 6.1 栈（用列表实现）

```python
stack = []

# 压栈
stack.append(1)

# 弹栈
val = stack.pop()

# 查看栈顶
val = stack[-1]

# 判断空
empty = len(stack) == 0
empty = not stack
```

### 6.2 队列

```python
from collections import deque

queue = deque()

# 入队
queue.append(1)

# 出队
val = queue.popleft()

# 查看队首
val = queue[0]

# 判断空
empty = len(queue) == 0
```

### 6.3 双端队列

```python
from collections import deque

dq = deque()

# 右端操作
dq.append(1)        # 右端添加
val = dq.pop()       # 右端弹出

# 左端操作
dq.appendleft(1)     # 左端添加
val = dq.popleft()   # 左端弹出

# 查看
val = dq[0]          # 队首
val = dq[-1]         # 队尾

# 旋转
dq.rotate(1)         # 右移1位
dq.rotate(-1)        # 左移1位
```

---

## 7. 堆

### 7.1 基本操作

```python
import heapq

# 创建堆（最小堆）
heap = []
heapq.heappush(heap, 3)
heapq.heappush(heap, 1)
heapq.heappush(heap, 2)

# 弹出最小值
val = heapq.heappop(heap)

# 查看最小值
val = heap[0]

# 列表转堆
arr = [3, 1, 2]
heapq.heapify(arr)   # 原地转堆
```

### 7.2 最大堆

```python
import heapq

# 最大堆技巧：存负数
heap = []
heapq.heappush(heap, -3)
heapq.heappush(heap, -1)
max_val = -heapq.heappop(heap)
```

### 7.3 常用操作

```python
# 获取前 n 大/小的元素
n_largest = heapq.nlargest(3, heap)
n_smallest = heapq.nsmallest(3, heap)

# 替换堆顶元素（更高效）
heapq.heapreplace(heap, new_value)  # 弹出最小，插入新值
```

---

## 8. 常用工具

### 8.1 排序

```python
# 列表排序
arr.sort()                  # 原地升序
arr.sort(reverse=True)      # 原地降序
sorted_arr = sorted(arr)    # 返回新列表

# 按绝对值排序
arr.sort(key=abs)

# 按多个键排序
arr.sort(key=lambda x: (x[0], -x[1]))

# 字典按键/值排序
sorted_dict = sorted(d.items(), key=lambda x: x[0])  # 按键
sorted_dict = sorted(d.items(), key=lambda x: x[1])  # 按值
```

### 8.2 数学函数

```python
import math

# 基本运算
max(a, b)
min(a, b)
abs(x)
pow(a, b)          # a^b
math.sqrt(x)
math.floor(x)       # 向下取整
math.ceil(x)        # 向上取整
round(x)            # 四舍五入

# 常量
math.pi
math.e

# 三角函数
math.sin(x)
math.cos(x)
math.tan(x)

# 对数
math.log(x)         # 自然对数
math.log10(x)       # 以10为底
```

### 8.3 进制转换

```python
# 十进制转其他
binary = bin(10)        # '0b1010'
octal = oct(10)         # '0o12'
hexadecimal = hex(10)   # '0xa'

# 其他转十进制
decimal = int('1010', 2)    # 二进制转十进制
decimal = int('a', 16)      # 十六进制转十进制
```

---

## 9. 输入输出（本地测试）

### 9.1 标准输入

```python
# 读取单个整数
n = int(input())

# 读取一行字符串
s = input().strip()

# 读取多个整数（空格分隔）
arr = list(map(int, input().split()))

# 读取多行
lines = [input().strip() for _ in range(n)]

# 快速读取大量数据
import sys
data = sys.stdin.read().split()
```

### 9.2 标准输出

```python
# 打印
print("Hello")
print(123, 456)       # "123 456"
print("a", "b", sep=',')  # "a,b"

# 格式化输出
print(f"Value: {x}")
print("Value: {}".format(x))
print("%d %s" % (123, "abc"))

# 刷新缓冲区
print("...", flush=True)
```

---

## 10. 常用技巧

### 10.1 交换

```python
# Python 风格交换
a, b = b, a

# 数组元素交换
arr[i], arr[j] = arr[j], arr[i]
```

### 10.2 列表推导式

```python
# 基本用法
squares = [x**2 for x in range(10)]

# 带条件
evens = [x for x in range(10) if x % 2 == 0]

# 嵌套
matrix = [[i*j for j in range(3)] for i in range(3)]
```

### 10.3 字典推导式

```python
# 基本用法
squares = {x: x**2 for x in range(10)}

# 带条件
evens = {x: x**2 for x in range(10) if x % 2 == 0}
```

### 10.4 Lambda 函数

```python
# 排序
arr.sort(key=lambda x: x[1])

# 自定义条件
arr.sort(key=lambda x: (x[0], -x[1]))

# map/filter
result = list(map(lambda x: x*2, arr))
result = list(filter(lambda x: x > 0, arr))
```

---

## 11. 位运算技巧

```python
# 判断奇偶
is_odd = (n & 1) == 1
is_even = (n & 1) == 0

# 判断是否为 2 的幂
is_power_of_two = n > 0 and (n & (n - 1)) == 0

# 获取最低位的 1
lowbit = n & (-n)

# 交换两数（异或）
a ^= b
b ^= a
a ^= b
```

---

## 12. 装饰器与类方法

### 12.1 类方法

```python
class Solution:
    # 实例方法
    def method(self, x):
        return x

    # 类方法
    @classmethod
    def class_method(cls, x):
        return x

    # 静态方法
    @staticmethod
    def static_method(x):
        return x
```

### 12.2 LRU 缓存（力扣常用）

```python
from functools import lru_cache

class Solution:
    @lru_cache(maxsize=None)
    def fib(self, n: int) -> int:
        if n < 2:
            return n
        return self.fib(n - 1) + self.fib(n - 2)
```
