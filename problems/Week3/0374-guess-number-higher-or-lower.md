# 374. Guess Number Higher or Lower

## 题目

系统预先选定一个 `1` 到 `n` 之间的数字，你需要猜出这个数字。  
每次猜测可以调用 `guess(num)` API：
- 返回 `-1`：你猜的数**偏大**
- 返回 `1`：你猜的数**偏小**
- 返回 `0`：猜对了 🎉

## 思路

标准二分搜索。每次取中间值调用 `guess()`，根据返回值缩小范围：
- `-1` → 目标在左半边，`right = middle - 1`
- `1` → 目标在右半边，`left = middle + 1`
- `0` → 直接返回

## 我踩的坑

无，一把过 ✅

## 解法

```python
# The guess API is already defined for you.
# @param num, your guess
# @return -1 if num is higher than the picked number
#          1 if num is lower than the picked number
#          otherwise return 0
# def guess(num: int) -> int:

class Solution:
    def guessNumber(self, n: int) -> int:
        left = 1
        right = n
        while left <= right:
            middle = (left + right) // 2
            if guess(middle) == -1:
                right = middle - 1
            elif guess(middle) == 1:
                left = middle + 1
            else:
                return middle
```

## 复杂度

| | 复杂度 |
|---|---|
| 时间 | O(log n) |
| 空间 | O(1) |
