# 1. Two Sum

## 思路
- 暴力解 O(n²)：双层循环，j 从 i+1 开始，直接比较 nums[i] + nums[j] == target
- 最优解 O(n)：哈希表，遍历时记录「见过的值 → 下标」，查 target - nums[i] 在不在 - 还没试过

## 我踩的坑
- 用 nums.index(j) 反查下标，数组有重复值时只返回第一个 → 不可靠
- 内层循环把切片 nums[i+1:] 的「值」当成「下标」用了 → 值和下标要分清
- range 多写了 -1，导致最后一个元素轮不到

## 暴力解
\`\`\`python
for i in range(len(nums)):
    for j in range(i + 1, len(nums)):
        if nums[i] + nums[j] == target:
            return [i, j]
\`\`\`