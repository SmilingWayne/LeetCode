# 双指针

## [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/description/?envType=study-plan-v2&envId=top-100-liked)


!!! note "两边向中间夹逼"
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->
给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

说明：你不能倾斜容器。


> 示例1:
> 输入：[1,8,6,2,5,4,8,3,7]
> 
> 输出：49
> 
> 解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。 
> 


```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        max_area = 0
        l , r = 0, len(height) - 1
        while l < r:
            max_area = max(max_area , min(height[l], height[r]) * (r - l))
            if height[l] <= height[r]:
                l += 1
            else:
                r -= 1
        return max_area

```

!!! quote "为什么可以两边向中间夹，每次只需要移动更小的指针：因为随着指针移动，能用来装水的范围只会越来越小，所以，就算移动最大的指针，找到的结果不会比现在的这个解更大的。"

-----

## [15. 三数之和](https://leetcode.cn/problems/3sum/description/?envType=study-plan-v2&envId=top-100-liked)


<!-- 所有文件名必须是该题目的英文名 -->

!!! note "双指针"
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->
给你一个整数数组 nums ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j、i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请你返回所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。

 

> 示例1:
> 示例 1：
> 输入：`nums = [-1,0,1,2,-1,-4]`
> 
> 输出：`[[-1,-1,2],[-1,0,1]]`
> 
> 解释：
> `nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 `
> 
> `nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0` 
> 
> `nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0` 。
> 
> 不同的三元组是 `[-1,0,1]` 和 `[-1,-1,2]` 。
> 
> 注意，输出的顺序和三元组的顺序并不重要。
> 


```python hl_lines="6 7 16 17"
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        result = []
        nums = sorted(nums)
        for i in range(len(nums) - 2):
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            l , r = i + 1, len(nums) - 1
            while l < r:
                if nums[i] + nums[l] + nums[r] > 0:
                    r -= 1
                elif nums[i] + nums[l] + nums[r] < 0:
                    l += 1
                else:
                    result.append([nums[i] , nums[l],  nums[r]])
                    l += 1
                    r -= 1
                    while l < r and nums[l] == nums[l - 1]:
                        l += 1
                    while r > l and nums[r] == nums[r + 1]:
                        r -= 1
                    
        return result
```

!!! quote "思路要清楚：先排序，然后双指针。双指针写的时候有个小技巧。就是用一个for循环充当第一个指针，然后在遍历过程中用另外的变量记录另一个（或另外几个）变量的位置。这个问题因为不允许重复，所以必须要对相同的元素进行消重。消重的时候为了避免不知道怎么处理，可以每次都把指针自增/自减1，然后判断加了之后是否会和加了之前的重复。见6～7行，见16～17行。也就是，消重的时候尽量按照 `i != i - 1` 的思路来写。"
