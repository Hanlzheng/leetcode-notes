# 203. Remove Linked List Elements

## 题目

给定一个链表和一个值 `val`，删除链表中所有值等于 `val` 的节点，返回新链表的头节点。

## 思路

用 dummy 节点避免删除头节点的边界情况。`curr` 从 dummy 出发，检查 `curr.next` 的值，如果等于 `val` 就跳过，否则移动 `curr`。

## 我踩的坑

**坑1：`dummy = ListNode` 没加括号**

```python
dummy = ListNode    # ✗ 这是类本身，不是对象
dummy = ListNode(0) # ✅ 这才是创建实例
```

**坑2：删除节点后仍然移动 `curr`，跳过了新的 `curr.next`**

```python
# ✗ 错误写法：删除后还是移动了 curr
while curr.next:
    if curr.next.val == val:
        curr.next = curr.next.next
    curr = curr.next  # 删除后不应该移动！新的 curr.next 还没检查

# ✅ 正确写法：只有不删除时才移动
while curr.next:
    if curr.next.val == val:
        curr.next = curr.next.next
    else:
        curr = curr.next
```

## 解法

```python
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        dummy = ListNode(0)
        dummy.next = head
        curr = dummy
        while curr.next:
            if curr.next.val == val:
                curr.next = curr.next.next
            else:
                curr = curr.next
        return dummy.next
```

## 复杂度

| | 复杂度 |
|---|---|
| 时间 | O(n) |
| 空间 | O(1) |
