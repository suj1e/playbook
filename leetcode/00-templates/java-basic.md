# Java 基础模板

力扣刷题 Java 常用代码片段和技巧。

---

## 1. 基本框架

### 力扣方法签名

```java
class Solution {
    // 返回单个值
    public int methodName(int[] nums) {
        return 0;
    }

    // 返回数组
    public int[] methodName(int n) {
        return new int[]{};
    }

    // 返回 List
    public List<Integer> methodName(int n) {
        return new ArrayList<>();
    }

    // 返回二维数组
    public int[][] methodName(int n) {
        return new int[][]{};
    }
}
```

---

## 2. 数组操作

### 2.1 创建与初始化

```java
// 创建固定大小数组
int[] arr = new int[10];
int[][] matrix = new int[3][4];

// 创建并初始化
int[] arr = {1, 2, 3};
int[] arr = new int[]{1, 2, 3};

// 动态数组 List
List<Integer> list = new ArrayList<>();
List<Integer> list = new ArrayList<>(100); // 指定初始容量

// 数组转 List
List<Integer> list = new ArrayList<>();
for (int num : arr) list.add(num);

// List 转数组
int[] arr = list.stream().mapToInt(i -> i).toArray();
Integer[] arr = list.toArray(new Integer[0]);
```

### 2.2 常用操作

```java
// 数组长度
int n = arr.length;

// 数组排序
Arrays.sort(arr);                    // 升序
Arrays.sort(arr, 0, n);              // 排序 [0, n)
Arrays.sort(arr, (a, b) -> b - a);   // 降序

// 二分查找（返回索引，不存在返回 -插入点-1）
int idx = Arrays.binarySearch(arr, target);

// 填充
Arrays.fill(arr, -1);

// 复制
int[] copy = Arrays.copyOf(arr, arr.length);
int[] copy = arr.clone();

// 比较内容
Arrays.equals(arr1, arr2);
```

### 2.3 二维数组

```java
// 行列数
int rows = matrix.length;
int cols = matrix[0].length;

// 遍历
for (int i = 0; i < rows; i++) {
    for (int j = 0; j < cols; j++) {
        // matrix[i][j]
    }
}

// 按行排序
Arrays.sort(matrix, (a, b) -> a[0] - b[0]);
```

---

## 3. 字符串操作

### 3.1 创建与基本操作

```java
// 创建
String s = "hello";
String s = new String("hello");

// 长度
int n = s.length();

// 字符访问（不能直接 s[i]）
char c = s.charAt(i);

// 子串
String sub = s.substring(start);          // [start, end)
String sub = s.substring(start, end);

// 转字符数组
char[] chars = s.toCharArray();

// 字符数组转字符串
String s = new String(chars);
String s = String.valueOf(chars);
```

### 3.2 常用方法

```java
// 查找
int idx = s.indexOf("abc");           // 首次出现位置，不存在返回 -1
int idx = s.lastIndexOf("abc");       // 最后出现位置

// 判断
boolean contains = s.contains("abc");
boolean starts = s.startsWith("abc");
boolean ends = s.endsWith("abc");
boolean empty = s.isEmpty();

// 替换
String s2 = s.replace('a', 'b');
String s2 = s.replaceAll("\\s+", ""); // 正则替换

// 分割
String[] parts = s.split(",");
String[] parts = s.split("\\s+");     // 正则分割

// 大小写
String s2 = s.toLowerCase();
String s2 = s.toUpperCase();

// 去除首尾空格
String s2 = s.trim();

// 反转（需要 StringBuilder）
String s2 = new StringBuilder(s).reverse().toString();
```

### 3.3 StringBuilder（可变字符串）

```java
StringBuilder sb = new StringBuilder();
sb.append("hello");
sb.append(123);
sb.insert(0, "xyz");
sb.delete(0, 2);
sb.reverse();
String result = sb.toString();
```

---

## 4. 集合框架

### 4.1 List

