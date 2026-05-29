# 278. First Bad Version

## 题目

你是产品经理，目前正在带领一个团队开发新产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。

假设你有 `n` 个版本 `[1, 2, ..., n]`，你想找出导致之后所有版本出错的第一个错误的版本。

你可以通过调用 `bool isBadVersion(version)` 接口来判断版本号 `version` 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。

**示例：**
```
输入：n = 5, bad = 4
输出：4
解释：
- isBadVersion(3) → false
- isBadVersion(5) → true
- isBadVersion(4) → true
→ 第一个错误版本是 4
```

## 思路

在一个 `[1, n]` 的有序序列中找第一个满足条件的位置，这是**二分查找**的经典模板题。

序列具有单调性：
```
1, 2, 3, ..., k-1,  k,    k+1, ..., n
false, false, ..., false, true, true, ..., true
                          ↑ 目标
```

每次取中点 `mid`：
- 若 `isBadVersion(mid) == true`：第一个坏版本在 `mid` 或更左 → `right = mid - 1`
- 若 `isBadVersion(mid) == false`：第一个坏版本在 `mid` 右边 → `left = mid + 1`

循环结束时，`left` 就是第一个坏版本。

## 我踩的坑

循环结束后返回了 `middle` 而不是 `left`。

`middle` 是循环中最后一次计算的中点，它的值取决于最后一次迭代的情况，**并不能保证是答案**。很多测试用例碰巧能过，但这是巧合，不是正确逻辑。

```python
# ❌ 错误：返回 middle
while left <= right:
    middle = (left + right) // 2
    ...
return middle  # 最后一次的中点，不一定是答案

# ✅ 正确：返回 left
return left    # 循环结束时 left 收敛到第一个坏版本
```

**为什么 `left` 是正确答案？**

循环的不变量是：第一个坏版本一定在 `[left, right]` 范围内。当 `left > right` 时循环终止，此时 `left` 正好指向第一个坏版本。

## 解法

```python
class Solution:
    def firstBadVersion(self, n: int) -> int:
        left = 1
        right = n
        while left <= right:
            middle = (left + right) // 2
            if isBadVersion(middle):
                right = middle - 1
            else:
                left = middle + 1
        return left
```

## 复杂度

- **时间复杂度**：`O(log n)`，每次二分将搜索范围缩小一半
- **空间复杂度**：`O(1)`，只用了常数个变量
