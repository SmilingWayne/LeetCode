# 动态规划

## [322.零钱兑换](https://leetcode.cn/problems/coin-change/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "递推表"
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 
> 给你一个整数数组 coins ，表示不同面额的硬币；以及一个整数 amount ，表示总金额。
> 
> 计算并返回可以凑成总金额所需的 最少的硬币个数 。如果没有任何一种硬币组合能组成总金额，返回 -1 。
> 
> 你可以认为每种硬币的数量是无限的。
> 
> 示例 1：
> 
> 输入：coins = [1, 2, 5], amount = 11
> 
> 输出：3 
> 
> 解释：11 = 5 + 5 + 1
> 


```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [[inf] * (amount + 1) for _ in range(len(coins) + 1)]
        dp[0][0] = 0
        for idx, coin in enumerate(coins):
            for j in range(amount + 1):
                if coin > j:
                    dp[idx + 1][j] = dp[idx][j]
                else:
                    dp[idx + 1][j] = min(dp[idx + 1][j - coin] + 1, dp[idx][j])
        ans = dp[len(coins)][amount]
        return ans if ans < inf else -1

```

!!! quote ""
