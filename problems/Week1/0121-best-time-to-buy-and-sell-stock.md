# 121. Best Time to Buy and Sell Stock

## 题目
给一个数组 prices，prices[i] 是第 i 天的股价。
只能买一次卖一次，且必须先买后卖。求最大利润，赚不到返回 0。

## 思路
- 暴力解 O(n²)：双层循环，每对 (买日 i, 卖日 j) 都试，j 从 i+1 开始
  → 会 TLE（数据量大时超时），题目在逼我优化
- 最优解 O(n)：从左往右扫一遍，一路维护「目前见过的最低价」
  每天算「今天价 − 最低价」，更新最大利润

## 我踩的坑
- 又是值 vs 下标：写了 for i in prices，i 拿到的是价格值不是下标
  然后 prices[i+1:] 把值当下标用 → case 2 输出错误的 3
- 教训：需要知道「第几天」（顺序）→ 必须用 range(len()) 下标遍历

## 暴力解 O(n²)
```python
def maxProfit(self, prices):
    profit = 0
    for i in range(len(prices)):
        for j in range(i + 1, len(prices)):
            current_profit = prices[j] - prices[i]
            if current_profit > profit:
                profit = current_profit
    return profit
```

## 最优解 O(n)
```python
def maxProfit(self, prices):
    min_price = prices[0]
    max_profit = 0
    for i in range(len(prices)):
        if prices[i] < min_price:
            min_price = prices[i]
        current_profit = prices[i] - min_price
        if current_profit > max_profit:
            max_profit = current_profit
    return max_profit
```

## 套路总结
- 「一遍扫描 + 一路维护某个最值/状态」可以把 O(n²) 降到 O(n)
- 面试模式：先给暴力解 → 被问「能更快吗」→ 优化到最优解