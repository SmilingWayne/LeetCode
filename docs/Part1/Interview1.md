

## 08. 08 有重复字符串的排列组合

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "回溯｜减枝"
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">中等</span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

有重复字符串的排列组合。编写一种方法，计算某字符串的所有排列组合。

> 示例1:
> 
> 
> 输入：S = "qqe"
> 
> 输出：["eqq","qeq","qqe"]


```python
class Solution:
    def permutation(self, S: str) -> List[str]:
        def backtrack(S, cur, chosen):
            if len(cur) == len(S):
                return ["".join(cur)]
            else:
                res = []
                cut = set()
                for i in range(len(S)):
                    if S[i] in cut or chosen[i]:
                        continue
                    chosen[i] = True
                    cur.append(S[i])
                    res += backtrack(S, cur, chosen)
                    cur.pop()
                    chosen[i] = False
                    cut.add(S[i])
                return res
        return backtrack(S, [], [False for _ in range(len(S))])
        
```

!!! quote "每次回溯的时候，有一些情况是不需要尝试的：<br> 1. 要换入的这个位置的字符之前被使用过了; <br> 2. 换入的字符没有被用过，但是出现了同样的字符。"


## 08.07. 无重复字符串的排列组合

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "回溯"
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

无重复字符串的排列组合。编写一种方法，计算某字符串的所有排列组合，字符串每个字符均不相同。


> 示例1:
> 
> 输入：S = "qwe"
> 
> 输出：["qwe", "qew", "wqe", "weq", "ewq", "eqw"]


```python
class Solution:
    def permutation(self, S: str) -> List[str]:
        n = len(S)
        def backtrack(S, cur):
            res = []
            if len(cur) == n:
                return ["".join(cur)]
            else:
                for i in range(len(S)):
                    cur.append(S[i])
                    res += backtrack(S[:i] + S[i + 1:], cur)
                    cur.pop()
                return res
        return backtrack(S, [])
```

!!! quote "模板！"
