# 217. Contains Duplicate

## 题目
给一个数组 nums，如果有任何值出现至少两次返回 True，
每个元素都不同返回 False。

## 思路
- set 会自动去重。如果原数组有重复，转成 set 后长度会变短
- len(set(nums)) != len(nums) → 不相等说明有重复
- 另一种思路：边遍历边把见过的数放进 set，遇到已在 set 里的就是重复 - 还没试过

## 我踩的坑
- 一开始写了 False if 条件 else True，多余
- 条件本身就是布尔值，不用三元表达式包一层
- 教训：想写 True if 条件 else False 就直接写「条件」；
  False if 条件 else True 就直接写「not 条件」

## 解法 O(n)
```python
def containsDuplicate(self, nums):
    return len(set(nums)) != len(nums)
```

## 复杂度
- O(n)：set() 转换需要扫一遍数组
- 比双层循环 O(n²) 快