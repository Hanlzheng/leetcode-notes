# 876. Middle of the Linked List

## 题目

Given the `head` of a singly linked list, return the middle node of the linked list.

If there are two middle nodes, return the second middle node.

注意:返回的是**一个节点**,不是一个列表。LeetCode 显示 `[3,4,5]` 是因为返回节点 `3` 后,从它开始遍历打印出了后面整条链。

```
[1] ──▶ [2] ──▶ [3] ──▶ [4] ──▶ [5] ──▶ None
                 ▲
              中点,返回这个节点 → 打印出 [3,4,5]

[1] ──▶ [2] ──▶ [3] ──▶ [4] ──▶ [5] ──▶ [6] ──▶ None
                          ▲
              两个中点取第二个,返回节点 4 → 打印出 [4,5,6]
```

## 思路

### 解法一:两次遍历(我的第一版)

先遍历一遍数长度 `pos`,再算中点下标 `mid = pos // 2`,然后从头走 `mid` 步到中点。

这个思路清晰、容易理解,但遍历了两遍链表。

### 解法二:快慢指针(一次遍历)

跟 141 判环一样的快慢指针,只是目的不同:

- `slow` 每次走 1 步,`fast` 每次走 2 步,同起点出发。
- `fast` 速度是 `slow` 的 2 倍,当 `fast` 走完整条链表时,`slow` 刚好走了一半 —— 停在中点。
- 偶数长度时,`while fast and fast.next` 让 `fast` 在走到最后一个节点时停,此时 `slow` 自然停在**第二个中点**,符合题目要求。
- 循环结束后直接 `return slow`。

不数长度、不算下标、不用两次遍历,两个指针以不同速度跑,速度差让 `slow` 自动停在中点。

## 我踩的坑

- **第二个 while 循环忘了 `curr = curr.next`。** `k` 在涨,但 `curr` 不动,返回的永远是 `head`。链表遍历的基本操作:每轮必须让指针往前挪一格。

- **`mid` 的计算奇偶分了两个分支,奇数算错了。** 用 `if/else` 分奇偶处理,奇数时多加了 1,返回的节点偏了一个。其实 `mid = pos // 2` 一个公式奇偶都对。**能用一个公式解决的事,别拆成两个分支 —— 多余的分支不仅多写代码,还容易算错。**

- **`k` 从 0 数(下标)但 `mid` 按"第几个"(从 1 起)算,编号不统一。** 两套编号混用导致差一个。定好一套(建议全用 0 起的下标),从头到尾一致。

- **LeetCode 输出 `[3,4,5]` 以为要返回三个节点。** 其实只返回一个 `ListNode`,`[3,4,5]` 是 LeetCode 从该节点开始遍历打印的结果。返回一个节点 = 返回以它为首的整条链,这是链表的基本特性。

## 解法

### 解法一:两次遍历

```python
class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        pos = 0
        curr = head
        while curr:
            pos += 1
            curr = curr.next

        mid = pos // 2      # 奇偶都对,不用分情况

        k = 0
        curr = head
        while curr:
            if k == mid:
                return curr
            k += 1
            curr = curr.next
```

### 解法二:快慢指针

```python
class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = head
        while fast and fast.next:
            slow = slow.next           # 慢:走 1 步
            fast = fast.next.next      # 快:走 2 步
        return slow                    # fast 到头时,slow 在中点
```

## 复杂度

|  | 解法一(两次遍历) | 解法二(快慢指针) |
|---|---|---|
| 时间 | O(n),遍历两遍 | O(n),遍历一遍 |
| 空间 | O(1) | O(1) |

两者时间复杂度量级相同,但快慢指针实际只走一遍,常数更小、代码更短。

> 快慢指针的通用性:141 判环 = fast 追上 slow → 有环;876 找中点 = fast 到头 → slow 在中点。同一个工具,不同的退出条件,解决不同的问题。
