# 704 Binary Search

## 题目
Given a sorted array of integers nums and a target value, return the index of target if it exists in the array, or -1 if it does not.

## Approach
- Since the array is sorted, we can use binary search.
- Create two pointers, `left` and `right`, pointing to the start and the end of the array.
- Compute the middle index, then compare the target with `nums[middle]`.
- If the target is greater than `nums[middle]`, it must be located in the second half of the array, so move the left pointer to `middle + 1`.
- If the target is smaller than `nums[middle]`, it must be located in the first half of the array, so move the right pointer to `middle - 1`.
- If the target equals `nums[middle]`, return `middle`.
- If `left > right` and the loop ends without finding the target, it is not in the array, so return -1.

## 我踩的坑
1. 用切片做二分。 这是总根源。nums[middle:right+1] 切出新数组,导致后面一连串问题。教训:二分不要切片,移动 left/right 下标。
2. 索引偏移。 切片的直接后果——子数组里找到的下标不是原数组的下标,返回值对不上。
递归返回值没 return。 self.search(...) 算完结果被丢掉。
3. 死循环 / 边界没排除 middle。 right = middle 写成这样,middle 还留在范围里,范围缩不动。正确是 middle - 1 / middle + 1。
4. right 初值越界。 right = n 配 nums[middle] 会摸到 nums[n]。right 代表「最后一个合法下标」就该是 n - 1。
5. 用 if target not in nums 打补丁。 这个坑最隐蔽——它让代码 AC 了,但 not in 是 O(n),掩盖了循环里的真 bug,还把二分的 O(log n) 优势抵消掉。「能 AC」不等于「写对了」。
## 解法
``` Python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        n = len(nums)
        left = 0
        right = n - 1
        while left <= right:
            middle = (left + right) // 2
            if target < nums[middle]:
                right = middle - 1
            elif target > nums[middle]:
                left = middle + 1
```
## 复杂度
- 时间 O(n^2)
- 空间 O(1)