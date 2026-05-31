# #49 Group Anagrams

## 题目

给一组字符串，把所有字母异位词分组在一起，返回 `List[List[str]]`。

## 思路

排序当签名：每个词排序后，异位词一定得到相同的 key。
用 dict 把相同 key 的词归到同一个 list。

1. 遍历 `strs`，每个词排序得到 key：`"".join(sorted(word))`
2. 把词 append 进 `d[key]`
3. 返回 `list(d.values())`

## 踩的坑

**坑 1：用 `set` 当 key**
`set` 有两个问题：
- `set` 是 unhashable 的，不能做 dict 的 key，会报错
- `set` 丢失次数信息：`"aab"` 和 `"ab"` 的 set 都是 `{'a', 'b'}`，但不是异位词

**坑 2：用 `sorted(i)` 当 key**
`sorted()` 返回的是 list，list 也是 unhashable 的。
→ 要用 `"".join(sorted(i))` 转成字符串才能当 key。

**坑 3：`return [d.values()]` vs `return list(d.values())`**
- `list(d.values())` → `[["eat", "tea"], ["tan"]]` ✅
- `[d.values()]` → 把整个 dict_values 对象塞进一个 list，只有一个元素 ❌

## 代码

```python
def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
    d = {}
    for i in strs:
        key = "".join(sorted(i))
        d.setdefault(key, []).append(i)
    return list(d.values())
```

## 复杂度

- 时间 O(n * k log k)，n 是字符串数量，k 是最长字符串长度（排序的代价）
- 空间 O(n)

## 和 #242 的关系

#242 可以用同样思路一行搞定：
```python
return sorted(s) == sorted(t)
```
但时间复杂度是 O(n log n)，比 dict 计数的 O(n) 慢。
→ #242 的 dict 计数版是最优解；排序版更简洁但稍慢。
