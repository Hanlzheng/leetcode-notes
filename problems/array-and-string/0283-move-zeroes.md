# 283. Move Zeros 

## 题目
- Given a integer array nums, move all o's to the end of it while maintaining the relative order of the non-zero elements. 

## 特点
- Must do this in-place without making a copy of the array

## 思路
- `i` (fast pointer) — scans every element in the array
- `j` (slow pointer) — marks the next position where a non-zero element should go

**Step 1 — collect the non-zero elements to the front.**
As `i` scans the array, whenever `nums[i]` is non-zero, place it at `nums[j]`
and move `j` forward by one. Zeros are skipped. After this pass, the first
`j` positions hold all the non-zero elements in their original order.

**Step 2 — fill the rest with zeros.**
From index `j` to the end of the array, set every position to 0.

## 我踩的坑
- 在函数里,给参数名做 nums = ... 的重新赋值,只影响函数内部那个名字,影响不到函数外面。但通过参数去修改它指向的对象内容(参数[i] = ...、参数.append() 等),改的是真实对象,函数外面看得到。
- 不要一边遍历一个列表、一边改它的长度。要用一个独立的新列表,或者用双指针原地做。


## 解法
```Python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        count0 = 0
        j = 0
        for i in range(len(nums)):
            if nums[i] != 0:
                nums[j] = nums[i]
                j += 1 
            else: 
                count0 += 1
        for k in range(count0):
            nums[j] = 0
            j += 1
```
## 复杂度
- Time: O(n)
- Space: O(1)