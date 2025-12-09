# 勇闯秋招手撕系列 IV：动态规划 1️⃣

!!! abstract ""
    <span style="color:orange;font-weight:bold">零钱兑换（🌟🌟）</span>

    <span style="color:orange;font-weight:bold">完全平方数（🌟）</span>

    <span style="color:orange;font-weight:bold">最长递增子序列（🌟🌟）</span>

    <span style="color:orange;font-weight:bold">最长回文子串（🌟🌟）</span>

    <span style="color:orange;font-weight:bold">最长公共子序列（🌟）</span>

    <span style="color:orange;font-weight:bold">乘积最大子数组</span>

    <span style="color:orange;font-weight:bold">编辑距离（🌟）</span>

    <span style="color:orange;font-weight:bold">分割等和子集</span>

    <span style="color:orange;font-weight:bold">打家劫舍系列</span>

![](https://cdn.jsdelivr.net/gh/SmilingWayne/picsrepo/202512091611508.png)

## [322.零钱兑换🌟](https://leetcode.cn/problems/coin-change/description/?envType=study-plan-v2&envId=top-100-liked)


> 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>,给你一个整数数组 coins ，表示不同面额的硬币；以及一个整数 amount ，表示总金额。返回可以凑成总金额所需的 最少的硬币个数，若无法组成总金额，返回 -1 。每种硬币的数量是无限的。
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
                    dp[idx + 1][j] = min(dp[idx + 1][j - coin] + 1, 
                    dp[idx][j])
        ans = dp[len(coins)][amount]
        return ans if ans < inf else -1
```
## [279. 完全平方数](https://leetcode.cn/problems/perfect-squares/?envType=study-plan-v2&envId=top-100-liked)


给你一个整数 n ，返回 和为 n 的完全平方数的最少数量 。

```python
class Solution:
    def numSquares(self, n: int) -> int:
        dp = [i for i in range(n + 1)]
        dp[0] = 0
        for i in range(2, int(math.sqrt(n)) + 1):
            for j in range(n + 1):
                if j >= i * i:
                    dp[j] = min(dp[j], dp[j - i * i] + 1)
        return dp[n]
```


## [300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/submissions/640054099/?envType=study-plan-v2&envId=top-100-liked)


给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。子序列 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。

难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>，最快做法是二分，始终维护一个“增长得最慢”的列表，也可DP；

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
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        if len(nums) == 0:
            return 1 
        n = len(nums)
        dp = [1 for _ in range(n)]
        for i in range(1, n):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j] + 1)
        return max(dp)
```

## [5. 最长回文子串🌟](https://leetcode.cn/problems/longest-palindromic-substring/description/?envType=study-plan-v2&envId=top-100-liked)


> 示例1:
> 
> 给你一个字符串 s，找到 s 中最长的 回文子串。

🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

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


## [1143.最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence?envType=study-plan-v2&envId=top-100-liked)

给定两个字符串 text1 和 text2，返回这两个字符串的最长 公共子序列 的长度。如果不存在 公共子序列 ，返回 0 。


```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m = len(text1)
        n = len(text2)
        dp = [[0 for _ in range(n + 1)] for _ in range(m + 1)]
        
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if text1[i - 1] == text2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1] + 1
                else:
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
        return dp[-1][-1]
```

## [152. 乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/?envType=study-plan-v2&envId=top-100-liked)


给你一个整数数组 nums ，请你找出数组中乘积最大的非空连续 子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

测试用例的答案是一个 32-位 整数。

🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

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

## [72. 编辑距离](https://leetcode.cn/problems/edit-distance/description/?envType=study-plan-v2&envId=top-100-liked)

给你两个单词 word1 和 word2， 请返回将 word1 转换成 word2 所使用的最少操作数。

你可以对一个单词进行如下三种操作：

插入一个字符
删除一个字符
替换一个字符

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        m = len(word1)
        n = len(word2)

        dp = [[0 for _ in range(n + 1)] for _ in range(m + 1)]

        for i in range(m):
            dp[i + 1][0] = i + 1
        for i in range(n):
            dp[0][i + 1] = i + 1 
        
        for i in range(m):
            for j in range(n):
                if word1[i] == word2[j]:
                    dp[i + 1][j + 1] = dp[i][j]
                else:
                    dp[i + 1][j + 1] = min(dp[i + 1][j], dp[i][j + 1], 
                    dp[i][j]) + 1
        return dp[-1][-1]

```

## [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/description/?envType=study-plan-v2&envId=top-100-liked)

给你一个 只包含正整数 的 非空 数组 nums 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

另一种形式的零钱兑换。

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        sum_ = sum(nums)
        if sum_ % 2 == 1:
            return False 
        target = sum_ // 2 
        dp = [False for _ in range(target + 1)]
        dp[0] = True 
        for num in nums:
            temp = dp.copy()
            for i in range(target + 1):
                if dp[i] and i + num < target + 1:
                    if i + num == target:
                        return True
                    temp[i + num] = True
            dp = temp.copy()
        return False
                    


```



## 打家劫舍系列


[打家劫舍 I](https://leetcode.cn/problems/house-robber/submissions/601261199/?envType=study-plan-v2&envId=dynamic-programming)  盗贼计划偷窃沿街的房屋。每间房内都藏有一定的现金，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        dp = [0 for _ in range(n + 1)]
        for i in range(n):
            dp[i + 1] = max(dp[i], dp[i - 1] + nums[i]) # 位置 i 所获得的最高金额，等于前一个的和倒数第二个+自己
        return dp[-1]
```

[打家劫舍 II](https://leetcode.cn/problems/house-robber-ii/) ；🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>；同上，但所有的房屋围成一圈 ，这意味着第一个房屋和最后一个房屋是紧挨着的，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。

> ==两次遍历==，可以复用I的代码，但是 n = max([2:-1] + num[0], [1:-1]) ；注意可以用常数级别空间进行解决。"

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

[打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/description/) 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span> ； 在二叉树上，相连的节点不能同时被偷窃。

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