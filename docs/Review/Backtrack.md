---
tags:
  - 回溯
---


# 勇闯秋招手撕系列 VII：回溯法

!!! abstract ""
    <span style="color:orange;font-weight:bold">组合 / 组合总和</span>

    <span style="color:orange;font-weight:bold">全排列（🌟）</span>

    <span style="color:orange;font-weight:bold">不重复的全排列（🌟）</span>

    <span style="color:orange;font-weight:bold">子集</span>

    <span style="color:orange;font-weight:bold">括号生成</span>

    <span style="color:red;font-weight:bold">N皇后</span>

    <span style="color:orange;font-weight:bold">分割回文字符串</span>

    <span style="color:orange;font-weight:bold">单词搜索（🌟）</span>

    <span style="color:green;font-weight:bold">电话号码的字符组合</span>

![](https://cdn.jsdelivr.net/gh/SmilingWayne/picsrepo/202512091556187.png)


## [39. 组合总和](https://leetcode.cn/problems/combination-sum/?envType=study-plan-v2&envId=top-100-liked)


难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span> ；给你一个 无重复元素 的整数数组 `candidates` 和一个目标整数 `target` ，找出 `candidates` 中可以使数字和为目标数 `target` 的 所有 不同组合 ，并以列表形式返回。你可以按 任意顺序 返回这些组合。

`candidates` 中的 同一个 数字可以 无限制重复被选取 。如果至少一个数字的被选数量不同，则两种组合是不同的。 


```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []
        n = len(candidates)
        def dfs(curr, curr_list, idx):
            if curr > target:
                return 
            
            if curr == target:
                res.append(curr_list)
                return 
            
            for i in range(idx, n):
                dfs(curr + candidates[i], curr_list + [candidates[i]], i )
        
        dfs(0, [], 0)
        return res
```

!!! quote ""
    思路就是每次放一个数字，但是要保证递归下去的时候总是从当前数字向后开始递归。




## [77. 组合](https://leetcode.cn/problems/combinations/description/?envType=study-plan-v2&envId=top-interview-150)

给定两个整数 n 和 k，返回范围 [1, n] 中所有可能的 k 个数的组合。

你可以按 任何顺序 返回答案。

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        res = []

        def dfs(idx, curr):
            if len(curr) == k:
                res.append(curr.copy())
                return 
            for i in range(idx, n + 1):
                dfs(i + 1, curr + [i])
        
        dfs(1, [])
        return res
```


----



## [17. 电话号码的字符组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number?envType=study-plan-v2&envId=top-100-liked)

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        res = []
        n = len(digits)
        record = {
            "1": "",
            "2": "abc",
            "3": "def",
            "4": "ghi",
            "5": "jkl",
            "6": "mno",
            "7": "pqrs",
            "8": "tuv", 
            "9": "wxyz"
        }
        curr = ["" for _ in range(n)]

        def dfs(idx):
            if idx == n:
                res.append("".join(curr.copy()))
                return 
            for ch in record[digits[idx]]:
                curr[idx] = ch 
                dfs(idx + 1)
        dfs(0)
        return res
```

---




## [51. N 皇后](https://leetcode.cn/problems/n-queens/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- ![](https://leetcode.cn/problems/n-queens/description/?envType=study-plan-v2&envId=top-100-liked) -->

难度：<span style = "color:red; font-weight:bold">High 困难</span>；按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。给你一个整数 `n` ，返回所有不同的 `n` 皇后问题 的解决方案。


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
        rows = ["."] * n; queens = [-1] * n
        columns = set()
        diagnose1 = set(); diagnose2 = set(); solutions = []
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


## [131. 分割回文字符串](https://leetcode.cn/problems/palindrome-partitioning/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->


难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>;给你一个字符串 s，请你将 s 分割成一些子串，使每个子串都是 回文串。返回 s 所有可能的分割方案。

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

## [46. 全排列](https://leetcode.cn/problems/permutations/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>


> 示例1:
> 输入：`nums = [1,2,3]`
> 
> 输出：`[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]`

> 

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


---

## [47. 全排列 II：不重复的全排列](https://leetcode.cn/problems/permutations-ii/description/)

给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。


输入：`nums = [1,1,2]`
输出：`[[1,1,2],[1,2,1],[2,1,1]]`

```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        nums.sort()
        path = [0 for _ in range(n)]
        on_path = [False for _ in range(n)]
        res = []
        def dfs(idx):
            if idx == n:
                res.append(path.copy())
                return 
            
            for i, on in enumerate(on_path):
                if on or i > 0 and nums[i] == nums[i - 1] and not on_path[i - 1]:
                    continue 
                path[idx] = nums[i]
                on_path[i] = True
                dfs(idx + 1)
                on_path[i] = False 
        
        dfs(0)
        return res

```






## [78. 子集](https://leetcode.cn/problems/subsets/?envType=study-plan-v2&envId=top-100-liked)

给你一个整数数组 nums ，数组中的元素 互不相同 。返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。你可以按 任意顺序 返回解集。

输入：`nums = [1,2,3]`
输出：`[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]`

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        res = []
        def dfs(idx, curr):
            res.append(curr.copy())
            for i in range(idx, n):
                dfs(i + 1, curr + [nums[i]])
        dfs(0, [])
        return res
```




## [22. 括号生成](https://leetcode.cn/problems/generate-parentheses/?envType=study-plan-v2&envId=top-100-liked)

数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

输入：`n = 3`
输出：`["((()))","(()())","(())()","()(())","()()()"]`

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        res = []
        m = 2 * n
        curr = ["" for _ in range(m)]

        def dfs(idx, cnt):
            if idx == m:
                res.append("".join(curr.copy()))
                return 
            if cnt < n:
                curr[idx] = "("
                dfs(idx + 1, cnt + 1)
            if m - idx - 1 >= 2 * (n - cnt):
                curr[idx] = ")"
                dfs(idx + 1, cnt)
        dfs(0, 0)
        return res
```



---


## [79. 单词搜索](https://leetcode.cn/problems/word-search/description/?envType=study-plan-v2&envId=top-100-liked)

给定一个 m x n 二维字符网格 board 和一个字符串单词 word 。如果 word 存在于网格中，返回 true ；否则，返回 false 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。


```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        m = len(board)
        n = len(board[0])
        k = len(word)

        def dfs(i, j, idx):
            if idx == k:
                return True
            a1, a2, a3, a4 = False, False, False, False
            if 0 <= i < m and 0 <= j < n and not visited[i][j] and  board[i][j] == word[idx]:
                visited[i][j] = True
                a1 = dfs(i + 1, j, idx + 1)
                a2 = dfs(i - 1, j, idx + 1)
                a3 = dfs(i, j + 1, idx + 1)
                a4 = dfs(i, j - 1, idx + 1)
                visited[i][j] = False 
            return a1 or a2 or a3 or a4 
        
        visited = [[False] * n for _ in range(m)]
        for i in range(m):
            for j in range(n):
                if board[i][j] == word[0]:
                    check = dfs(i, j, 0)
                    if check:
                        return True 
        return False
```



