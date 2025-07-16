# DFS/BFS

## [17. 电话号码的数字组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span> 

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。


```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        result = []
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
        
        def dfs(curr, idx):
            if idx == n:
                result.append("".join(curr))
                return 
            for char_ in record[digits[idx]]:
                dfs(curr + [char_], idx + 1)
        
        if n == 0:
            return []
        dfs([], 0)
        return result
```

!!! quote ""
    每次枚举一个元素就行了，注意ending condition。

## [22. 括号生成](https://leetcode.cn/problems/generate-parentheses/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span> 

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。




```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        result = []
        m = 2 * n
        curRoute = [""] * m 
        
        def dfs(idx, temp):
            if idx == m:
                result.append("".join(curRoute))
                return 
            if temp < n:
                curRoute[idx] = "("
                dfs(idx + 1, temp + 1)
            if idx - temp < temp:
                curRoute[idx] = ")"
                dfs(idx + 1, temp)
        
        dfs(0, 0)
        return result

```

!!! quote ""
    做法：枚举下一个元素可能的括号，同时**记录现在已经有的左括号数**（`temp`）。
    
    什么时候能放左括号？很简单，只要当前已经放的左括号数还没到 n 个就行了；
    
    什么时候能放右括号？这个难理解一点：我只要看目前左边已经放的所有括号 （`idx` 个）减去目前已经放的左括号数 ( `temp`)，如果这个数字比 `temp`还要小，说明有一些左括号没有闭合，可以放右括号。反过来讲，如果 `idx - temp == temp` 了，就说明现有的 `idx` 个括号互相闭合了，不能放右括号。

---

## [78. 子集](https://leetcode.cn/problems/subsets/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span> 

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给你一个整数数组 `nums` ，数组中的元素 互不相同 。返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。你可以按 任意顺序 返回解集。

 

示例 1：

> 输入：`nums = [1,2,3]`
> 
> 输出：`[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]`



```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        result = []
        n = len(nums)
        def backtrack(idx, arr):
            result.append(arr)
            for j in range(idx, n):
                backtrack(j + 1, arr + [nums[j]])
        
        backtrack(0, [])
        return result
```

!!! quote ""
    每次向下走，不带筛除的。


## [🌟394. 字符串解码](https://leetcode.cn/problems/decode-string/description/?envType=study-plan-v2&envId=top-100-liked)

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
> 给定一个经过编码的字符串，返回它解码后的字符串。

> 编码规则为: `k[encoded_string]`，表示其中方括号内部的 `encoded_string` 正好重复 `k` 次。注意 `k` 保证为正整数。

> 你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

> 此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 `k` ，例如不会出现像 `3a` 或 `2[4]` 的输入。

 


> 输入：`s = "3[a]2[bc]"`
> 
> 输出：`"aaabcbc"`

> 


```python
class Solution:
    def decodeString(self, s: str) -> str:

        def dfs(s, idx):
            res = ""
            multi = 0
            while idx < len(s):
                if "0" <= s[idx] <= "9" :
                    multi = multi * 10 + int(s[idx])
                elif s[idx] == "[":
                    idx, tmp = dfs(s, idx + 1)

                    res += multi * tmp
                    multi = 0
                elif s[idx] == "]":

                    return idx, res
                else:
                    res += s[idx]
                idx += 1
            return idx, res

        idx, sol = dfs(s, 0)
        return sol

    
```

!!! quote "第一次写一定会觉得很麻烦不好做的题。第一想法就是递归（虽然题目在stack部分。根据 `[]` 的位置判断递归的情况。遇到  `[` 就进入下一层，在下一层返回当前的索引位置以及字符串；遇到 `]` 就直接停止。"


## [433. 最小基因变化](https://leetcode.cn/problems/minimum-genetic-mutation/?envType=study-plan-v2&envId=top-interview-150)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

基因序列可以表示为一条由 8 个字符组成的字符串，其中每个字符都是 'A'、'C'、'G' 和 'T' 之一。

假设我们需要调查从基因序列 start 变为 end 所发生的基因变化。一次基因变化就意味着这个基因序列中的一个字符发生了变化。

