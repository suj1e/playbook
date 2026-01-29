# 数组代码模板

数组是力扣中最基础的数据结构，掌握其操作模板非常重要。

---

## 1. 基础操作模板

### 1.1 遍历数组

#### Java
```java
// 索引遍历
for (int i = 0; i < arr.length; i++) {
    int val = arr[i];
    // 处理 arr[i]
}

// 增强 for 循环
for (int val : arr) {
    // 处理 val
}
```

#### Python
```python
# 索引遍历
for i in range(len(arr)):
    val = arr[i]
    # 处理 arr[i]

# 直接遍历
for val in arr:
    # 处理 val

# 带索引遍历
for i, val in enumerate(arr):
    # i 是索引, val 是值
```

---

### 1.2 数组反转

#### Java
```java
// 原地反转
int left = 0, right = arr.length - 1;
while (left < right) {
    int temp = arr[left];
    arr[left] = arr[right];
    arr[right] = temp;
    left++;
    right--;
}

// 使用库
int[] reversed = arr.clone(); // 先复制
for (int i = 0; i < reversed.length / 2; i++) {
    int temp = reversed[i];
    reversed[i] = reversed[reversed.length - 1 - i];
    reversed[reversed.length - 1 - i] = temp;
}
```

#### Python
```python
# 切片反转（最简洁）
reversed = arr[::-1]

# 原地反转
arr.reverse()

# 手动反转
left, right = 0, len(arr) - 1
while left < right:
    arr[left], arr[right] = arr[right], arr[left]
    left += 1
    right -= 1
```

---

## 2. 双指针模板

### 2.1 相向双指针（对撞指针）

**适用场景**：有序数组、两数之和、回文判断

#### Java
```java
int left = 0, right = arr.length - 1;
while (left < right) {
    int sum = arr[left] + arr[right];
    if (sum == target) {
        // 找到目标
        return new int[]{left, right};
    } else if (sum < target) {
        left++;  // 需要更大的和
    } else {
        right--; // 需要更小的和
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

### 2.2 同向双指针（快慢指针）

**适用场景**：删除元素、原地修改数组

#### Java
```java
// 慢指针指向有效位置，快指针遍历
int slow = 0;
for (int fast = 0; fast < arr.length; fast++) {
    if (arr[fast] != val) {  // 保留条件
        arr[slow] = arr[fast];
        slow++;
    }
}
// slow 是新数组的长度
return slow;
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

## 3. 滑动窗口模板

### 3.1 固定大小窗口

**适用场景**：固定长度的子数组问题

#### Java
```java
int k = 3;  // 窗口大小
int maxSum = Integer.MIN_VALUE;
int windowSum = 0;

for (int i = 0; i < arr.length; i++) {
    // 进入窗口
    windowSum += arr[i];

    // 窗口满了，开始滑
    if (i >= k) {
        // 离开窗口
        windowSum -= arr[i - k];
    }

    // 更新答案
    if (i >= k - 1) {
        maxSum = Math.max(maxSum, windowSum);
    }
}
```

#### Python
```python
k = 3  # 窗口大小
max_sum = float('-inf')
window_sum = 0

for i in range(len(arr)):
    # 进入窗口
    window_sum += arr[i]

    # 窗口满了，开始滑
    if i >= k:
        # 离开窗口
        window_sum -= arr[i - k]

    # 更新答案
    if i >= k - 1:
        max_sum = max(max_sum, window_sum)
```

---

### 3.2 可变大小窗口

**适用场景**：最长/最短子数组、满足条件的子数组

#### Java
```java
int left = 0;
int maxLength = 0;

for (int right = 0; right < arr.length; right++) {
    // 1. 进入窗口：处理 arr[right]
    window.add(arr[right]);

    // 2. 窗口收缩条件
    while (window需要收缩的条件) {
        // 离开窗口：移除 arr[left]
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
    # 1. 进入窗口：处理 arr[right]
    window.add(arr[right])

    # 2. 窗口收缩条件
    while window需要收缩的条件:
        # 离开窗口：移除 arr[left]
        window.remove(arr[left])
        left += 1

    # 3. 更新答案
    max_length = max(max_length, right - left + 1)
```

---

## 4. 二分查找模板

### 4.1 标准二分查找

#### Java
```java
int left = 0, right = arr.length - 1;
while (left <= right) {
    int mid = left + (right - left) / 2;  // 防止溢出
    if (arr[mid] == target) {
        return mid;  // 找到目标
    } else if (arr[mid] < target) {
        left = mid + 1;
    } else {
        right = mid - 1;
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

### 4.2 查找左边界（第一个 ≥ target）

#### Java
```java
int left = 0, right = arr.length;
while (left < right) {
    int mid = left + (right - left) / 2;
    if (arr[mid] < target) {
        left = mid + 1;
    } else {
        right = mid;
    }
}
return left;  // 返回第一个 ≥ target 的位置
```

#### Python
```python
left, right = 0, len(arr)
while left < right:
    mid = left + (right - left) // 2
    if arr[mid] < target:
        left = mid + 1
    else:
        right = mid
return left
```

---

### 4.3 查找右边界（最后一个 ≤ target）

#### Java
```java
int left = 0, right = arr.length;
while (left < right) {
    int mid = left + (right - left) / 2;
    if (arr[mid] <= target) {
        left = mid + 1;
    } else {
        right = mid;
    }
}
return right - 1;  // 返回最后一个 ≤ target 的位置
```

#### Python
```python
left, right = 0, len(arr)
while left < right:
    mid = left + (right - left) // 2
    if arr[mid] <= target:
        left = mid + 1
    else:
        right = mid
return right - 1
```

---

## 5. 常用技巧

### 5.1 前缀和

**适用场景**：频繁查询子数组和

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

### 5.2 数组去重（有序数组）

#### Java
```java
// 返回去重后的长度
int slow = 1;
for (int fast = 1; fast < arr.length; fast++) {
    if (arr[fast] != arr[fast - 1]) {
        arr[slow] = arr[fast];
        slow++;
    }
}
return slow;
```

#### Python
```python
slow = 1
for fast in range(1, len(arr)):
    if arr[fast] != arr[fast - 1]:
        arr[slow] = arr[fast]
        slow += 1
return slow
```

---

### 5.3 荷兰国旗问题（三路划分）

#### Java
```java
// 将数组分为：[0, zero) = 0, [zero, i) = 1, [two, n) = 2
int zero = 0, two = arr.length;
int i = 0;
while (i < two) {
    if (arr[i] == 0) {
        swap(arr, zero++, i++);
    } else if (arr[i] == 1) {
        i++;
    } else {  // arr[i] == 2
        swap(arr, i, --two);
    }
}

private void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```

#### Python
```python
zero, two = 0, len(arr)
i = 0
while i < two:
    if arr[i] == 0:
        arr[zero], arr[i] = arr[i], arr[zero]
        zero += 1
        i += 1
    elif arr[i] == 1:
        i += 1
    else:  # arr[i] == 2
        two -= 1
        arr[i], arr[two] = arr[two], arr[i]
```

---

## 6. 复杂度总结

| 操作 | 时间复杂度 | 空间复杂度 |
|------|-----------|-----------|
| 访问元素 | O(1) | O(1) |
| 顺序查找 | O(n) | O(1) |
| 二分查找（有序） | O(log n) | O(1) |
| 插入/删除（平均） | O(n) | O(1) |
| 双指针遍历 | O(n) | O(1) |
| 滑动窗口 | O(n) | O(k)，k 为窗口大小 |
