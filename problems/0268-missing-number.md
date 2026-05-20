# 268. Missing Number 

## 题目
- 数组里是 0 到 n 中的 n 个不同数,缺一个,找出缺的那个

## 思路
- set 解法 —— 把 nums 转 set,从 0 到 n 检查哪个不在 set 里
- 数学解法 —— 缺失数 = n*(n+1)//2 − sum(nums),完整总和减实际总和 - 还没试过，但是很有趣啊这方法

## 我踩的坑
- 第一版用 in nums,in list 是 O(n) → 隐形的 O(n²)
- 第二版 set 套错对象,套在 len() 上了
- 第三版 set 写在循环里,每圈重建 → 还是 O(n²)
- 第四版才对:set 建在循环外,只建一次

## 原则
- in list 是 O(n),in set/in dict 是 O(1)
- set 要套在"被反复查的那个对象"上
- 循环里不变的东西要提到循环外,只算一次

## 解法
``` Python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        nums_set = set(nums)
        for i in range(len(nums)+1):
            if i not in nums_set:
                return i
```