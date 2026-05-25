# 206. Reverse Linked List

## 题目

Given the `head` of a singly linked list, reverse the list and return the new head.

原本箭头朝后,反转后箭头全部朝前:

```
原始:  1 ──▶ 2 ──▶ 3 ──▶ None
反转后: None ◀── 1 ◀── 2 ◀── 3
即:    3 ──▶ 2 ──▶ 1 ──▶ None
```

## 思路

单链表每个节点只有 `next`,箭头单向。反转的本质就是:**沿链表走一遍,把每个节点的 `next` 逐个掉头,指向前一个节点。**

用三个指针:

- `prev`:已经反转好的那部分的头。初始为 `None`,因为原 head 反转后变成尾,尾的 `next` 必须指向 `None`。
- `curr`:当前正在处理的节点。初始为 `head`。
- `nxt`:临时存住 `curr` 的下一个节点,留好退路。

每一轮按固定顺序做四件事 —— **先存、再改、后移**:

1. `nxt = curr.next` —— 先把下一个节点存住(留退路)
2. `curr.next = prev` —— 掉头:当前节点的箭头指向前一个
3. `prev = curr` —— `prev` 前进一格
4. `curr = nxt` —— `curr` 前进一格

`curr` 走到 `None` 时停,此时 `prev` 停在最后处理的节点,就是新头。

## 我踩的坑

- **乱用 dummy node。** dummy 是给删除/插入题用的(给 head 配前驱)。反转题里"前一个节点"已经由 `prev` 指针扮演,不需要 dummy。错误地用 dummy 还会让反转后的链表尾巴挂一个多余的 val=0 假节点。

- **复制节点 vs 指针指向没分清。** `x = 某个已有节点` 是"贴标签",指向原节点;`ListNode(...)` 是"造新货",新建一个独立节点。反转题全程只移动指针、改原节点的 `next`,一个新节点都不该造。

- **循环里从来没有写 `curr.next`。** 反转的核心是**改写** `next`(`curr.next = prev`)。早期几版循环里全是在读 `curr.next`、存进别的变量,却没有任何一行去写 `curr.next`,结果链表结构原封不动,什么都没反转。自检:循环里必须有一行 `curr.next = ...`。

- **四步顺序错乱。** 必须先 `nxt = curr.next` 再 `curr.next = prev`。如果先掉头,`curr` 通往后面的箭头被覆盖,后面整段失联、找不回来。任何破坏性操作之前,先把会被破坏的东西存好。

- **`return head` 而不是 `return prev`。** 循环结束时原 `head` 已经变成新链表的尾。新头是 `prev`。

## 解法

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        curr = head
        prev = None
        while curr:
            nxt = curr.next      # 1. 先存退路
            curr.next = prev     # 2. 掉头
            prev = curr          # 3. prev 前进
            curr = nxt           # 4. curr 前进
        return prev              # prev 是新头
```

## 复杂度

- 时间:O(n) —— 整条链表走一遍。
- 空间:O(1) —— 只用 `prev`、`curr`、`nxt` 三个指针,与链表长度无关。

> 注:如果用递归写,递归调用栈深度为 n,空间会变成 O(n)。迭代版才是 O(1)。