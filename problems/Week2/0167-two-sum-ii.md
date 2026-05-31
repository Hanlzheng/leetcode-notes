# 167. Two Sum II - Input Array Is Sorted

## 题目

给你一个**已按非递减顺序排列**的整数数组 `numbers`，找出两个数使它们的和等于 `target`。

返回这两个数的**下标**（**1-indexed**，从 1 开始数）。

**保证**：恰好有一组解。

**例子**：
```
numbers = [2, 7, 11, 15], target = 9   → [1, 2]   (2 + 7 = 9)
numbers = [2, 3, 4],      target = 6   → [1, 3]   (2 + 4 = 6)
```

## 思路

题目关键提示：**数组已排序**。这是给双指针准备的。

**双指针法**：
- `left = 0`（指向最小值），`right = n - 1`（指向最大值）
- 每次看 `numbers[left] + numbers[right]`：
  - **等于 target** → 找到答案 ✅
  - **小于 target** → 和太小，`left++`（让和变大）
  - **大于 target** → 和太大，`right--`（让和变小）

**为什么对**：数组有序，所以方向明确 —— `left++` 必让和变大，`right--` 必让和变小，不会错过任何可能解。

**举例** `numbers = [2, 7, 11, 15]`, `target = 9`：
```
left=0(2), right=3(15)  →  2+15=17 > 9, right--
left=0(2), right=2(11)  →  2+11=13 > 9, right--
left=0(2), right=1(7)   →  2+7=9   ✅ 返回 [1, 2]
```

## 我踩的坑

1. **用了暴力 O(n²) 导致 TLE**：第一版用双层循环遍历所有 (i, j) 对，超时。没意识到"已排序"是个强力提示。

2. **循环条件 `<=` 还是 `<`**：写过 `while left <= right`，但 `left == right` 时是在用同一个元素加自己（题目要求两个不同位置）。应该是 `while left < right`。

3. **下标忘了 +1**：题目要求 **1-indexed**，所以返回 `[left + 1, right + 1]`，不是 `[left, right]`。

## 解法

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left = 0
        right = len(numbers) - 1
        while left < right:
            s = numbers[left] + numbers[right]
            if s < target:
                left += 1
            elif s > target:
                right -= 1
            else:
                return [left + 1, right + 1]
```

### 对比：哈希表法（O(n) 但 O(n) 空间）

如果数组**没排序**（比如 LeetCode 1. Two Sum），就用哈希：

```python
def twoSum(self, nums, target):
    d = {}
    for i, num in enumerate(nums):
        if target - num in d:
            return [d[target - num], i]
        d[num] = i
```

但这题已排序，**双指针 O(1) 空间更优**。

## 复杂度

| 解法 | 时间 | 空间 |
|---|---|---|
| 暴力 | O(n²) ❌ TLE | O(1) |
| 哈希表 | O(n) | O(n) |
| 双指针 ⭐ | O(n) | **O(1)** |

## 关键收获

**看到"已排序数组"，第一反应想双指针**。这是这道题最重要的模式识别 🎯
