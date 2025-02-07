# DFS/BFS

## [394. 字符串解码](https://leetcode.cn/problems/decode-string/description/?envType=study-plan-v2&envId=top-100-liked)

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
