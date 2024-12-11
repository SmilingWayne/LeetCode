# 二分搜索

## [153. 寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "二分搜索，但是每次不是比较mid和target，而是比较mid和最小值，所以需要记录每次搜索时的最小值。"
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

已知一个长度为 `n` 的数组，预先按照升序排列，经由 `1` 到 `n` 次 旋转 后，得到输入数组。例如，原数组 `nums = [0,1,2,4,5,6,7]` 在变化后可能得到：
若旋转 `4` 次，则可以得到 `[4,5,6,7,0,1,2]`
若旋转 `7` 次，则可以得到 `[0,1,2,4,5,6,7]`
注意，数组 `[a[0], a[1], a[2], ..., a[n-1]]` 旋转一次 的结果为数组 `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`。

给你一个元素值 互不相同 的数组 `nums` ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 最小元素 。

你必须设计一个时间复杂度为` O(log n) `的算法解决此问题。


> 输入：nums = `[3,4,5,1,2]`
> 
> 输出：1
> 
> 解释：原数组为 `[1,2,3,4,5]` ，旋转 3 次得到输入数组。
> 


```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        min_ = nums[0]
        l = 0
        r = len(nums) - 1
        while l <= r:
            mid = l + (r - l) // 2
            if nums[mid] >= min_:
                l = mid + 1
            else:
                min_ = nums[mid]
                r = mid - 1
        return min_
```

!!! quote ""
