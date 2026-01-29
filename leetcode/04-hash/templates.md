# 哈希表代码模板

哈希表是力扣中最常用的数据结构之一，能够在 O(1) 时间复杂度内完成查找、插入和删除操作。

---

## 1. 哈希表定义和初始化

### Java

```java
import java.util.*;

// HashMap：键值对映射
Map<K, V> map = new HashMap<>();
Map<String, Integer> map = new HashMap<>();

// HashSet：去重集合
Set<T> set = new HashSet<>();
Set<Integer> set = new HashSet<>();

// LinkedHashMap：保持插入顺序
Map<K, V> map = new LinkedHashMap<>();

// TreeMap：键有序
Map<K, V> map = new TreeMap<>();
```

### Python

```python
# dict：键值对映射
hash_map = {}
hash_map = dict()

# set：去重集合
hash_set = set()
hash_set = {1, 2, 3}

# OrderedDict：保持插入顺序（Python 3.7+ dict 默认有序）
from collections import OrderedDict
ordered_dict = OrderedDict()

# defaultdict：带默认值的字典
from collections import defaultdict
default_dict = defaultdict(int)
default_dict = defaultdict(list)
```

---

## 2. 基本操作模板

### 2.1 添加和获取元素

#### Java

```java
Map<Integer, Integer> map = new HashMap<>();

// 添加元素
map.put(key, value);

// 获取元素
if (map.containsKey(key)) {
    int value = map.get(key);
}

// 获取元素，不存在时返回默认值
int value = map.getOrDefault(key, defaultValue);

// 不存在时添加并返回默认值
int value = map.computeIfAbsent(key, k -> 0);
```

#### Python

```python
hash_map = {}

# 添加元素
hash_map[key] = value

# 获取元素
if key in hash_map:
    value = hash_map[key]

# 获取元素，不存在时返回默认值
value = hash_map.get(key, default_value)

# 不存在时添加并返回默认值
from collections import defaultdict
hash_map = defaultdict(int)
value = hash_map[key]  # 不存在时自动为 0
```

---

### 2.2 遍历哈希表

#### Java

```java
Map<String, Integer> map = new HashMap<>();

// 遍历键值对
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    String key = entry.getKey();
    int value = entry.getValue();
    // 处理 key, value
}

// 遍历键
for (String key : map.keySet()) {
    // 处理 key
}

// 遍历值
for (int value : map.values()) {
    // 处理 value
}

// Lambda 遍历
map.forEach((key, value) -> {
    // 处理 key, value
});
```

#### Python

```python
hash_map = {}

# 遍历键值对
for key, value in hash_map.items():
    # 处理 key, value

# 遍历键
for key in hash_map.keys():
    # 处理 key

# 遍历值
for value in hash_map.values():
    # 处理 value
```

---

## 3. 计数问题模板

### 3.1 统计元素出现次数

#### Java

```java
int[] nums = {1, 2, 3, 2, 1, 3, 3};
Map<Integer, Integer> count = new HashMap<>();

for (int num : nums) {
    count.put(num, count.getOrDefault(num, 0) + 1);
}

// 或使用 Map.merge
for (int num : nums) {
    count.merge(num, 1, Integer::sum);
}

// 或使用 computeIfAbsent
for (int num : nums) {
    count.put(num, count.getOrDefault(num, 0) + 1);
}
```

#### Python

```python
nums = [1, 2, 3, 2, 1, 3, 3]

# 方法1：手动计数
count = {}
for num in nums:
    count[num] = count.get(num, 0) + 1

# 方法2：使用 defaultdict
from collections import defaultdict
count = defaultdict(int)
for num in nums:
    count[num] += 1

# 方法3：使用 Counter（最简洁）
from collections import Counter
count = Counter(nums)
```

---

### 3.2 找出出现次数最多的元素

#### Java

```java
Map<Integer, Integer> count = new HashMap<>();
// ... 填充 count

// 找出出现次数最多的元素
int maxCount = 0;
int mostFrequent = -1;
for (Map.Entry<Integer, Integer> entry : count.entrySet()) {
    if (entry.getValue() > maxCount) {
        maxCount = entry.getValue();
        mostFrequent = entry.getKey();
    }
}

// 或使用堆找出 Top K
PriorityQueue<Map.Entry<Integer, Integer>> heap =
    new PriorityQueue<>((a, b) -> a.getValue() - b.getValue());
for (Map.Entry<Integer, Integer> entry : count.entrySet()) {
    heap.offer(entry);
    if (heap.size() > k) {
        heap.poll();
    }
}
```

#### Python

```python
from collections import Counter

count = Counter(nums)

# 找出出现次数最多的元素
most_frequent = count.most_common(1)[0][0]

# 找出 Top K
top_k = count.most_common(k)
```

---

## 4. 前缀和 + 哈希表模板

**适用场景**：和为 k 的子数组问题

### Java

