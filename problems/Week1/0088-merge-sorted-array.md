# 88. Merge Sorted Array

## 题目
- 给两个有序数组 nums1 和 nums2。nums1 的长度是 m+n —— 前 m 个是有效元素,后 n 个是占位的 0。nums2 有 n 个元素。要把两个数组合并成一个有序数组,原地存进 nums1(不返回)。

## 思路
0. 如果nums2是空array，直接返回
1. i 指向 nums1 的有效部分开头,j 指向 nums2 开头,都从 0
2. 当 i 和 j 都还没走完时,循环:
   3. 如果 nums1[i] <= nums2[j],把 nums1[i] 放进结果,i 加 1
   4. 否则,把 nums2[j] 放进结果,j 加 1
5. 循环结束后,如果 nums2 还有剩,把剩下的接到结果后面
6. 如果 nums1 还有剩,把剩下的接到结果后面
7. 把结果逐个写回 nums1
## 我踩的坑
- 写了俩小时？？ merge sort有点忘了

## 解法
```Python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        if n == 0:
            return

        merge_list = []
        i = j = 0
        while i < m and j < n:
            if nums1[i] <= nums2[j]:
                merge_list.append(nums1[i])
                i += 1
            else:
                merge_list.append(nums2[j])
                j += 1
        if j < n:
            merge_list += nums2[j:]
        if i < m:
            merge_list += nums1[i:m]
        for k in range(len(merge_list)):
            nums1[k] = merge_list[k]

```
## 复杂度
- 时间 O(m+n),空间 O(m+n)
- 可以做到O(1) - 还没写过