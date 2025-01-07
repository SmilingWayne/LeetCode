---
tags:
  - 动态规划
---

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


-----

## [3202. 找出有效子序列的最大长度 II](https://leetcode.cn/problems/find-the-maximum-length-of-valid-subsequence-ii/description/)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span> 

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 给你一个整数数组 nums 和一个 正 整数 $k$ 。nums 的一个 子序列sub 的长度为 $x$ ，如果其满足以下条件，则称其为 有效子序列 ：
> 
> ($sub[0] + sub[1]) % k == (sub[1] + sub[2]) % k == ... == (sub[x - 2] + sub[x - 1]) % k$ 返回 $nums$ 的 最长有效子序列 的长度。
> 


```python
class Solution:
    def maximumLength(self, nums: List[int], k: int) -> int:
        n = len(nums)
        dp = [[1] * k for _ in range(n)]
        res = 2
        for i in range(1, n):
            for j in range(i):
                dp[i][(nums[i] + nums[j]) % k] = dp[j][(nums[i] + nums[j]) % k] + 1
                res = max(res, dp[i][(nums[i] + nums[j]) % k])
        return res
        
```

!!! quote "怎么理解"
    这里题目暗含一个假设，所有偶数位的数字的奇偶性和所有奇数位的奇偶性是相同的。换句话说，如果确定了最终选中的序列中的倒数第一和第二个数字，那么“最长的序列”的模数就知道了。


    假设`nums[j], nums[i]`为当前子序列的最后两个元素，这样就确定了序列相邻元素模k的值，`dp[i][(nums[i] + nums[j]) % k] = dp[j][(nums[i] + nums[j]) % k] + 1`，`dp[i][m]`表示以`nums[i]`结尾且序列相邻元素模`k`值均为`m`的最长序列长度。

    > 上面遍历的第二个for循环，就是在检查以`j`作为倒数第二个数字时的子序列长度。

----

## [5. 最长回文子串🌟](https://leetcode.cn/problems/longest-palindromic-substring/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 
> 给你一个字符串 s，找到 s 中最长的 回文子串。
> 
> 输入：s = "babad"
> 
> 输出："bab"
> 
> 解释："aba" 同样是符合题意的答案。
> 



```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        n = len(s)
        if n < 2:
            return s
        max_len = 1
        left = 0; right = 0
        dp = [[False] * n for _ in range(n)]
        for j in range(n):
            for i in range(j, -1, -1):
                if j - 1 > i:
                    dp[i][j] = dp[i + 1][j - 1] and s[i] == s[j]
                else:
                    dp[i][j] = (s[i] == s[j])
                
                if dp[i][j] and j - i + 1 > max_len:
                    left = i
                    right = j 
                    max_len = j - i + 1
        return s[left: right + 1]

```

!!! quote "因为要返回字符串，所以必须找到一对上下标。同时，这个需要两层遍历，一个用来锚定“当前的最后一个字符所在位置”，一个找“前向的可能与这个字符串匹配并且中间夹着的部分都是回文”的“第一个字符”所在的位置。这里的dp表存储的是从 $i$ 到 $j$ 的字符串是否是回文串。"

