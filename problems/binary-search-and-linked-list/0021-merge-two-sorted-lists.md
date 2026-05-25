# 21. Merge Two Sorted Lists

## 题目

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one sorted list, splicing together the nodes of the
two lists, and return the head of the merged list.

```
list1: 1 ──▶ 2 ──▶ 4 ──▶ None
list2: 1 ──▶ 3 ──▶ 4 ──▶ None
结果:  1 ──▶ 1 ──▶ 2 ──▶ 3 ──▶ 4 ──▶ 4 ──▶ None
```

注意是"拼接已有节点",不是新建节点。

## 思路

数组里"收集结果"是 `res = []` + `append`;链表里没有 `append`,要用 **dummy + tail 指针**自己把节点接起来。

- `dummy`:假头,占位用。省掉"接第一个节点"的特判。
- `tail`:始终指向结果链表的最后一个节点,知道下一个该接在哪。

两条链表都是有序的,所以用 `curr1`、`curr2` 两个指针同时往后走,每轮比较两个当前节点的值,小的那个接到 `tail` 后面,然后那条链表的指针前进一格、`tail` 也前进一格。

`while curr1 and curr2` 在其中一条走完时结束。此时另一条还剩一段没接 —— 直接把**整条剩余链表**接到 `tail` 后面即可(剩余链表本身有序,且接在结果末尾仍然有序)。

最后 `return dummy.next`,跳过假头。

## 我踩的坑

- **`tail.next = None` 收尾收错了。** 一开始在函数末尾加了 `tail.next = None`,结果把链表截断了。原因:`while` 结束后用 `tail.next = curr1` 接的是**整条剩余链表**,而 `tail` 只挪到这条链的第一个节点上,后面还有一长串。这时 `tail.next = None` 会把第一个节点之后的全部砍掉。

- **关键判断:接"整条"还是逐个"挑节点"。**
  - 接整条(本题 merge):剩余链表自带正确的 `None` 结尾,结果尾巴天然正确,**不需要也不能** `tail.next = None`。
  - 逐个挑节点(过滤类题目):挑中的节点 `next` 还连着原链表里不该要的节点,最后一个节点没人覆盖,**必须** `tail.next = None` 切断。
  - 本题属于前者,所以删掉 `tail.next = None` 才是干净解法。

- **AC 不等于代码对。** 把 `tail.next = None` 移到 `while` 循环里也能 AC —— 因为每轮设的 `None` 下一轮立刻被 `tail.next = curr` 覆盖掉。但这是一行"被立刻抹掉、毫无作用"的死代码。代码好的标准是每一行都有不可省略的作用,不是"测试通过"。正确做法是直接删掉它。

## 解法

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        curr1 = list1
        curr2 = list2

        dummy = ListNode()      # 假头
        tail = dummy            # 始终指向结果链表的尾

        while curr1 and curr2:
            if curr1.val <= curr2.val:
                tail.next = curr1
                tail = curr1
                curr1 = curr1.next
            else:
                tail.next = curr2
                tail = curr2
                curr2 = curr2.next

        # 其中一条已走完,把另一条剩余的整条接上(不需要 tail.next = None)
        if curr1:
            tail.next = curr1
        if curr2:
            tail.next = curr2

        return dummy.next
```

## 复杂度

- 时间:O(n + m) —— n、m 为两条链表长度,每个节点各处理一次。
- 空间:O(1) —— 只用 `dummy`、`tail`、`curr1`、`curr2` 几个指针,没有新建节点,直接拼接原有节点。
