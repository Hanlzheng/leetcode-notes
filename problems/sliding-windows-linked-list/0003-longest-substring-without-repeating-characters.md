# 3. Longest Substring Without Repeating Characters

## 题目

给定一个字符串 `s`，找出不含重复字符的最长子串的长度。

## 思路

滑动窗口 + set。用 `lw` 和 `rw` 维护一个无重复字符的窗口，用 set 记录窗口内的字符。

- `rw` 每次往右扩展一个字符
- 如果 `s[rw]` 已经在 set 里，从左边逐个移除直到没有重复
- 每次更新 `max_len = rw - lw + 1`

## 我踩的坑

**坑1：用字符串切片 `s[lw:rw]` 做 `in` 查找，O(n) 太慢**

```python
if s[rw] not in s[lw:rw]  # ✗ 每次扫描整个窗口，整体 O(n²)
if s[rw] not in window     # ✅ set 查找 O(1)，整体 O(n)
```

**坑2：`lw` 更新逻辑复杂且容易出错**

用 `.index()` 找重复字符位置再跳跃，边界情况多，容易出 bug。用 set 之后直接从左边逐个移除，逻辑简单清晰：

```python
while s[rw] in window:
    window.remove(s[lw])
    lw += 1
```

**坑3：`cur_len` 多余**

单独维护 `cur_len` 容易出错（比如 else 分支忘记更新）。窗口长度直接用 `rw - lw + 1` 计算，不需要额外变量。

**坑4：空字符串边界**

初始值 `max_len = 0` 就能处理空字符串，不需要单独判断 `if n == 0`。

## 为什么用 set

| 操作 | 字符串切片 | set |
|------|-----------|-----|
| 查找 `in` | O(n) | O(1) |
| 整体复杂度 | O(n²) | O(n) |

set 天然不存重复，语义上也完美契合"当前窗口有没有重复字符"这个需求。

## 解法

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        window = set()
        lw = 0
        max_len = 0
        for rw in range(len(s)):
            while s[rw] in window:
                window.remove(s[lw])
                lw += 1
            window.add(s[rw])
            max_len = max(max_len, rw - lw + 1)
        return max_len
```

## 复杂度

| | 复杂度 |
|---|---|
| 时间 | O(n) |
| 空间 | O(min(n, m))，m 为字符集大小 |
