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

- Python 的两种除法:
- / 浮点除法,结果永远带小数:4/2 → 2.0
- // 整数除法(整除),结果是整数:4//2 → 2
- 算下标、计数、求和公式这类整数运算,用 //,省得再套 int()

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

``` Python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        n = len(nums)
        return (n*(n+1))//2 - sum(nums)
```

## 复杂度
- 复杂度相同 ≠ 实际速度相同:
- 大 O 忽略常数,但常数在真实运行时间里是存在的
- 简单操作(加减、比较)比复杂操作(hash 计算、内存分配)的常数小
- 同样 O(n),"只做加法"会比"做 hash 操作"实测更快
- 面试看复杂度,不看 LeetCode 那个 runtime 数字 —— runtime 受常数、机器、当时负载影 - 响,不稳定;面试官只问大 O