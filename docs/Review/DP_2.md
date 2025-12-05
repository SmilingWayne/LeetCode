# 勇闯秋招手撕系列 V：动态规划 2️⃣

!!! abstract ""
    <span style="color:orange;font-weight:bold">爬楼梯系列🌟</span>

    <span style="color:orange;font-weight:bold">单词拆分</span>

    <span style="color:orange;font-weight:bold">不同路径系列🌟</span>

    <span style="color:orange;font-weight:bold">最小路径和</span>

    <span style="color:orange;font-weight:bold">最大正方形🌟🌟</span>

    <span style="color:orange;font-weight:bold">交错字符串（150）</span>

    <span style="color:orange;font-weight:bold">买卖股票的最佳时机系列（150）🌟</span>

    <span style="color:red;font-weight:bold">最长有效括号</span>

## [70. 爬楼梯 I](https://leetcode.cn/problems/climbing-stairs/?envType=study-plan-v2&envId=top-100-liked)

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？输入：n = 2;输出：2

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n == 1:
            return 1
        if n == 2:
            return 2 
        dp = [0 for _ in range(n + 1)]
        dp[0] = 1
        dp[1] = 1
        for i in range(2, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]
        return dp[-1]
```

## [746. 使用最小费用爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs/description/)

给你一个整数数组 cost ，其中 cost[i] 是从楼梯第 i 个台阶向上爬需要支付的费用。一旦你支付此费用，即可选择向上爬一个或者两个台阶。你可以选择从下标为 0 或下标为 1 的台阶开始爬楼梯。**请你计算并返回达到楼梯顶部的最低花费**。

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        n = len(cost)
        dp = [0] * n 
        dp[0] = cost[0]
        dp[1] = cost[1]
        for i in range(2, n):
            dp[i] = min(dp[i - 1], dp[i - 2]) + cost[i]
        return min(dp[-1], dp[-2])
```

## [139. 单词拆分](https://leetcode.cn/problems/word-break/?envType=study-plan-v2&envId=top-100-liked)

给你一个字符串 s 和一个字符串列表 wordDict 作为字典。如果可以利用字典中出现的一个或多个单词拼接出 s 则返回 true。

注意：不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        self.res = False 
        maxLen = max(map(len, wordDict))
        wordSet = set(wordDict)
        n = len(s)
        @cache
        def dfs(idx):
            if idx == n:
                self.res = True 
                return 
            
            for i in range(maxLen):
                if s[idx: idx + i + 1] in wordSet:
                    dfs(idx + i + 1)
        dfs(0)
        return self.res
```












---


## [221. 最大正方形](https://leetcode.cn/problems/maximal-square/?envType=study-plan-v2&envId=top-interview-150)

在一个由 '0' 和 '1' 组成的二维矩阵内，找到只包含 '1' 的最大正方形，并返回其面积。

```python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        m = len(matrix)
        n = len(matrix[0])
        dp = [[0 for _ in range(n)] for _ in range(m)]
        res = 0
        for i in range(m):
            if matrix[i][0] == "1":
                dp[i][0] = 1
                res = 1
        for i in range(n):
            if matrix[0][i] == "1":
                dp[0][i] = 1
                res = 1
        for i in range(1, m):
            for j in range(1, n):
                if matrix[i][j] == "1":
                    w = min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1]) + 1
                    dp[i][j] = w 
                    res = max(res, w * w )
        return res
```





## [64. 最小路径和](https://leetcode.cn/problems/minimum-path-sum/?envType=study-plan-v2&envId=top-interview-150)

给定一个包含非负整数的 m x n 网格 grid ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

说明：每次只能向下或者向右移动一步。


```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        m = len(grid)
        n = len(grid[0])
        dp = [[0 for _ in range(n)] for _ in range(m)]
        dp[0][0] = grid[0][0]
        for i in range(1, m):
            dp[i][0] = dp[i - 1][0] + grid[i][0]
        for i in range(1, n):
            dp[0][i] = dp[0][i - 1] + grid[0][i]
        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j]
        return dp[-1][-1]
```















## [63. 不同路径](https://leetcode.cn/problems/unique-paths-ii/?envType=study-plan-v2&envId=top-interview-150)

给定一个 m x n 的整数数组 grid。一个机器人初始位于 左上角（即 grid[0][0]）。机器人尝试移动到 右下角（即 grid[m - 1][n - 1]）。机器人每次只能向下或者向右移动一步。

**网格中的障碍物和空位置分别用 1 和 0 来表示。机器人的移动路径中不能包含 任何 有障碍物的方格。返回机器人能够到达右下角的不同路径数量。**

```python
class Solution:
    def uniquePathsWithObstacles(self, grid: List[List[int]]) -> int:
        m = len(grid)
        n = len(grid[0]) 
        dp = [[0 for _ in range(n)] for _ in range(m)]
        dp[0][0] = 1
        if grid[0][0] == 1 or grid[-1][-1] == 1:
            return 0
        for i in range(1, m):
            if grid[i][0] == 1:
                break
            else:
                dp[i][0] = 1
        for i in range(1, n):
            if grid[0][i] == 1:
                break
            else:
                dp[0][i] = 1
        for i in range(1, m):
            for j in range(1, n):
                if grid[i][j] == 1:
                    dp[i][j] = 0
                else:
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
        return dp[-1][-1]
        
```




## [32. 最长有效括号](https://leetcode.cn/problems/longest-valid-parentheses/description/?envType=study-plan-v2&envId=top-100-liked)


难度：<span style = "color:red; font-weight:bold">Medium 困难 </span>。给你一个只包含 '(' 和 ')' 的字符串，找出最长有效（格式正确且连续）括号子串的长度。输入：s = "(()"；输出：2




```python
from collections import deque
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        stack = deque([-1])
        res = 0
        for idx, sub_s in enumerate(s):
            if sub_s == "(":
                stack.append(idx)
            else:
                stack.pop()
                if len(stack) == 0:
                    stack.append(idx)
                else:
                    res = max(res, idx - stack[-1])
        return res

