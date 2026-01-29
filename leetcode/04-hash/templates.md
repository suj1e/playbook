# 哈希表代码模板

哈希表是力扣中最常用的数据结构之一，能够在 O(1) 时间复杂度内完成查找、插入和删除操作。

---

## 1. 基本操作

### 1.1 初始化

#### Java
```java
// HashMap
Map<Integer, Integer> map = new HashMap<>();

// HashSet
Set<Integer> set = new HashSet<>();

// LinkedHashMap（保持插入顺序）
Map<K, V> map = new LinkedHashMap<>();

// TreeMap（键有序）
Map<K, V> map = new TreeMap<>();
```

#### Python
```python
# dict
hash_map = {}

# set
hash_set = set()

# defaultdict（带默认值）
from collections import defaultdict
hash_map = defaultdict(int)
```

---

### 1.2 计数问题

**适用场景**：统计元素出现次数

```
原数组: [1, 2, 3, 2, 1, 3, 3]

统计:   {1: 2, 2: 2, 3: 3}

查找:   count[3] = 3  (元素 3 出现 3 次)
```

#### Java
```java
Map<Integer, Integer> count = new HashMap<>();
for (int num : nums) {
    count.put(num, count.getOrDefault(num, 0) + 1);
}

// 获取出现次数
int frequency = count.get(num);
```

#### Python
```python
from collections import Counter

# 方法1：手动计数
count = {}
for num in nums:
    count[num] = count.get(num, 0) + 1

# 方法2：使用 Counter（推荐）
count = Counter(nums)
```

---

### 1.3 两数之和

**适用场景**：查找互补数

```
数组: [2, 7, 11, 15], target = 9

i=0: num=2, complement=7
     map中没有7，存入 {2: 0}

i=1: num=7, complement=2
     map中有2，返回 [0, 1]
```

#### Java
```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();

    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }
        map.put(nums[i], i);  // 先查后存
    }
    return new int[]{};
}
```

#### Python
```python
def two_sum(nums, target):
    num_map = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in num_map:
            return [num_map[complement], i]
        num_map[num] = i
    return []
```

---

## 2. 前缀和 + 哈希表

**适用场景**：和为 k 的子数组问题

```
数组: [1, 2, 3, 3, 2], k = 6

前缀和: [0, 1, 3, 6, 9, 11]
索引:    0  1  2  3  4   5

查找和为6的子数组:
- prefix[3] - prefix[0] = 6 -> [0,2] 即 [1,2,3]
- prefix[4] - prefix[2] = 6 -> [2,3] 即 [3,3]
```

#### Java
```java
public int subarraySum(int[] nums, int k) {
    Map<Integer, Integer> prefixCount = new HashMap<>();
    prefixCount.put(0, 1);  // 前缀和为 0 的情况

    int sum = 0, count = 0;

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

#### Python
```python
def subarray_sum(nums, k):
    from collections import defaultdict

    prefix_count = defaultdict(int)
    prefix_count[0] = 1  # 前缀和为 0 的情况

    total = count = 0

    for num in nums:
        total += num
        count += prefix_count[total - k]
        prefix_count[total] += 1

    return count
```

---

## 3. 分组问题

**适用场景**：字母异位词分组

```
输入: ["eat", "tea", "tan", "ate", "nat", "bat"]

分组:
  "aet" -> ["eat", "tea", "ate"]
  "ant" -> ["tan", "nat"]
  "abt" -> ["bat"]

关键: 排序后的字符串作为 key
```

#### Java
```java
public List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> groups = new HashMap<>();

    for (String s : strs) {
        char[] chars = s.toCharArray();
        Arrays.sort(chars);
        String key = new String(chars);

        groups.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
    }

    return new ArrayList<>(groups.values());
}
```

#### Python
```python
from collections import defaultdict

def group_anagrams(strs):
    groups = defaultdict(list)

    for s in strs:
        key = ''.join(sorted(s))
        groups[key].append(s)

    return list(groups.values())
```

---

## 4. 复杂度总结

| 操作 | HashMap | HashSet |
|------|---------|---------|
| 插入 | O(1) | O(1) |
| 删除 | O(1) | O(1) |
| 查找 | O(1) | O(1) |
| 遍历 | O(n) | O(n) |

**注意**：平均 O(1)，最坏 O(n)（哈希冲突严重时）
