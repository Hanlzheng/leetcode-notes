# 35. Search Insert Position

## 题目 
Given a sorted array of integers nums and a target value, return the index of target if it exists in the array. If it does not, return the index where it would be inserted in order

## Approach
- Same binary search as problem 704. The only difference is what we return after the loop.
- 704 returns -1 when the target is not found; here we return `left` instead, which is the position where the target should be inserted.

## 我踩的坑

## 解法
```Python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        n = len(nums)
        left = 0
        right = n - 1
        while left <= right:
            middle = (left + right) // 2
            if target < nums[middle]:
                right = middle - 1
            elif target > nums[middle]:
                left = middle + 1
            else:
                return middle
        return left
```
## 复杂度