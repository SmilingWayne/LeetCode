---
tags:
  - 贪心算法
---

## [55. 跳跃游戏](https://leetcode.cn/problems/jump-game/description/?envType=study-plan-v2&envId=top-100-liked)

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
> 给你一个非负整数数组 nums ，你最初位于数组的 第一个下标 。数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标，如果可以，返回 true ；否则，返回 false 。

 

示例 1：

> 输入：`nums = [2,3,1,1,4]`
> 
> 输出：`true`
> 
> 解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。



```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        res = nums[0]
        n = len(nums)
        if n == 1:
            return True
        for i in range(1, n):
            if i > res:
                return False
            res = max(res, i + nums[i])
        if res >= n - 1:
            return True
        return False
```

!!! quote "一开始想得复杂了啊..只需要看最远能走多远就行了。因为你可以任选跳跃的步长...所以只需要记录“当前最远抵达哪里”。这里需要注意处理长度为1的情况。"

----

## [763. 划分字母区间]{https://leetcode.cn/problems/partition-labels/description/?envType=study-plan-v2&envId=top-100-liked}

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度： <span style = "color:gold; font-weight:bold">Medium 中等 </span> 

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

> 给你一个字符串 `s` 。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。
> 
> 注意，划分结果需要满足：将所有划分结果按顺序连接，得到的字符串仍然是 s 。
> 
> 返回一个表示每个字符串片段的长度的列表。

 

> 示例 1：
> 输入：`s = "ababcbacadefegdehijhklij"`
> 输出：`[9,7,8]`
> 解释：
> 划分结果为 `"ababcbaca"、"defegde"、"hijhklij"` 。
> 每个字母最多出现在一个片段中。
> 像 `"ababcbacadefegde", "hijhklij"` 这样的划分是错误的，因为划分的片段数较少。 

```python
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        result = []
        n = len(s)
        last = [0 for _ in range(26)]
        for i in range(n):
            last[ord(s[i]) - ord('a')] = i 
        
        end = last[ord(s[0]) - ord('a')]
        start = 0
        for i in range(n):
            end = max(last[ord(s[i]) - ord('a')], end)
            if i == end:
                result.append(end - start + 1)
                start = end + 1
        return result


```

!!! quote "最开始还想着需要排序，后来发现不需要，最开始的时候记录一下，每个字母的最后出现的位置，然后从头到尾开始iterate，遇到一个字母，就看看，当前的右边pivot是否是当前字母的最后出现的位置，如果是，那么就说明可以划分了，否则就继续iterate，并更新当前的右端pivot即可。"


-----


## [45. 跳跃游戏 II](https://leetcode.cn/problems/jump-game-ii/description/?envType=study-plan-v2&envId=top-interview-150)

## 

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给定一个长度为 n 的 0 索引整数数组 nums。初始位置为 nums[0]。

每个元素 nums[i] 表示从索引 i 向后跳转的最大长度。换句话说，如果你在 nums[i] 处，你可以跳转到任意 nums[i + j] 处:

0 <= j <= nums[i] 
i + j < n
返回到达 nums[n - 1] 的最小跳跃次数。生成的测试用例可以到达 nums[n - 1]。

 

示例 1:

输入: nums = [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。


```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        cnt = 0
        curr = 0
        curmax = 0
        for i in range(len(nums) - 1):
            curmax = max(curmax, i + nums[i])
            if i == curr:
                cnt += 1
                curr = curmax  
        return cnt
```

!!! quote ""
