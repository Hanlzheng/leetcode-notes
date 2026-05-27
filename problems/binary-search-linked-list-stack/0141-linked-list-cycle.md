# 141. Linked List Cycle

## 题目

Given the `head` of a linked list, determine if the linked list has a cycle in it.

有环 = 某个节点的 `next` 指回了链表前面的某个节点,导致没有尾节点、没有 `None` 结尾。

```
[1] ──▶ [2] ──▶ [3] ──▶ [4]
         ▲                │
         └────────────────┘
```

题目里的 `pos` 只是用来描述测试用例(尾节点接到哪个下标),**不是函数参数,代码里拿不到、也用不上**。要靠算法自己判断有没有环。

## 思路

有环时,普通的 `while curr:` 遍历不会停 —— `next` 永远不为 `None`,死循环。所以不能靠"走到 None 为止"。

用 **Floyd 判圈算法 / 快慢指针(龟兔赛跑)**:

- `slow` 每次走 1 步,`fast` 每次走 2 步,同起点出发。
- 无环:`fast` 跑得快,会先到链表尾,`fast` 或 `fast.next` 变成 `None` → 退出 → 返回 `False`。
- 有环:两个指针都进环后,`fast` 每轮比 `slow` 多走 1 步,距离每轮严格减 1,迟早归零 → 相遇 `slow == fast` → 返回 `True`。

为什么一定相遇:速度差恰好为 1,两指针的距离每轮减 1,是个每次减 1 的非负整数,必然踩到 0,不会"跨过"。这也是为什么必须是 1 步 vs 2 步 —— 速度差更大可能擦肩而过。

注意这是"追及"机制(距离单调减到 0),不是"最小公倍数 / 周期重合"机制。

## 我踩的坑

- **`while fast` 不够,会崩 `AttributeError`。** 循环体里有 `fast = fast.next.next`,连点了两层 `.next`。`while fast` 只保证 `fast` 不是 `None`,没保证 `fast.next` 不是 `None`。当 `fast` 走到最后一个节点时,`fast` 非空但 `fast.next` 是 `None`,`fast.next.next` 就是 `None.next` → 报 `'NoneType' object has no attribute 'next'`。

- **修复:`while fast and fast.next`。** 进循环体前同时确认两层都不是 `None`。规律:**循环体里 `.next` 点了几层,`while` 条件就要检查几层。** 点一层查 `fast`,点两层查 `fast and fast.next`。

- **`and` 短路,顺序不能反。** 写 `fast and fast.next`,不能写 `fast.next and fast`。`and` 左边为假就不看右边;先检查"父节点 `fast`"存在,再检查"`fast.next`",由外到内。反过来当 `fast` 是 `None` 时,先求 `fast.next` 就直接崩了。

- **两个出口各管一种情况。** 无环靠 `while` 条件不成立退出(返回 `False`);有环靠循环体内 `slow == fast` 退出(返回 `True`)。`while` 条件对有环情况"没用",但对无环情况必不可少 —— 它是为无环兜底的。一个循环要安全,每种输入都得有一个一定会触发的出口。

## 解法

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        fast, slow = head, head
        while fast and fast.next:
            slow = slow.next         # 慢:走 1 步
            fast = fast.next.next    # 快:走 2 步
            if slow == fast:         # 相遇 → 有环
                return True
        return False                 # fast 走到 None → 无环
```

## 复杂度

- 时间:O(n) —— 无环时 `fast` 走 n/2 步到头;有环时两指针在环内 O(n) 步内相遇。
- 空间:O(1) —— 只用 `fast`、`slow` 两个指针,与链表长度无关。

> 进阶:142 题"找环的入口节点"也用 Floyd 算法 —— 第一次相遇后把一个指针放回 `head`,两指针都改成每次走 1 步,再次相遇处就是环入口。
