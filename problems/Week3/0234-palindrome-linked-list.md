# 234. Palindrome Linked List

## 题目

给定一个单链表，判断它是否为回文链表。

## 思路

用快慢指针找到链表中点，同时用 stack 记录前半段的值。找到中点后，用 stack 和后半段逐一比较。

- 快指针每次走两步，慢指针每次走一步
- 快指针走完时，慢指针在中点
- 奇数长度时中间节点不需要比较，跳过它
- 逐一 pop stack 和后半段比较，不一致则返回 False

## 我踩的坑

**坑1：while 条件用了 `fast.next and fast.next.next`**

这个条件下偶数链表结束时 `fast` 停在倒数第二个节点，`fast` 不为 `None`，导致 `if fast` 永远为 `True`，偶数链表也会错误地跳过一个节点。

应该改成 `while fast and fast.next`，这样：
- 偶数长度：fast 为 `None`，`if fast` 为 False，slow 不跳，直接从后半段起点开始比较 ✅
- 奇数长度：fast 不为 `None`，`if fast` 为 True，slow 跳过中间节点 ✅

**坑2：pop stack 时没有处理 stack 为空的情况**

第二个 while 循环里需要检查 `not stack`，否则奇偶长度不匹配时会报错。

## 解法

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        stack = []
        fast = head
        slow = head
        while fast and fast.next:
            stack.append(slow.val)
            slow = slow.next
            fast = fast.next.next

        # 奇数长度时跳过中间节点
        if fast:
            slow = slow.next

        while slow:
            if not stack or slow.val != stack.pop():
                return False
            slow = slow.next
        return True
```

## 复杂度

| | 复杂度 |
|---|---|
| 时间 | O(n) |
| 空间 | O(n/2) = O(n) |
