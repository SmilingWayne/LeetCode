---
tags:
  - 动态规划
---

# 动态规划








-----

## [3202. 找出有效子序列的最大长度 II](https://leetcode.cn/problems/find-the-maximum-length-of-valid-subsequence-ii/description/)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span> 

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
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

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

---


## [152. 乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给你一个整数数组 nums ，请你找出数组中乘积最大的非空连续 子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

测试用例的答案是一个 32-位 整数。

 

示例 1:

输入: `nums = [2,3,-2,4]`
输出: `6`
解释: 子数组 `[2,3]` 有最大乘积 `6`。


```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        leftMax = 1
        leftMin = 1
        res = float('-inf')
        for num in nums:
            if num < 0:
                leftMax, leftMin = leftMin, leftMax
                leftMax = max(leftMax * num, num)
                leftMin = min(leftMin * num, num)
            else:
                leftMax = max(leftMax * num, num)
                leftMin = min(leftMin * num, num)
            res = max(res, leftMax)
        return res
```

!!! quote ""
    数字有两种情况：非负和负数。我们记录在当前数字前面的元素构成的列表的最大的乘积 leftMax，和当前数字前面的最小的乘积 leftMin。
    
    1. 如果这个数字是正数，那么，截止到这个数字时的最大乘积有2种情况： （1）前面有一个大数，乘之后更大了，（2）这个正数比前面所有元素的最大乘积还要大（这可以理解，因为前面的乘积还可能是负数嘛；同理，截止到这个数字时的最小乘积有2种情况：（1）前面有一个数，乘之后更小了（比如原先是个负数），（2）这个正数比前面所有元素的最小乘积还要小（这也可以理解，因为前面的乘积还可能是正数嘛！）； 
        1. 上面的就可以写成：leftMax = max(leftMax * num, num); leftMin = min(leftMin * num, num)
    2. **如果这个数字是负数**，分析思路是一样的，但是要提前交换一下  leftMin, leftMax。为什么？
        1. 因为截止到这个数字时候的最大乘积，不可能是负数乘以原先的大数字得到的（原先是 +,-，那么一定是负数乘负数得到的更大；原先是 -,-，那么一定是负得更多的那个得到的更大；原先是+，+，那一定是正得不那么多的得到的更大）；
        2. 因为截止到这个数字时候的最小乘积，不可能是负数乘以原先的小数字得到的（原先是 +,-，那么一定是负数乘正数得到的更小；原先是 -,-，那么一定是负得更少的那个乘出来的更小；原先是+，+，那一定是正更多的那个得的得到的更小）；
        3. 于是我们有了最开始的互换操作。

---

## [300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/submissions/640054099/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。

子序列 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。




```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        
        arr = []
        for num in nums:
            if not arr or num > arr[-1]:
                arr.append(num)
            else:
                l = 0; r = len(arr) - 1
                while l <= r:
                    mid = l + (r - l) // 2
                    if arr[mid] >= num:
                        r = mid - 1
                    else:
                        l = mid + 1
                arr[l] = num 
        return len(arr)

```

!!! quote ""
    始终维护一个“增长得最慢”的列表。


---


## 买卖股票专题

直接上最困难的版本：

给你一个整数数组 prices 和一个整数 k ，其中 prices[i] 是某支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 k 笔交易。也就是说，你最多可以买 k 次，卖 k 次。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。


我们首先定义关键的：一次交易，买入一个股票然后售出，才算一次交易，仅仅买入股票没有售出，那么即使这是第 k + 1 次买入，也只能算进行了 k 次交易。

1. `buys[i][j]` ，描述在第 $i$ 天，进行了 $j$ 次交易，==且手头持有股票时的最大收益== ；
2. `sells[i][j]` ，描述在第 $i$ 天，进行了 $j$ 次交易，==且手头不持有股票时的最大收益== ；

> 上述划分方法确定了，同一个 j 下，卖出一定是在买入的状态之后的；

1. 你需要声明 $k + 1$ 长度的，来为一些前后处理留空间；
2. 在第一天的时候，你不可能执行多次交易，所以，[1, k] 次交易的值全部是 - inf；

在此后的每一天，对于你进行了 j 次交易时，有状态转移方程：

`buys[i][j] = max( buys[i - 1][j], - prices[i] + sells[i - 1][j])`

