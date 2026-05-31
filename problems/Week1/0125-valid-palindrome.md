# 125. Valid Palindrome

## 题目
- Given a string, determine whether it is a palindrome, considering only alphanumeric characters and ignoring letter case.
## 思路
- I iterate over the string and append every letter or digit to a new string, converting it to lowercase as I go. Then I check whether this new string equals its reverse — if so, it's a palindrome.
- 可以用碰撞指针： 设置左指针为能access的第一个index，设置右指针为能access的最后一个index，碰撞条件是两指针过了相交。然后检查两指针指向的元素是不是数字/字母，如果是，检查小写是否不相等，如果是的话就可以直接return False。如果不全是数字/字母，检查左指针是不是数字/字母，如果不是，就往右一格。检查右指针是不是数字/字母，如果不是就往左一格。如果全部检查完都没有early exit，就证明没问题，就可以返回True

## 我踩的坑
- Need to check both alpha and digit

## 解法
```Python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        palindrome = ""
        for i in s:
            if i.isalpha() or i.isdigit():
                palindrome += i
        return palindrome.lower()[::-1] == palindrome.lower()

```
```Python - 碰撞指针
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left = 0
        right = len(s) - 1
        while left < right:
            if s[left].isalnum() and s[right].isalnum():
                if s[left].lower() != s[right].lower():
                    return False
                left += 1
                right -= 1
            elif not s[left].isalnum():
                left += 1
            elif not s[right].isalnum():
                right -= 1
        return True
```
## 复杂度