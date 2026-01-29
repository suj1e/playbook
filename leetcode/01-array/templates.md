# 数组代码模板

数组是力扣中最基础的数据结构，掌握其操作模板非常重要。

---

## 1. 双指针模板

### 1.1 相向双指针（对撞指针）

**适用场景**：有序数组、两数之和、回文判断

```
原数组: [1, 2, 3, 4, 5, 6]
         ^           ^
        left       right

特点: 两端向中间移动
```

#### Java
```java
int left = 0, right = arr.length - 1;
while (left < right) {
    int sum = arr[left] + arr[right];
    if (sum == target) {
        return new int[]{left, right};
    } else if (sum < target) {
        left++;    // 需要更大的和，左指针右移
    } else {
        right--;   // 需要更小的和，右指针左移
    }
}
```

#### Python
```python
left, right = 0, len(arr) - 1
while left < right:
    total = arr[left] + arr[right]
    if total == target:
        return [left, right]
    elif total < target:
        left += 1
    else:
        right -= 1
```

---

### 1.2 同向双指针（快慢指针）

**适用场景**：删除元素、原地修改数组

```
原数组: [1, 2, 2, 3, 2, 4]
         ^  ^
        slow fast

移动后: [1, 2, 3, 4, _, _]
                 ^        ^
               slow     fast

特点: fast 探路，slow 跟踪有效位置
```

#### Java
```java
int slow = 0;
for (int fast = 0; fast < arr.length; fast++) {
    if (arr[fast] != val) {  // 保留条件
        arr[slow] = arr[fast];
        slow++;
    }
}
return slow;  // 新数组长度
```

#### Python
```python
slow = 0
for fast in range(len(arr)):
    if arr[fast] != val:  # 保留条件
        arr[slow] = arr[fast]
        slow += 1
return slow
```

---

## 2. 滑动窗口模板

### 2.1 固定大小窗口

**适用场景**：固定长度的子数组问题

```
窗口大小 k = 3
原数组: [1, 2, 3, 4, 5, 6]
         [│ 2  3  4 │]  <-- 窗口
           ↑     ↑
         left  right

滑动后: [1, 2, 3, 4, 5, 6]
            [│ 3  4  5 │]
```

#### Java
```java
int k = 3;  // 窗口大小
int maxSum = Integer.MIN_VALUE;
int windowSum = 0;

for (int i = 0; i < arr.length; i++) {
    windowSum += arr[i];  // 进入窗口

    if (i >= k) {
        windowSum -= arr[i - k];  // 离开窗口
    }

    if (i >= k - 1) {
        maxSum = Math.max(maxSum, windowSum);  // 更新答案
    }
}
```

#### Python
```python
k = 3  # 窗口大小
max_sum = float('-inf')
window_sum = 0

for i in range(len(arr)):
    window_sum += arr[i]  # 进入窗口

    if i >= k:
        window_sum -= arr[i - k]  # 离开窗口

    if i >= k - 1:
        max_sum = max(max_sum, window_sum)  # 更新答案
```

---

### 2.2 可变大小窗口

**适用场景**：最长/最短子数组、满足条件的子数组

```
窗口大小可变
原数组: [1, 2, 3, 1, 2, 4, 2]
         [│ 2  3  1 │]  满足条件
         [│ 2  3  1  2  4 │]  扩大
               [│ 3  1  2 │]  收缩

特点: 根据条件动态调整窗口大小
```

#### Java
```java
int left = 0;
int maxLength = 0;

for (int right = 0; right < arr.length; right++) {
    // 1. 进入窗口
    window.add(arr[right]);

    // 2. 窗口收缩（根据条件）
    while (需要收缩的条件) {
        window.remove(arr[left]);
        left++;
    }

    // 3. 更新答案
    maxLength = Math.max(maxLength, right - left + 1);
}
```

#### Python
```python
left = 0
max_length = 0

for right in range(len(arr)):
    # 1. 进入窗口
    window.add(arr[right])

    # 2. 窗口收缩
    while 需要收缩的条件:
        window.remove(arr[left])
        left += 1

    # 3. 更新答案
    max_length = max(max_length, right - left + 1)
```

---

## 3. 二分查找模板

### 3.1 标准二分查找

```
有序数组: [1, 2, 3, 4, 5, 6, 7], target = 4

第一轮: [1, 2, 3, 4, 5, 6, 7]
              ^mid=4
         left        right

找到目标，直接返回 mid
```

#### Java
```java
int left = 0, right = arr.length - 1;
while (left <= right) {
    int mid = left + (right - left) / 2;  // 防止溢出
    if (arr[mid] == target) {
        return mid;      // 找到目标
    } else if (arr[mid] < target) {
        left = mid + 1;  // 在右半区
    } else {
        right = mid - 1; // 在左半区
    }
}
return -1;  // 未找到
```

#### Python
```python
left, right = 0, len(arr) - 1
while left <= right:
    mid = left + (right - left) // 2
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        left = mid + 1
    else:
        right = mid - 1
return -1
```

---

### 3.2 查找左边界（第一个 ≥ target）

**适用场景**：寻找插入位置

```
数组: [1, 2, 2, 2, 3], target = 2
                        ^-- 返回第一个 2 的位置

查找范围: [0, 5)  <-- right 是开区间
```

#### Java
```java
int left = 0, right = arr.length;  // right 是开区间
while (left < right) {
    int mid = left + (right - left) / 2;
    if (arr[mid] < target) {
        left = mid + 1;
    } else {
        right = mid;  // 收缩右边界
    }
}
return left;  // 返回第一个 ≥ target 的位置
```

#### Python
```python
left, right = 0, len(arr)  # right 是开区间
while left < right:
    mid = left + (right - left) // 2
    if arr[mid] < target:
        left = mid + 1
    else:
        right = mid
return left
```

---

## 4. 前缀和模板

**适用场景**：频繁查询子数组和

```
原数组: [1, 2, 3, 4, 5]
索引:    0  1  2  3  4

前缀和:  [0, 1, 3, 6, 10, 15]
索引:     0  1  2  3  4   5

查询 [1, 3] 的和: prefix[4] - prefix[1] = 10 - 1 = 9
              2 + 3 + 4 = 9 ✓
```

#### Java
```java
// 构建前缀和数组
int[] prefix = new int[arr.length + 1];
for (int i = 0; i < arr.length; i++) {
    prefix[i + 1] = prefix[i] + arr[i];
}

// 查询子数组 [i, j] 的和
int sum = prefix[j + 1] - prefix[i];
```

#### Python
```python
# 构建前缀和数组
prefix = [0] * (len(arr) + 1)
for i in range(len(arr)):
    prefix[i + 1] = prefix[i] + arr[i]

# 查询子数组 [i, j] 的和
sum_ij = prefix[j + 1] - prefix[i]
```

---

## 5. 复杂度总结

| 操作 | 时间复杂度 | 空间复杂度 |
|------|-----------|-----------|
| 双指针遍历 | O(n) | O(1) |
| 滑动窗口 | O(n) | O(k) |
| 二分查找 | O(log n) | O(1) |
| 前缀和构建 | O(n) | O(n) |
| 前缀和查询 | O(1) | O(1) |
