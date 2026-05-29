# 34. Find First and Last Position of Element in Sorted Array

## 题目

给定一个升序排列的整数数组 `nums` 和目标值 `target`，找出 `target` 在数组中的开始和结束位置。  
如果不存在，返回 `[-1, -1]`。

## 思路

先用二分搜索找到任意一个等于 `target` 的位置 `middle`，再从 `middle` 向左右两边扩展，找到边界 `l` 和 `r`。

## 我踩的坑

**坑1：`range(left, right)` 少了 +1**  
找到边界后用 `list(range(left, right))` 返回，但 `range` 不包含右端点，导致 `nums = [1], target = 1` 时返回 `[]` 而不是 `[0, 0]`。  
应该用 `range(left, right + 1)`，或者直接 `return [l, r]`。

**坑2：return 写成了 `[left, right]` 而不是 `[l, r]`**  
`left` / `right` 是二分搜索的指针，`l` / `r` 才是向两边扩展后的目标边界，两者不是同一个变量。

## 解法

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        left = 0
        right = len(nums) - 1
        while left <= right:
            middle = (left + right) // 2
            if nums[middle] < target:
                left = middle + 1
            elif nums[middle] > target:
                right = middle - 1
            else:
                l, r = middle, middle
                while l > 0 and nums[l - 1] == target:
                    l -= 1
                while r < len(nums) - 1 and nums[r + 1] == target:
                    r += 1
                return [l, r]
        return [-1, -1]
```

## 复杂度

| | 复杂度 |
|---|---|
| 时间 | O(log n + k)，k 为 target 出现次数 |
| 空间 | O(1) |
