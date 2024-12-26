# 回溯 

## 131. 分割回文字符串

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "DP，记录“从i到j的字串是否回文”，然后再回溯。"
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 给你一个字符串 s，请你将 s 分割成一些子串，使每个子串都是 回文串。返回 s 所有可能的分割方案。

 

> 示例 1：
> 
> 输入：`s = "aab"`
> 
> 输出：`[["a","a","b"],["aa","b"]]`


```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        n = len(s)
        dp = [[True for _ in range(n)] for _ in range(n)]

        for i in range(n - 1, -1, -1):
            for j in range(i + 1, n):
                dp[i][j] = (s[i] == s[j]) and dp[i + 1][j - 1]
        
        res = []
        temp = []
        def dfs(idx):
            if idx == n:
                res.append(temp[:])
                return 
            for i in range(idx, n):
                if dp[idx][i]:
                    temp.append(s[idx: i + 1])
                    dfs(i + 1)
                    temp.pop()

        dfs(0)
        return res
```

!!! quote "重要的是dp的过程啊!!因为要返回所有的回文串.. DP的第一个for循环要从后到前面开始。因为从 `i` 到 `j` 的字串是回文，意味着 `s[i] == s[j]` 以及 `s[i + 1: j - 1]`的串是回文的"

----

## [46.全排列](https://leetcode.cn/problems/permutations/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 输入：`nums = [1,2,3]`
> 
> 输出：`[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]`

> 

hehh
```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        result = []
        def backtrack(cur, rest):

            if len(cur) == len(nums):
                result.append(cur)
                return
            for i in range(len(rest)):
                tmp = copy.copy(cur)
                cur.append(rest[i])
                backtrack(cur, rest[:i] + rest[i + 1:])
                cur = tmp

        backtrack([], nums)
        return result

```

!!! quote ""
