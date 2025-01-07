---
tags:
  - 贪心算法
---

## [55. 跳跃游戏](https://leetcode.cn/problems/jump-game/description/?envType=study-plan-v2&envId=top-100-liked)

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