```java
List<Integer> list = new ArrayList<>();

// 添加
list.add(1);
list.add(0, 2);           // 在指定位置插入

// 访问
int val = list.get(i);

// 修改
list.set(i, 10);

// 删除
list.remove(i);           // 按索引
list.remove(Integer.valueOf(5)); // 按值

// 大小
int n = list.size();

// 排序
Collections.sort(list);
Collections.sort(list, (a, b) -> b - a); // 降序
Collections.sort(list, (a, b) -> {
    // 自定义比较
    return a.val - b.val;
});

// 检查
boolean empty = list.isEmpty();
boolean has = list.contains(5);

// 转数组
int[] arr = list.stream().mapToInt(i -> i).toArray();
```

### 4.2 Set

```java
// HashSet（无序）
Set<Integer> set = new HashSet<>();

// LinkedHashSet（插入顺序）
Set<Integer> set = new LinkedHashSet<>();

// TreeSet（有序）
Set<Integer> set = new TreeSet<>();

// 常用操作
set.add(1);
set.remove(1);
boolean has = set.contains(1);
int n = set.size();
boolean empty = set.isEmpty();

// 遍历
for (int num : set) {
    // ...
}
```

### 4.3 Map

```java
// HashMap
Map<String, Integer> map = new HashMap<>();

// TreeMap（按键排序）
Map<String, Integer> map = new TreeMap<>();

// 添加/修改
map.put("a", 1);
map.putIfAbsent("a", 2);

// 获取
Integer val = map.get("a");
Integer val = map.getOrDefault("a", 0); // 不存在返回默认值

// 删除
map.remove("a");

// 检查
boolean has = map.containsKey("a");
boolean has = map.containsValue(1);

// 遍历
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    String key = entry.getKey();
    Integer val = entry.getValue();
}

// 只遍历 key/value
for (String key : map.keySet()) { }
for (Integer val : map.values()) { }

// 大小
int n = map.size();
```

### 4.4 Queue

```java
// LinkedList 作为 Queue
Queue<Integer> queue = new LinkedList<>();

// PriorityQueue（最小堆）
Queue<Integer> pq = new PriorityQueue<>();

// PriorityQueue（最大堆）
Queue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);

// 队列操作
queue.offer(1);    // 入队，返回 boolean
queue.poll();      // 出队，队列为空返回 null
queue.peek();      // 查看队首，为空返回 null

int n = queue.size();
boolean empty = queue.isEmpty();
```

### 4.5 Deque（双端队列）

```java
// LinkedList 作为 Deque
Deque<Integer> deque = new LinkedList<>();

// ArrayDeque（更高效，但 null 值有限制）
Deque<Integer> deque = new ArrayDeque<>();

// 队列操作
deque.offerFirst(1);   // 队首入队
deque.offerLast(1);    // 队尾入队
deque.pollFirst();     // 队首出队
deque.pollLast();      // 队尾出队
deque.peekFirst();     // 查看队首
deque.peekLast();      // 查看队尾

// 栈操作（推荐用 Deque 代替 Stack）
deque.push(1);     // 栈顶压入（等价于 addFirst）
deque.pop();       // 栈顶弹出（等价于 removeFirst）
deque.peek();      // 查看栈顶（等价于 peekFirst）
```

---

## 5. 栈

```java
// 推荐：使用 Deque 代替 Stack
Deque<Integer> stack = new ArrayDeque<>();

stack.push(1);      // 压栈
stack.pop();        // 弹栈，空栈抛出异常
stack.peek();       // 查看栈顶
boolean empty = stack.isEmpty();
int n = stack.size();

// 不推荐：Stack 类（遗留类）
Stack<Integer> stack = new Stack<>();
```

---

## 6. 常用工具类

### 6.1 Math

```java
Math.max(a, b);
Math.min(a, b);
Math.abs(x);
Math.pow(a, b);      // a^b
Math.sqrt(x);
Math.cbrt(x);        // 立方根
Math.ceil(x);        // 向上取整
Math.floor(x);       // 向下取整
Math.round(x);       // 四舍五入

// 常量
Math.PI;
Math.E;

// 随机数
double rand = Math.random(); // [0.0, 1.0)
int rand = (int)(Math.random() * n); // [0, n)
```

### 6.2 Integer