```

!!! quote ""
    每次标记“前一个导致不可行的位置，注意前面补-1；

    如果遇到了一个 `(`，无脑入stack；

    如果遇到了一个 `)`，默认踢掉前面一个（假设进行了匹配），这时候有一种情况是把原先队列的 -1 踢掉了，此时这个`)` 就成了前一个不可行的位置，于是自己入队；

    否则，查看当前位置到前面一个不可行位置的距离。









## [97. 交错字符串](https://leetcode.cn/problems/interleaving-string/?envType=study-plan-v2&envId=top-interview-150)

给定三个字符串 s1、s2、s3，请你帮忙验证 s3 是否是由 s1 和 s2 交错 组成的。两个字符串 s 和 t 交错 的定义与过程如下，其中每个字符串都会被分割成若干 非空 子字符串：

- s = s1 + s2 + ... + sn
- t = t1 + t2 + ... + tm
- |n - m| <= 1

交错 是 s1 + t1 + s2 + t2 + s3 + t3 + ... 或者 t1 + s1 + t2 + s2 + t3 + s3 + ...；注意：a + b 意味着字符串 a 和 b 连接。

```python
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        self.ans = False 
        m = len(s1)
        n = len(s2)
        l = len(s3)
        if m + n != l:
            return False 
        dp = [[False for _ in range(n + 1)] for _ in range(m + 1)]
        def dfs(i, j, k):
            if self.ans or dp[i][j]:
                return 
            dp[i][j] = True 
            if i == m and j == n and k == l:
                self.ans = True 
                return 
            if i < m and s1[i] == s3[k]:
                dfs(i + 1, j, k + 1)
            if j < n and s2[j] == s3[k]:
                dfs(i, j + 1, k + 1)
        dfs(0, 0, 0)
        return self.ans
```

---


## [121. 买卖股票的最佳时机 I ](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。

你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。如果你不能获取任何利润，返回 0 。

输入：[7,1,5,3,6,4]；输出：5

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        curmin = prices[0]
        ans = 0
        for price in prices:
            ans = max(ans, price - curmin)
            curmin = min(curmin, price)
        return ans
```

## [121. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/description/)


给你一个整数数组 prices ，其中 prices[i] 表示某支股票第 i 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 最多 只能持有 一股 股票。然而，你可以在 同一天 多次买卖该股票，但要确保你持有的股票不超过一股。返回 你能获得的 最大 利润 。

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        res = 0
        n = len(prices)
        for i in range(n - 1):
            if prices[i + 1] - prices[i] > 0:
                res += (prices[i + 1] - prices[i])
        return res
```

## [188. 买卖股票的最佳时机 IV](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/)

给你一个整数数组 prices 和一个整数 k ，其中 prices[i] 是某支给定的股票在第 i 天的价格。

**设计一个算法来计算你所能获取的最大利润。你最多可以完成 k 笔交易。也就是说，你最多可以买 k 次，卖 k 次。**

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

> LeetCode 上 III 和 IV 实际上是同一道题。只不过一个hardcode了次数为3，一个写成了 k，思路都是一样的。
 

```python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        FREE = 0
        HOLD = 1
        n = len(prices)
        dp = [[[0, 0] for _ in range(k + 1)] for _ in range(n + 1)]
        for i in range(k + 1):
            dp[0][i][HOLD] = - prices[0]
        
        for i in range(1, n + 1):
            for j in range(1, k + 1):
                dp[i][j][FREE] = max(dp[i - 1][j][FREE], dp[i - 1][j][HOLD] + prices[i - 1])
                dp[i][j][HOLD] = max(dp[i - 1][j][HOLD], dp[i - 1][j - 1][FREE] - prices[i - 1])
            
        return dp[-1][-1][FREE]
```








### [309. 买卖股票的最佳时机含冷冻期](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

给定一个整数数组prices，其中第  prices[i] 表示第 i 天的股票价格 。​

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。
注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。


输入: prices = [1,2,3,0,2]
输出: 3 ，实际就是在原先的 dp 基础上新增一个冷冻状态；

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices:
            return 0
        f0 = float('-inf')
        f1 = 0
        f2 = 0 
        for price in prices:
            f0_n = max(f0, f2 - price)
            f1_n = max(f1, f0 + price)
            f2_n = max(f1, f2)
            f0, f1, f2 = f0_n, f1_n, f2_n
        
        return max(f1, f2)
```














## [714. 买卖股票的最佳时机含手续费](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

给定一个整数数组 prices，其中 prices[i]表示第 i 天的股票价格 ；整数 fee 代表了交易股票的手续费用。

你可以无限次地完成交易，但是你每笔交易都需要付手续费。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。

返回获得利润的最大值。注意：这里的一笔交易指买入持有并卖出股票的整个过程，每笔交易你只需要为支付一次手续费。

输入：prices = [1, 3, 2, 8, 4, 9], fee = 2
输出：8

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        FREE = 0
        HOLD = 1
        n = len(prices)
        dp = [[0, 0] for _ in range(n + 1)]
        dp[0][HOLD] = - prices[0] 
        for i in range(1, n + 1):
            dp[i][FREE] = max(dp[i - 1][FREE], dp[i - 1][HOLD] + prices[i - 1] - fee)
            dp[i][HOLD] = max(dp[i - 1][HOLD], dp[i - 1][FREE] - prices[i - 1])
        return dp[-1][FREE]
```
