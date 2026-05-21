# 66. Plus One

## 题目
- 用数组表示一个非负整数(每个元素是一位数字,最高位在前),给这个数加 1,返回结果数组。

## 思路
- 模拟手算加法。最后一位 +1,从后往前遍历:某位满 10 就置 0、向前一位进位;如果最高位还在进位,最后在数组最前面插一个 1。

## 我踩的坑
- digits[i-1] += 1 在 i=0 时,i-1 变成 -1,digits[-1] 是最后一位 → 进位绕回末尾,结果全错。要用 if i > 0 区分。(这个坑 #26 也踩过)
- 一开始没处理"最高位溢出"的情况,[9,9,9] 这种数组要变长,得 insert(0,1)。
- 把 digits.insert(0,1) 写在了循环里 → 边遍历边改数组长度。虽然碰巧能过(因为插入发生在最后一轮),但不可靠。正确做法:循环里只设 carry 标记,循环结束后再 insert。

## 解法
```Python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        n = len(digits)
        digits[-1] += 1
        carry = False
        for i in range(n-1,-1,-1):
            if digits[i] == 10:
                digits[i] = 0
                if i == 0:
                    carry = True
                else:
                    digits[i-1] += 1
        if carry:
            digits.insert(0,1)
        return digits
```
## 复杂度
- 时间 O(n),空间 O(1)