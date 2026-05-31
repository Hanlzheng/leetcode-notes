# 350. Intersection of Two Arrays II

## 题目

给你两个整数数组 `nums1` 和 `nums2`，返回它们的**交集**。

结果中每个元素出现的次数，应与元素在两个数组中都出现的**最小次数**一致。可以**不考虑输出顺序**。

**例子**：
```
nums1 = [1, 2, 2, 1], nums2 = [2, 2]       → [2, 2]
nums1 = [4, 9, 5],    nums2 = [9, 4, 9, 8, 4]  → [4, 9] 或 [9, 4]
```

## 思路

**关键理解**：交集里每个数出现的次数 = `min(nums1 中出现次数, nums2 中出现次数)`

**哈希表法**：
1. 用 `d1` 统计 `nums1` 每个数的次数
2. 用 `d2` 统计 `nums2` 每个数的次数
3. 遍历 d1，对每个 key：如果在 d2 里，取 `min(d1[k], d2[k])` 作为输出次数
4. 把 (k, count) 展开成 list

## 我踩的坑

1. **变量名打错**：第二个循环写了 `d1[j] = 1` 而不是 `d2[j] = 1`，导致 nums2 的新数字都跑去加到 d1 里，d2 漏掉，后面访问 `d2[k]` 时 `KeyError`。**教训**：d1/d2/d3 这种命名很容易抄错，可以改成 `count1`、`count2` 更清楚。

2. **逻辑错：用判等代替 min**：第一版写了 `if k in d2 and d1[k] == d2[k]`，只取频率相等的情况。但交集应该是**取较小频率**。比如 `nums1=[1,1,2]`, `nums2=[1,2,2]`，正确是 `[1, 2]`，我那版会返回空。

3. **KeyError 的根源**：`d1[k] == d2[k]` 这种判断如果 k 不在 d2 里就会崩。要么用 `k in d2 and ...`（短路求值），要么用 `d2.get(k, 0)`。

## 解法

### 解法 1：双哈希表（我写的）

```python
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        d1 = {}
        d2 = {}
        d3 = {}
        for i in nums1:
            if i in d1:
                d1[i] += 1
            else:
                d1[i] = 1
        for j in nums2:
            if j in d2:
                d2[j] += 1
            else:
                d2[j] = 1
        for k in d1:
            if k in d2:
                d3[k] = min(d1[k], d2[k])
        result = [k for k, v in d3.items() for _ in range(v)]
        return result
```

### 解法 2：单哈希表（空间优化）

只统计较短的数组，遍历较长的边查边减：

```python
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        if len(nums1) > len(nums2):
            nums1, nums2 = nums2, nums1
        
        d = {}
        for x in nums1:
            d[x] = d.get(x, 0) + 1
        
        result = []
        for x in nums2:
            if d.get(x, 0) > 0:
                result.append(x)
                d[x] -= 1
        return result
```

**思路**：d 维护"还能匹配几次"。nums2 里每出现一次，就消耗一次库存。

### 解法 3：Counter（终极一行）

```python
from collections import Counter

class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list((Counter(nums1) & Counter(nums2)).elements())
```

- `Counter & Counter` → 自动取较小频率（交集语义）
- `.elements()` → 按次数展开

### 解法 4：排序 + 双指针

如果数组已经有序（或可以排序），不用哈希也行：

```python
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        nums1.sort()
        nums2.sort()
        i = j = 0
        result = []
        while i < len(nums1) and j < len(nums2):
            if nums1[i] < nums2[j]:
                i += 1
            elif nums1[i] > nums2[j]:
                j += 1
            else:
                result.append(nums1[i])
                i += 1
                j += 1
        return result
```

排序后双指针扫描，匹配就 append。

## 复杂度

| 解法 | 时间 | 空间 |
|---|---|---|
| 双哈希表 | O(m + n) | O(m + n) |
| 单哈希表 ⭐ | O(m + n) | O(min(m, n)) |
| Counter | O(m + n) | O(m + n) |
| 排序 + 双指针 | O(m log m + n log n) | O(1)*（不算排序栈） |

## 进阶问题（面试常问）

1. **如果数组已排序怎么办？** → 用解法 4（双指针），空间 O(1)
2. **如果 nums1 比 nums2 小很多，且 nums2 在磁盘上读不进内存？** → 用解法 2，只哈希 nums1，nums2 边读边匹配
3. **如果输入是流数据呢？** → 哈希表 + 实时输出