```java
public int subarraySum(int[] nums, int k) {
    Map<Integer, Integer> prefixCount = new HashMap<>();
    prefixCount.put(0, 1);  // 前缀和为 0 的情况

    int sum = 0;
    int count = 0;

    for (int num : nums) {
        sum += num;  // 当前前缀和

        // 检查是否存在 sum - k
        count += prefixCount.getOrDefault(sum - k, 0);

        // 记录当前前缀和
        prefixCount.put(sum, prefixCount.getOrDefault(sum, 0) + 1);
    }

    return count;
}
```

### Python

```python
def subarray_sum(nums, k):
    from collections import defaultdict

    prefix_count = defaultdict(int)
    prefix_count[0] = 1  # 前缀和为 0 的情况

    total = 0
    count = 0

    for num in nums:
        total += num  # 当前前缀和

        # 检查是否存在 total - k
        count += prefix_count[total - k]

        # 记录当前前缀和
        prefix_count[total] += 1

    return count
```

---

## 5. 分组问题模板

**适用场景**：字母异位词分组等

### Java

```java
// 使用排序后的字符串作为 key
Map<String, List<String>> groups = new HashMap<>();

for (String str : strs) {
    char[] chars = str.toCharArray();
    Arrays.sort(chars);
    String key = new String(chars);

    groups.computeIfAbsent(key, k -> new ArrayList<>()).add(str);
}

return new ArrayList<>(groups.values());
```

### Python

```python
from collections import defaultdict

groups = defaultdict(list)

for s in strs:
    # 使用排序后的字符串作为 key
    key = ''.join(sorted(s))
    groups[key].append(s)

return list(groups.values())
```

---

## 6. 去重和存在性判断

### 6.1 去重

#### Java

```java
// 使用 HashSet 去重
Set<Integer> set = new HashSet<>();
for (int num : nums) {
    set.add(num);
}

// 去重后的数组
int[] unique = set.stream().mapToInt(Integer::intValue).toArray();

// 或使用 LinkedHashSet 保持顺序
Set<Integer> linkedSet = new LinkedHashSet<>();
for (int num : nums) {
    linkedSet.add(num);
}
```

#### Python

```python
# 使用 set 去重
unique = list(set(nums))

# 保持顺序去重
unique = list(dict.fromkeys(nums))
```

---

### 6.2 存在性判断

#### Java

```java
Set<Integer> set = new HashSet<>();

// 添加元素
set.add(num);

// 判断是否存在
if (set.contains(target)) {
    // 找到目标
}

// 删除元素
set.remove(num);
```

#### Python

```python
hash_set = set()

# 添加元素
hash_set.add(num)

# 判断是否存在
if target in hash_set:
    # 找到目标

# 删除元素
hash_set.discard(num)  # 不存在不会报错
hash_set.remove(num)   # 不存在会报错
```

---

## 7. 两数之和模板

### Java

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();  // 数值 -> 索引

    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];

        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }

        map.put(nums[i], i);  // 先查后存，避免自己匹配自己
    }

    return new int[]{};
}
```

### Python

```python
def two_sum(nums, target):
    num_map = {}  # 数值 -> 索引

    for i, num in enumerate(nums):
        complement = target - num

        if complement in num_map:
            return [num_map[complement], i]

        num_map[num] = i  # 先查后存

    return []
```

---

## 8. 复杂度总结

| 操作 | HashMap | HashSet | TreeMap | TreeSet |
|------|---------|---------|---------|---------|
| 插入 | O(1) | O(1) | O(log n) | O(log n) |
| 删除 | O(1) | O(1) | O(log n) | O(log n) |
| 查找 | O(1) | O(1) | O(log n) | O(log n) |
| 遍历 | O(n) | O(n) | O(n) | O(n) |

**说明**：
- HashMap/HashSet：平均 O(1)，最坏 O(n)（哈希冲突严重时）
- TreeMap/TreeSet：基于红黑树，保证 O(log n) 且有序

---

## 9. 常见陷阱

| 陷阱 | 说明 | 解决方案 |
|------|------|----------|
| 键的类型 | 使用自定义类作为键时，必须正确实现 hashCode() 和 equals() | 使用 Integer、String 等标准类型，或正确实现方法 |
| null 处理 | HashMap 允许一个 null 键和多个 null 值，TreeMap 不允许 | 注意判断 null |
| 并发修改 | 遍历时修改会抛出 ConcurrentModificationException | 使用 Iterator.remove() 或创建副本 |
| 初始容量 | 频繁扩容影响性能 | 合理设置初始容量：`new HashMap<>(expectedSize * 4 / 3 + 1)` |
| 内存消耗 | 哈希表占用空间较大 | 数据量小时考虑其他数据结构 |

---

## 10. 选择合适的哈希表结构

| 场景 | 推荐结构 |
|------|----------|
| 普通键值对映射 | HashMap / dict |
| 需要有序 | TreeMap / OrderedDict |
| 需要默认值 | defaultdict / computeIfAbsent |
| 去重 | HashSet / set |
| 需要排序去重 | TreeSet / sorted(set) |
| 计数问题 | Counter / HashMap + merge |
| LRU 缓存 | LinkedHashMap / OrderedDict |
