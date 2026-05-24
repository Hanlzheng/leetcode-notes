# 125. Valid Palindrome

## 题目
- Given a string, determine whether it is a palindrome, considering only alphanumeric characters and ignoring letter case.
## 思路
- I iterate over the string and append every letter or digit to a new string, converting it to lowercase as I go. Then I check whether this new string equals its reverse — if so, it's a palindrome.
- 可以用碰撞指针 - 还没试过

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
## 复杂度