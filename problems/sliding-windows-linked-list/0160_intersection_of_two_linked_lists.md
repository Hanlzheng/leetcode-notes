# 160. Intersection of Two Linked Lists

## 题目

给定两个单链表，找到它们相交的起始节点。如果不相交，返回 `None`。

## 思路

双指针法。两个指针分别从两个链表头出发，走到末尾后重定向到另一个链表的头部。由于两个指针总共走的距离相同（`len(A) + len(B)`），如果有交点一定会在交点处相遇；如果没有交点，两个指针会同时变为 `None`。

## 我踩的坑

**修改了链表结构而不是重定向指针**

写成了：
```python
if p1.next == None:
    p1.next = headB  # 直接修改了节点的 next，破坏了原链表！
```

应该是重定向指针本身，而不是修改节点的链接：
```python
p1 = p1.next if p1 else headB  # 只移动指针，不碰节点
```

## 解法

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        p1 = headA
        p2 = headB
        while p1 != p2:
            p1 = p1.next if p1 else headB
            p2 = p2.next if p2 else headA
        return p1
```

## 复杂度

| | 复杂度 |
|---|---|
| 时间 | O(m + n) |
| 空间 | O(1) |
