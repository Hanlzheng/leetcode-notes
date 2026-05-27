# 383. Ransom Note

## 题目

给你两个字符串 `ransomNote` 和 `magazine`，判断 `ransomNote` 能不能由 `magazine` 里的字符**拼出来**。

**限制**：`magazine` 里**每个字符只能用一次**。

**例子**：
```
ransomNote = "a",   magazine = "b"    → False
ransomNote = "aa",  magazine = "ab"   → False  (只有一个 a，不够拼两个)
ransomNote = "aa",  magazine = "aab"  → True   (有两个 a，够用)
```

**背景小知识**：ransom note（赎金信）是绑匪从杂志上剪字母拼出来的勒索信，避免被笔迹识别 💸

## 思路

核心就是**数每个字母够不够用**。用哈希表：

1. **第一遍**：遍历 `magazine`，统计每个字符出现的次数，存到字典 `d` 里
2. **第二遍**：遍历 `ransomNote`，每用一个字符就在 `d` 里减 1
   - 如果字符不在 `d` 里 → 杂志没这个字母 → `False`
   - 如果字符在 `d` 里但次数已经是 0 → 库存用完了 → `False`
   - 否则 `d[j] -= 1`，继续
3. 循环走完都没 return False → 全部能匹配 → `True`

## 我踩的坑

1. **`()` 和 `[]` 混淆**：写过 `magazine_list(i)`，圆括号是**函数调用**，所以报 `'list' object is not callable`。访问 list 元素必须用方括号 `magazine_list[i]`。

2. **字典 key 用错**：第一版 `d[i] += 1` 里的 `i` 是循环索引（0, 1, 2…），但我想统计的是**字符**。要用 `d[magazine_list[i]] += 1`，或者直接 `for i in magazine` 让 `i` 就是字符本身。

3. **循环变量串台**：第二个循环写 `for j in ransomNote` 但里面用了 `d[i] -= 1` —— `i` 是上一个循环的最后一个值，跟当前完全无关。改成 `d[j] -= 1`。

4. **多余的兜底检查**：一开始结尾写了 `return all(v >= 0 for v in d.values())` 来补救"减成负数"的情况。其实只要在减之前先判断 `d[j] > 0`，就不会减成负数，根本不需要这行。

5. **漏写 `return True`**：把最后一行删掉后函数会默认返回 `None`，逻辑上错了。Python 函数不显式 return 就是 `None`。

6. **多此一举的转 list**：写了 `list(ransomNote)`、`list(magazine)` 转成 list 再遍历。其实字符串本身就是可迭代的，`for ch in s` 直接用就行。

## 解法

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        d = {}
        # 第一遍：统计 magazine 里每个字符的次数
        for i in magazine:
            if i in d:
                d[i] += 1
            else:
                d[i] = 1
        # 第二遍:遍历 ransomNote，每用一个字符就减 1
        for j in ransomNote:
            if j in d and d[j] > 0:
                d[j] -= 1
            else:
                return False
        return True
```

### 进阶写法（了解一下）

**用 `dict.get()` 简化统计**：
```python
for i in magazine:
    d[i] = d.get(i, 0) + 1
```

**用 `collections.Counter` 一行搞定**：
```python
from collections import Counter
d = Counter(magazine)
```

**终极一行（炫技用，不推荐面试写）**：
```python
from collections import Counter
return not (Counter(ransomNote) - Counter(magazine))
```
Counter 之间的减法会自动处理"够不够"，剩下的就是"不够"的字符。结果为空就说明能拼出。

## 复杂度

- **时间复杂度**：O(m + n)
  - m = `magazine` 的长度，n = `ransomNote` 的长度
  - 两个循环各走一遍，字典的 `in`、`+=`、`-=` 都是 O(1)
- **空间复杂度**：O(k)
  - k 是字符种类数，最多 26（小写英文字母），所以也可以说是 O(1)
