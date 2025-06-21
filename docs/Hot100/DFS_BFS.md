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


## [394. 字符串解码](https://leetcode.cn/problems/decode-string/description/?envType=study-plan-v2&envId=top-100-liked)

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
