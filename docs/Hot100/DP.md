---
tags:
  - 动态规划
---

# 动态规划

## 打家劫舍系列

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    [打家劫舍 I](https://leetcode.cn/problems/house-robber/submissions/601261199/?envType=study-plan-v2&envId=dynamic-programming) 
    
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span> 

    [打家劫舍 II](https://leetcode.cn/problems/house-robber-ii/) 
    
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span> 

    [打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/description/) 
    
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span> 

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

- I. 计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        dp = [0 for _ in range(n + 1)]
        for i in range(n):
            dp[i + 1] = max(dp[i], dp[i - 1] + nums[i]) # 位置 i 所获得的最高金额，等于前一个的和倒数第二个+自己
        return dp[-1]
```

- II. 同上，但所有的房屋围成一圈 ，这意味着第一个房屋和最后一个房屋是紧挨着的，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。

!!! example "==两次遍历==，可以复用I的代码，但是 n = max([2:-1] + num[0], [1:-1]) ；注意可以用常数级别空间进行解决。"

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        def temp(arr):
            n = len(arr)
            f0 = f1 = 0
            
            for i in range(n):
                new_f = max(f1, f0 + arr[i])
                f0 = f1
                f1 = new_f
            return f1

        a = temp(nums[1:]) 
        b = temp(nums[2: -1]) + nums[0]
        return max(a, b)
```

- III. 在二叉树上，相连的节点不能同时被偷窃。

!!! example "每个节点会有两个状态，每次递归的时候分别返回这两个状态下的最大值，然后当前节点的也按照同理进行计算。"

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        def helper(head):
            if not head:
                return 0, 0
            l_rob, l_not_rob = helper(head.left)
            r_rob, r_not_rob = helper(head.right)
            rob = head.val + l_not_rob + r_not_rob
            not_rob = max(l_rob, l_not_rob) + max(r_rob, r_not_rob)
            return rob, not_rob
        return max(helper(root))
            
```



!!! quote ""

-------



## [322.零钱兑换🌟](https://leetcode.cn/problems/coin-change/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "递推表"
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

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
