# 643. Maximum Average Subarray I

## 题目

给定一个整数数组 `nums` 和整数 `k`，找出长度为 `k` 的连续子数组的最大平均值并返回。

## 思路

滑动窗口。先计算第一个窗口的和，然后每次滑动时加上新进来的元素、减去滑出去的元素，维护一个 running sum，避免重复求和。

## 我踩的坑

**坑1：变量名大小写不一致**

```python
max_Avg = cur_avg  # 赋值给 max_Avg（大写A）
return max_avg     # 返回 max_avg（小写a）永远返回初始值
```

**坑2：初始值设为 `0`**

如果所有元素都是负数，最大平均值也是负数，`0` 会比所有值都大，导致结果错误。应该用第一个窗口的值作为初始值：

```python
max_sum = sum(nums[:k])  # ✅ 用第一个窗口初始化
```

**坑3：`while rw <= n` 导致越界**

```python
while rw <= n:       # rw 最大等于 n
    rw += 1          # rw 变成 n
    cur_sum += nums[rw]  # nums[n] 越界！
```

应该改成 `while rw < len(nums)`，确保 `nums[rw]` 不越界。

## 解法

```python
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        cur_sum = sum(nums[:k])
        max_sum = cur_sum
        lw = 0
        rw = k
        while rw < len(nums):
            cur_sum = cur_sum - nums[lw] + nums[rw]
            max_sum = max(max_sum, cur_sum)
            lw += 1
            rw += 1
        return max_sum / k
```

## 复杂度

| | 复杂度 |
|---|---|
| 时间 | O(n) |
| 空间 | O(1) |
