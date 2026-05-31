# 169. Majority Element

## 题目

给定一个大小为 n 的数组 `nums`，找出其中的**多数元素**。

多数元素指在数组中出现次数**大于 ⌊n/2⌋** 的元素。

**假设**：数组非空，且多数元素**总是存在**。

**例子**：
```
nums = [3, 2, 3]              → 3
nums = [2, 2, 1, 1, 1, 2, 2]  → 2  (出现 4 次 > 7/2)
```

## 思路

**哈希表法**：统计每个元素的出现次数，找出第一个次数 > n/2 的元素。

1. 遍历 `nums`，用字典 `d` 记录每个数字出现的次数
2. 再遍历字典，找出 value > len(nums) // 2 的 key 返回

也可以**边数边判断**，一旦达到阈值立即返回，省一次遍历。

## 我踩的坑

1. **判断条件用 `>` 不是 `>=`**：题目要求出现次数**大于** n/2，所以是 `d[i] > len(nums) // 2`，不是 `>=`。

2. **整数除法用 `//`**：`len(nums) / 2` 在 Python 3 里是浮点数（比如 3.5），用 `//` 才得到整数。

## 解法

### 解法 1：哈希表（基础版）

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        d = {}
        for num in nums:
            if num in d:
                d[num] += 1
            else:
                d[num] = 1
        for i in d:
            if d[i] > len(nums) // 2:
                return i
```

### 解法 2：哈希表（边数边判断）

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        d = {}
        for num in nums:
            d[num] = d.get(num, 0) + 1
            if d[num] > len(nums) // 2:
                return num
```

### 解法 3：摩尔投票法 ⭐ O(1) 空间

**核心思想**：不同数字互相"抵消"，因为多数派 > n/2，最后一定剩下多数派。

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        count = 0
        candidate = None
        for num in nums:
            if count == 0:
                candidate = num
            count += 1 if num == candidate else -1
        return candidate
```

**举例** `nums = [2, 2, 1, 1, 1, 2, 2]`：
```
num  candidate  count
2    2          1     (count=0，换 2 上场)
2    2          2     (相同，+1)
1    2          1     (不同，-1)
1    2          0     (不同，-1，归零)
1    1          1     (count=0，换 1 上场)
2    1          0     (不同，-1，归零)
2    2          1     (count=0，换 2 上场)
→ return 2 ✅
```

## 复杂度

| 解法 | 时间 | 空间 |
|---|---|---|
| 哈希表 | O(n) | O(n) |
| 摩尔投票 | O(n) | **O(1)** ⭐ |