例如，"AACCGGTT" --> "AACCGGTA" 就是一次基因变化。
另有一个基因库 bank 记录了所有有效的基因变化，只有基因库中的基因才是有效的基因序列。（变化后的基因必须位于基因库 bank 中）

给你两个基因序列 start 和 end ，以及一个基因库 bank ，请你找出并返回能够使 start 变化为 end 所需的最少变化次数。如果无法完成此基因变化，返回 -1 。

注意：起始基因序列 start 默认是有效的，但是它并不一定会出现在基因库中。

 

示例 1：

输入：start = "AACCGGTT", end = "AACCGGTA", bank = ["AACCGGTA"]
输出：1

```python
class Solution:
    def minMutation(self, startGene: str, endGene: str, bank: List[str]) -> int:
        if startGene == endGene:
            return 0
        if endGene not in bank:
            return -1
        
        queue = deque([startGene])
        visited = set([startGene])
        step = 0
        while queue:
            l = len(queue)
            for i in range(l):
                cur_g = queue.popleft()
                if cur_g == endGene:
                    return step
                for j in range(8):
                    for k in ["A", "G", "C", "T"]:
                        if cur_g[j] == k:
                            continue
                        next_g = cur_g[: j] + k + cur_g[j + 1: ]
                        if next_g not in visited and next_g in bank:
                            visited.add(next_g)
                            queue.append(next_g)
            step += 1
        return -1
```

!!! quote ""


## [909. 蛇梯棋](https://leetcode.cn/problems/snakes-and-ladders/?envType=study-plan-v2&envId=top-interview-150)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给你一个大小为 n x n 的整数矩阵 board ，方格按从 1 到 n2 编号，编号遵循 转行交替方式 ，从左下角开始 （即，从 board[n - 1][0] 开始）的每一行改变方向。

你一开始位于棋盘上的方格  1。每一回合，玩家需要从当前方格 curr 开始出发，按下述要求前进：

选定目标方格 next ，目标方格的编号在范围 [curr + 1, min(curr + 6, n2)] 。
该选择模拟了掷 六面体骰子 的情景，无论棋盘大小如何，玩家最多只能有 6 个目的地。
传送玩家：如果目标方格 next 处存在蛇或梯子，那么玩家会传送到蛇或梯子的目的地。否则，玩家传送到目标方格 next 。 
当玩家到达编号 n2 的方格时，游戏结束。
如果 board[r][c] != -1 ，位于 r 行 c 列的棋盘格中可能存在 “蛇” 或 “梯子”。那个蛇或梯子的目的地将会是 board[r][c]。编号为 1 和 n2 的方格不是任何蛇或梯子的起点。

注意，玩家在每次掷骰的前进过程中最多只能爬过蛇或梯子一次：就算目的地是另一条蛇或梯子的起点，玩家也 不能 继续移动。

举个例子，假设棋盘是 [[-1,4],[-1,3]] ，第一次移动，玩家的目标方格是 2 。那么这个玩家将会顺着梯子到达方格 3 ，但 不能 顺着方格 3 上的梯子前往方格 4 。（简单来说，类似飞行棋，玩家掷出骰子点数后移动对应格数，遇到单向的路径（即梯子或蛇）可以直接跳到路径的终点，但如果多个路径首尾相连，也不能连续跳多个路径）
返回达到编号为 n2 的方格所需的最少掷骰次数，如果不可能，则返回 -1。

```python
class Solution:
    def snakesAndLadders(self, board: List[List[int]]) -> int:
        n = len(board)
        visited = [False] * (n * n + 1)
        q = [1]
        step = 0
        while q:
            tmp = q
            q = []
            for x in tmp:
                if x == n * n:
                    return step
                for y in range(x + 1, min(n * n, x + 6) + 1):
                    r, c = divmod(y - 1, n )
                    if r % 2:
                        c = n - 1 - c
                    nxt = board[-1 - r][c]
                    if nxt < 0:
                        nxt = y
                    if not visited[nxt]:
                        visited[nxt] = True
                        q.append(nxt)
            step += 1
        return -1
                    
```

!!! quote "重要的是把这个编码规则实现了！！！"
