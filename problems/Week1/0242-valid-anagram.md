# #242 Valid Anagram

## 题目

给两个字符串 `s` 和 `t`，判断 `t` 是不是 `s` 的字母异位词（用了完全相同的字母、相同次数，只是顺序不同）。

## 思路

Hash 计数 + 抵消：

1. 遍历 `s`，用 dict 记录每个字母出现的次数
2. 遍历 `t`，每个字母在 dict 里把对应计数 -1
3. 最后检查 dict 里所有计数是否全为 0

## 踩的坑

**坑 1：t 里有 s 没有的字母**
第二个循环要用 `if j in d`，否则 KeyError。
但注意：跳过了不等于"不存在"，这个字母就被无视了。
→ 最干净的解法：开头先 `if len(s) != len(t): return False`，长度不等直接否掉。

**坑 2：计数减成负数**
如果 `t` 里某字母比 `s` 多，`d[j]` 会变负数。
最后用 `all(v == 0 for v in d.values())` 检查就能正确否掉（负数 ≠ 0）。

**坑 3：最后检查写错**
要的是「**所有** count 都为 0」，不是「有没有一个为 0」。
→ 用 `all()`，不要用 `any()`。

## 代码

```python
def isAnagram(s: str, t: str) -> bool:
    if len(s) != len(t):
        return False

    d = {}
    for c in s:
        d[c] = d.get(c, 0) + 1

    for c in t:
        if c not in d:
            return False
        d[c] -= 1

    return all(v == 0 for v in d.values())
```

## 复杂度

- 时间 O(n)
- 空间 O(1)（字母只有 26 个，dict 大小有上限）

## 用英文讲解（面试用）

> First, if the two strings have different lengths, they can't be anagrams — return False immediately.
> Then, I iterate over `s` and use a dictionary to record how many times each letter appears.
> Next, I iterate over `t`, and for each letter, I decrement its count in the dictionary.
> Finally, I check whether all the counts are zero — if they are, the two strings are anagrams.

关键词：`iterate over` / `decrement` / `check whether all ... are zero`
