# 232. Implement Queue using Stacks

## 题目

使用两个**栈**实现一个先入先出（FIFO）的队列。队列需要支持以下操作：

- `push(x)`：将元素 x 推入队列末尾
- `pop()`：从队列开头移除并返回元素
- `peek()`：返回队列开头的元素（不移除）
- `empty()`：判断队列是否为空

**限制**：只能使用栈的标准操作 —— 即 `push to top`、`peek/pop from top`、`size`、`is empty`。

## 思路

队列是 **FIFO**（先进先出），栈是 **LIFO**（后进先出），用栈实现队列的核心就是**把顺序反过来**。

用两个栈：
- `in_stack`：专门负责入队，新元素都 push 到这里
- `out_stack`：专门负责出队，从这里 pop / peek

**关键操作**：当 `out_stack` 为空、需要出队时，把 `in_stack` 里所有元素**一个一个 pop 出来 append 到 out_stack**，顺序就自动反转了，栈顶正好是最早入队的元素。

举例 push 1, 2, 3 后：
```
in_stack  = [1, 2, 3]   (3 在栈顶)
out_stack = []
```
第一次 pop 时把 in_stack 倒过去：
```
in_stack  = []
out_stack = [3, 2, 1]   (1 在栈顶 ← 队首)
```
之后只要 `out_stack` 还有东西，就继续从它 pop，不用每次都倒。**只有 out_stack 空了才再倒一次**，这样均摊复杂度是 O(1)。

## 我踩的坑

1. **`self` 是什么没搞清楚**：一开始写了 `self.append(x)`、`self[0]`、`len(self)`，以为 `self` 就是一个 list。其实 `self` 是"对象本身"，需要在 `__init__` 里给它绑定属性（比如 `self.lists = []`），所有数据操作都对那个属性进行。

2. **`pop` 无限递归**：写过 `def pop(self): self.pop()`，这是在调用自己，会栈溢出。应该调用 list 的 pop（`self.lists.pop()`），不是方法自己。

3. **`empty` 逻辑写反了**：写过 `if len > 0: return True`，但"非空"应该返回 `False`。后来发现最 Pythonic 的写法是直接 `return not self.lists`，利用空 list 的 falsy 特性。

4. **混淆了 FIFO 和 LIFO**：第一版用 `pop(-1)` 弹末尾元素，那是栈的行为。队列要弹的是**第一个**，应该 `pop(0)`。

5. **用了 list 的特性"作弊"**：用 `pop(0)` 虽然能 AC，但违背了题目"只用栈操作"的要求。栈只能在末尾操作，所以必须用双栈来反转顺序。

6. **变量命名**：`self.list = []` 会遮蔽内置类型 `list`，改成 `self.lists` 或 `self.in_stack` 更好。

## 解法

```python
class MyQueue:

    def __init__(self):
        self.in_stack = []   # 负责入队
        self.out_stack = []  # 负责出队

    def push(self, x: int) -> None:
        self.in_stack.append(x)

    def pop(self) -> int:
        self._move()
        return self.out_stack.pop()

    def peek(self) -> int:
        self._move()
        return self.out_stack[-1]

    def empty(self) -> bool:
        return not self.in_stack and not self.out_stack

    def _move(self) -> None:
        """当 out_stack 为空时，把 in_stack 全部倒过去"""
        if not self.out_stack:
            while self.in_stack:
                self.out_stack.append(self.in_stack.pop())
```

## 复杂度

- **push**：O(1) —— 直接 append 到 in_stack
- **pop / peek**：**均摊 O(1)**，最坏 O(n)
  - 每个元素最多被"搬运"一次（从 in_stack 到 out_stack），所以 n 次操作的总成本是 O(n)，均摊到每次操作就是 O(1)
  - 单次最坏情况是 O(n)（刚好触发搬运全部元素）
- **empty**：O(1)
- **空间复杂度**：O(n)，两个栈合计存放所有元素
