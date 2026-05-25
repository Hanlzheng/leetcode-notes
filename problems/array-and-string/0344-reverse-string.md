# 344. Reverse String

## 题目
- 原地反转一个字符数组,不返回。

## 思路
- 两个指针:left 从头,right 从尾,交换后向中间移动
循环条件 while left < right(或 right > left),相遇就停
关键:循环体里必须有 left += 1 和 right -= 1,否则指针不动 → 死循环
这是「对撞指针」,区别于之前的「快慢指针」(#283/#26)
空间 O(1),不需要额外数组
## 我踩的坑
- for vs while:循环次数事先知道用 for;停止条件取决于运行中的状态(如指针相遇)用 while。
## 解法
```Python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left = 0
        n = len(s)
        right = n-1
        while right > left:
                s[left],s[right] = s[right], s[left]
                left += 1
```
## 复杂度
- 时间 O(n),
- 空间 O(1)。