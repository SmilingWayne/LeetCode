# 数组

## [轮转数组](https://leetcode.cn/problems/rotate-array/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->
<!-- 题目简介 -->

> 示例1:
> 给定一个整数数组 nums，将数组中的元素向右轮转 k 个位置，其中 k 是非负数
> 
> 

> 输入: `nums = [1,2,3,4,5,6,7], k = 3`
> 
> 输出: `[5,6,7,1,2,3,4]`
> 
> 解释:
> 
> 向右轮转 1 步: `[7,1,2,3,4,5,6]`
> 
> 向右轮转 2 步: `[6,7,1,2,3,4,5]`
> 
> 向右轮转 3 步: `[5,6,7,1,2,3,4]`
> 


```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        n = len(nums)
        k = k % n 
        l = 0
        r = len(nums) - 1
        while l < r:
            nums[l], nums[r] = nums[r], nums[l]
            l += 1
            r -= 1
        l = 0
        r = k - 1
        while l < r:
            nums[l], nums[r] = nums[r], nums[l]
            l += 1
            r -= 1
        l = k
        r = len(nums) - 1
        while l < r:
            nums[l], nums[r] = nums[r], nums[l]
            l += 1
            r -= 1

# ----->--->, reverse k: ----->--->
# reverse: <---<-----
# reverse first k: ---><-----
# reverse last ones: --->----->
```

!!! quote ""