```java
// 字符串转数字
int n = Integer.parseInt("123");
int n = Integer.valueOf("123");

// 数字转字符串
String s = String.valueOf(123);
String s = Integer.toString(123);

// 进制转换
String binary = Integer.toBinaryString(n);   // 二进制字符串
String hex = Integer.toHexString(n);         // 十六进制字符串
int n = Integer.parseInt("1010", 2);         // 二进制转十进制

// 常量
Integer.MAX_VALUE;  // 2147483647
Integer.MIN_VALUE;  // -2147483648
```

### 6.3 Character

```java
// 判断
Character.isDigit('5');
Character.isLetter('a');
Character.isUpperCase('A');
Character.isLowerCase('a');
Character.isWhitespace(' ');

// 转换
char c = Character.toUpperCase('a');
char c = Character.toLowerCase('A');
int n = Character.getNumericValue('5'); // 字符转数字

// 常用
int diff = 'a' - 'A'; // 32
char c = (char)('a' + 1); // 'b'
```

---

## 7. 输入输出（本地测试）

### 7.1 Scanner 输入

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // 读取单个整数
        int n = sc.nextInt();

        // 读取一行字符串
        String line = sc.nextLine();

        // 读取单词（以空白分隔）
        String word = sc.next();

        // 读取单个字符
        char c = sc.next().charAt(0);

        // 读取数组（一行，空格分隔）
        String[] parts = sc.nextLine().split(" ");
        int[] arr = new int[parts.length];
        for (int i = 0; i < parts.length; i++) {
            arr[i] = Integer.parseInt(parts[i]);
        }

        sc.close();
    }
}
```

### 7.2 快速输出

```java
import java.io.PrintWriter;

public class Main {
    public static void main(String[] args) {
        PrintWriter out = new PrintWriter(System.out);

        out.println("Hello");
        out.printf("%d %s%n", 123, "abc");

        out.flush(); // 刷新缓冲区
    }
}
```

---

## 8. 常用技巧

### 8.1 交换

```java
// 数组元素交换
int temp = arr[i];
arr[i] = arr[j];
arr[j] = temp;

// 不用临时变量（异或，仅限整数）
arr[i] ^= arr[j];
arr[j] ^= arr[i];
arr[i] ^= arr[j];
```

### 8.2 数组填充

```java
// 一维数组
Arrays.fill(arr, -1);

// 二维数组
for (int[] row : matrix) {
    Arrays.fill(row, -1);
}
```

### 8.3 深拷贝

```java
// 一维数组
int[] copy = arr.clone();

// 二维数组
int[][] copy = new int[matrix.length][];
for (int i = 0; i < matrix.length; i++) {
    copy[i] = matrix[i].clone();
}
```

### 8.4 反转数组

```java
// 原地反转
int left = 0, right = arr.length - 1;
while (left < right) {
    int temp = arr[left];
    arr[left++] = arr[right];
    arr[right--] = temp;
}
```

---

## 9. 位运算技巧

```java
// 判断奇偶
boolean isOdd = (n & 1) == 1;
boolean isEven = (n & 1) == 0;

// 判断是否为 2 的幂
boolean isPowerOfTwo = (n > 0) && ((n & (n - 1)) == 0);

// 获取最低位的 1
int lowbit = n & (-n);

// 交换两数（异或）
a ^= b;
b ^= a;
a ^= b;
```

---

## 10. 比较器

### 10.1 对象排序

```java
// 排序数组
Arrays.sort(arr, (a, b) -> a - b);  // 升序
Arrays.sort(arr, (a, b) -> b - a);  // 降序

// 排序 List
Collections.sort(list, (a, b) -> {
    // 多条件排序
    if (a.val != b.val) {
        return a.val - b.val;
    }
    return a.index - b.index;
});
```

### 10.2 优先队列自定义

```java
// 最小堆（默认）
Queue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);

// 最大堆
Queue<int[]> pq = new PriorityQueue<>((a, b) -> b[0] - a[0]);

// 多条件
Queue<int[]> pq = new PriorityQueue<>((a, b) -> {
    if (a[0] != b[0]) return a[0] - b[0];
    return a[1] - b[1];
});
```