`sells[i][j] = max(sells[i - 1][j], prices[i] + buys[i - 1][j - 1])`

也就是，我在 第 $i$ 天，如果进行了 $j$ 次，为了求手头持有股票时的最大收益，这个收益： 1）要么我当天没有买股票，我的收益等于前一天，同样已经进行了 j 次交易时的那个收益； 2）我当天买股票了，由于我没有卖出，所以这一份买入没有产生一次完整的交易（尽管买入了一笔新的，但是还是只进行了 j 次交易，所以此时的收益相当于前一天已经进行了 j 次交易时那份卖出的收益，减去我这笔交易的成本；

我在 第 $i$ 天，如果进行了 $j$ 次，为了求手头不持有股票时的最大收益，这个收益： 1）要么我当天没有卖股票，我的收益等于前一天，同样已经进行了 j 次交易，且手头没股票时的那个收益； 2）我**当天卖股票**了，这下是我卖出了的结果，此次卖出就是第 j 次卖出，所以这一份卖出后的收益相当于前一天已经进行了 j - 1 次交易时的那个买入（已经包含了这一次的成本了！）加上我这笔交易的利润；

处理第 1 天时的问题，此时不可能存在一次完整的交易，所以 [1, k] 的值都是 负无穷。同时还需要处理边界，即为0天时后的情况。

```python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        FREE, HOLD = 0, 1
        n = len(prices)
        dp = [[[0] * 2 for _ in range(k + 1)] for _ in range(n + 1)]
        for i in range(k + 1):
            dp[0][i][HOLD] = - prices[0]
        for i in range(1, n + 1):
            for j in range(1, k + 1):
                dp[i][j][FREE] = max(dp[i - 1][j][FREE], dp[i - 1][j][HOLD] + prices[i - 1])
                dp[i][j][HOLD] = max(dp[i - 1][j][HOLD], dp[i - 1][j - 1][FREE] - prices[i - 1])
        return dp[-1][-1][FREE]
        
```


### [3573. 买卖股票的最佳时机 V](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-v/description/)

给你一个整数数组 prices，其中 prices[i] 是第 i 天股票的价格（美元），以及一个整数 k。

你最多可以进行 k 笔交易，每笔交易可以是以下任一类型：

普通交易：在第 i 天买入，然后在之后的第 j 天卖出，其中 i < j。你的利润是 prices[j] - prices[i]。

做空交易：在第 i 天卖出，然后在之后的第 j 天买回，其中 i < j。你的利润是 prices[i] - prices[j]。

注意：你必须在开始下一笔交易之前完成当前交易。此外，你不能在已经进行买入或卖出操作的同一天再次进行买入或卖出操作。

通过进行 最多 k 笔交易，返回你可以获得的最大总利润。

输入: prices = [1,7,9,8,2], k = 2

输出: 14

解释:

我们可以通过 2 笔交易获得 14 美元的利润：
一笔普通交易：第 0 天以 1 美元买入，第 2 天以 9 美元卖出。
一笔做空交易：第 3 天以 8 美元卖出，第 4 天以 2 美元买回。

> 这里展示最终我确定下来我做这道题的模板，感谢[Storm老师的题解](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-v/solutions/3695625/3573-mai-mai-gu-piao-de-zui-jia-shi-ji-v-gtyu/)。

```python
class Solution:
    def maximumProfit(self, prices: List[int], k: int) -> int:
        FREE, HOLD, SHORT_SELL = 0, 1, 2
        n = len(prices)
        dp = [[[0] * 3 for _ in range(k + 1)] for _ in range(n + 1)]
        for i in range(k + 1):
            dp[0][i][HOLD] = - prices[0]
            dp[0][i][SHORT_SELL] = prices[0]
        for i in range(1, n + 1):
            for j in range(1, k + 1):
                dp[i][j][FREE] = max(dp[i - 1][j][FREE], dp[i - 1][j][HOLD] + prices[i - 1],  dp[i - 1][j][SHORT_SELL] - prices[i - 1] )
                dp[i][j][HOLD] = max(dp[i - 1][j][HOLD], dp[i - 1][j - 1][FREE] - prices[i - 1])
                dp[i][j][SHORT_SELL] = max(dp[i - 1][j][SHORT_SELL], dp[i - 1][j - 1][FREE] + prices[i - 1])
        return dp[-1][-1][0]
```
