# 496. Next Greater Element I / 下一个更大元素 I

[中文链接](https://leetcode.cn/problems/next-greater-element-i/) | [English](https://leetcode.com/problems/next-greater-element-i/)

**难度**：Easy
**标签**：栈 | 数组 | 哈希表 | 单调栈
**相似题目**：[lc-739](./lc-739-daily-temperatures.md) | [lc-503](./) | [lc-496](./)
**面试频次**：⭐⭐⭐

---

## 题目描述

`nums1` 中数字 `x` 的 下一个更大元素 是指 `x` 在 `nums2` 中对应位置 右侧 的 第一个 比 `x` 大的元素。

给你两个 没有重复元素 的数组 `nums1` 和 `nums2` ，下标从 0 开始计数，其中`nums1` 是 `nums2` 的子集。

对于每个 `0 <= i < nums1.length` ，找出满足 `nums1[i] == nums2[j]` 的下标 `j` ，并且在 `nums2` 确定 `nums2[j]` 的 下一个更大元素 。如果不存在下一个更大元素，那么本次查询的答案是 `-1` 。

返回一个长度为 `nums1.length` 的数组 `ans` 作为答案，满足 `ans[i]` 是如上所述的 下一个更大元素 。

**示例 1**：
```
输入：nums1 = [4,1,2], nums2 = [1,3,4,2]
输出：[-1,3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：
- 4 ，用加粗斜体标识，nums2 = [1,3,**4**,2]。不存在下一个更大元素，所以答案是 -1 。
- 1 ，用加粗斜体标识，nums2 = [**1**,3,4,2]。下一个更大元素是 3 。
- 2 ，用加粗斜体标识，nums2 = [1,3,4,**2**]。不存在下一个更大元素，所以答案是 -1 。
```

**示例 2**：
```
输入：nums1 = [2,4], nums2 = [1,2,3,4]
输出：[3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：
- 2 ，用加粗斜体标识，nums2 = [1,**2**,3,4]。下一个更大元素是 3 。
- 4 ，用加粗斜体标识，nums2 = [1,2,3,**4**]。不存在下一个更大元素，所以答案是 -1 。
```

**约束条件**：
- 1 <= nums1.length <= nums2.length <= 1000
- 0 <= nums1[i], nums2[i] <= 10⁴
- nums1 和 nums2 中所有整数 互不相同
- nums1 中的所有整数同样出现在 nums2 中

**进阶**：你可以设计一个时间复杂度为 O(nums1.length + nums2.length) 的解决方案吗？

---

## 解题思路

### 方法一：暴力枚举

**思路**：
对于 nums1 中的每个元素，在 nums2 中找到位置，然后向右找第一个更大的元素。

**复杂度**：
- 时间：O(n × m)
- 空间：O(1)

---

### 方法二：单调栈 + 哈希表（推荐）

**思路**：
1. 先用单调栈预处理 nums2，找出每个元素的下一个更大元素
2. 用哈希表存储结果（元素 -> 下一个更大元素）
3. 遍历 nums1，从哈希表中查询答案

**复杂度**：
- 时间：O(n + m)
- 空间：O(n)

**步骤**：
1. 遍历 nums2，使用单调递减栈
2. 当前元素 > 栈顶时，栈顶找到"下一个更大元素"
3. 存入哈希表：stackTop -> curr
4. 遍历 nums1，从哈希表查询

---

## 代码实现

### Java

```java
class Solution {
    // 方法一：暴力 O(n × m)
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int n = nums1.length, m = nums2.length;
        int[] answer = new int[n];

        for (int i = 0; i < n; i++) {
            int j = 0;
            // 在 nums2 中找到 nums1[i] 的位置
            while (j < m && nums2[j] != nums1[i]) {
                j++;
            }
            // 从 j+1 开始找第一个更大的元素
            while (j < m && nums2[j] <= nums1[i]) {
                j++;
            }
            answer[i] = j < m ? nums2[j] : -1;
        }

        return answer;
    }

    // 方法二：单调栈 + 哈希表 O(n + m)
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        // 1. 预处理 nums2
        Map<Integer, Integer> nextGreater = new HashMap<>();
        Deque<Integer> stack = new ArrayDeque<>();  // 单调递减栈

        for (int num : nums2) {
            while (!stack.isEmpty() && num > stack.peek()) {
                nextGreater.put(stack.pop(), num);
            }
            stack.push(num);
        }

        // 栈中剩余元素没有下一个更大元素
        while (!stack.isEmpty()) {
            nextGreater.put(stack.pop(), -1);
        }

        // 2. 查询 nums1
        int[] answer = new int[nums1.length];
        for (int i = 0; i < nums1.length; i++) {
            answer[i] = nextGreater.get(nums1[i]);
        }

        return answer;
    }
}
```

### Python

```python
from typing import List

class Solution:
    # 方法一：暴力 O(n × m)
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        answer = []

        for num1 in nums1:
            # 在 nums2 中找到 num1 的位置
            idx = nums2.index(num1)
            # 从 idx+1 开始找第一个更大的元素
            found = False
            for num2 in nums2[idx + 1:]:
                if num2 > num1:
                    answer.append(num2)
                    found = True
                    break
            if not found:
                answer.append(-1)

        return answer

    # 方法二：单调栈 + 哈希表 O(n + m)
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        # 1. 预处理 nums2
        next_greater = {}
        stack = []  # 单调递减栈

        for num in nums2:
            while stack and num > stack[-1]:
                next_greater[stack.pop()] = num
            stack.append(num)

        # 栈中剩余元素没有下一个更大元素
        while stack:
            next_greater[stack.pop()] = -1

        # 2. 查询 nums1
        return [next_greater[num] for num in nums1]
```

---

## 关键点

| 关键点 | 说明 |
|-------|------|
| 单调递减栈 | 栈中元素从底到顶递减 |
| 预处理思想 | 先统一处理 nums2，存入哈希表，然后快速查询 |
| 哈希表存储 | 元素 -> 下一个更大元素 |
| 栈中元素 | 遍历后仍留在栈中的元素没有下一个更大元素 |
| 一次遍历 | 对 nums2 只需遍历一次，建立完整的映射关系 |

---

## 图解

```
nums1 = [4, 1, 2]
nums2 = [1, 3, 4, 2]

预处理 nums2（单调递减栈）:

num=1: stack=[1]
num=3: 3>1, 弹出1, map[1]=3, stack=[3]
num=4: 4>3, 弹出3, map[3]=4, stack=[4]
num=2: 2<4, stack=[4,2]

遍历结束，栈中剩余元素没有下一个更大元素:
map[4]=-1, map[2]=-1

哈希表: {1: 3, 3: 4, 4: -1, 2: -1}

查询 nums1:
4 -> -1
1 -> 3
2 -> -1

结果: [-1, 3, -1]
```

---

## 变种/延伸

### 变种1：下一个更大元素 II（循环数组）
- **题目**：[lc-503 下一个更大元素 II](./)
- **变化**：数组是循环的
- **解法**：遍历 2n 次，对 n 取模

### 变种2：每日温度
- **题目**：[lc-739 每日温度](./lc-739-daily-temperatures.md)
- **变化**：返回的是距离而非具体值
- **解法**：单调栈存储索引

### 变种3：下一个更大元素 III
- **题目**：[lc-556 下一个更大元素 III](./)
- **变化**：给定一个整数，找比它大的最小整数（数字排列）
- **解法**：数学方法

### 进阶思考

1. **如果 nums1 和 nums2 有重复元素怎么办？**
   - 题目保证没有重复元素
   - 如果有重复，哈希表存储会冲突，需要改为存储索引列表

2. **为什么单调递减栈？**
   - 我们要找"下一个更大元素"
   - 新元素 > 栈顶时，栈顶找到了答案

3. **如果要从前往后找"上一个更大元素"？**
   - 使用单调递减栈
   - 栈中元素就是"上一个更大元素"

4. **空间复杂度能否优化？**
   - 哈希表最多存储 n 个键值对
   - 如果 nums1 很小，可以只存 nums1 中的元素

5. **如何验证单调栈的正确性？**
   - 每个元素入栈一次
   - 当有更大元素出现时，立即出栈并记录
   - 栈中保留的元素都是"还没找到更大元素"的
