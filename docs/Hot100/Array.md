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

---

## [238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/?envType=study-plan-v2&envId=top-100-liked)


<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 给你一个整数数组 `nums`，返回 数组 `answer` ，其中 `answer[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积 。
> 
> 题目数据 保证 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在  32 位 整数范围内。
> 
> 请 不要使用除法，且在 `O(n)` 时间复杂度内完成此题。

> 示例 1:
> 
> 输入: `nums = [1,2,3,4]`
> 
> 输出: `[24,12,8,6]` 
> 
> 输入: `nums = [-1,1,0,-3,3]`
> 
> 输出: `[0,0,9,0,0]`
> 


```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        result = [1 for _ in range(n)]
        pre = [1 for _ in range(n)]
        post = [1 for _ in range(n)]
        tmp = 1
        for i in range(1, len(nums)):
            pre[i] = tmp * nums[i - 1]
            tmp = pre[i]
        tmp = 1
        for i in range(n - 2, -1, - 1):
            post[i] = tmp * nums[i + 1]
            tmp = post[i]
        for i in range(n):
            result[i] = pre[i] * post[i]
        return result
```

!!! quote ""
