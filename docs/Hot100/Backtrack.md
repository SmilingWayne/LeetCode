---
tags:
  - 回溯
---


# 回溯 

## [51. N 皇后](https://leetcode.cn/problems/n-queens/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:crisma; font-weight:bold">High 困难</span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> ![](https://leetcode.cn/problems/n-queens/description/?envType=study-plan-v2&envId=top-100-liked)
> 
> 按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。
> 
> `n` 皇后问题 研究的是如何将 `n` 个皇后放置在 `n×n` 的棋盘上，并且使皇后彼此之间不能相互攻击。
> 
> 给你一个整数 `n` ，返回所有不同的 `n` 皇后问题 的解决方案。
> 
> 每一种解法包含一个不同的 `n` 皇后问题 的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        
        def generateBoard():
            board = []
            for i in range(n):
                rows[queens[i]] = 'Q'
                board.append("".join(rows))
                rows[queens[i]] = '.'
            return board

        def backtrack(idx):
            if idx == n:
                board = generateBoard()
                solutions.append(board)
            else:
                for i in range(n):
                    if i in columns or idx - i in diagnose1 or idx + i in diagnose2:
                        continue
                    columns.add(i)
                    diagnose1.add(idx - i)
                    diagnose2.add(idx + i)
                    queens[idx] = i
                    backtrack(idx + 1)
                    columns.remove(i)
                    diagnose1.remove(idx - i)
                    diagnose2.remove(idx + i)
            

        rows = ["."] * n
        queens = [-1] * n
        columns = set()
        diagnose1 = set()
        diagnose2 = set()
        solutions = []
        backtrack(0)
        return solutions
```

!!! quote "最经典的回溯题。"
    第一，如何表示整个棋盘？一个列表足以。`row = [2,0,1,3]` ，第 `i` 个元素表示第 `i` 行的皇后在第 `row[i]` 列。

    第二，这个回溯每次backtrack之后都是需要恢复原始状态的，表明忽略刚刚的操作。

    第三，terminal 条件：一旦所有的行都被安排了Queen，直接返回；

    第四，也是最重要的，如何判断冲突？一个set来记录已经安排的列，一个set来记录已经安排的左对角线，一个set来记录已经安排的右对角线。`idx - i` 就是被占用的对角线。

    如此，以上。

----


## 131. 分割回文字符串

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "DP，记录“从i到j的字串是否回文”，然后再回溯。"
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

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
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

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
