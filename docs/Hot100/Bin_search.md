---
tags:
  - 二分搜索
---

# 二分搜索

## [153. 寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
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

!!! quote "二分搜索，但是每次不是比较mid和target，而是比较mid和最小值，所以需要记录每次搜索时的最小值。"

----

## [74. 搜索二维矩阵](https://leetcode.cn/problems/search-a-2d-matrix/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

> 给你一个满足下述两条属性的 m x n 整数矩阵：
> 
> 每行中的整数从左到右按非严格递增顺序排列。每行的第一个整数大于前一行的最后一个整数。给你一个整数 target ，如果 target 在矩阵中，返回 true ；否则，返回 false 。

示例 1：

输入：`matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]]`, `target = 3`
输出：`true`



```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        
        i ,j = 0, 0
        m , n = len(matrix), len(matrix[0])
        if matrix[0][0] > target or matrix[m - 1][-1] < target:
            return False
        l = 0
        r = m - 1
        while l <= r:
            mid = l + (r - l) // 2
            if matrix[mid][-1] == target:
                return True
            elif matrix[mid][-1] > target:
                r = mid - 1
            else:
                l = mid + 1
        i = l
        l = 0
        r = n - 1
        while l <= r:
            mid = l + (r - l) // 2
            if matrix[i][mid] == target:
                return True
            elif matrix[i][mid] > target:
                r = mid - 1
            else:
                l = mid + 1
        return False

```

!!! quote "思路：两次二分搜索！一次针对行、一次针对列"


----

## [33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/description/)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

> 整数数组 nums 按升序排列，数组中的值 互不相同 。
> 
> 在传递给函数之前，`nums` 在预先未知的某个下标 `k`（0 <= k < nums.length）上进行了 旋转，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 从 0 开始 计数）。例如， `[0,1,2,4,5,6,7]` 在下标 3 处经旋转后可能变为 `[4,5,6,7,0,1,2]` 。

> 给你 旋转后 的数组 `nums` 和一个整数 `target` ，如果 `nums` 中存在这个目标值 `target` ，则返回它的下标，否则返回 -1 。
> 
> 你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题。

 

> 示例 1：
> 
> 输入：`nums = [4,5,6,7,0,1,2], target = 0`
> 输出：`4`


```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l = 0; r = len(nums) - 1
        while l <= r:
            mid = l + (r - l) // 2
            if nums[mid] == target:
                return mid 
            if nums[mid] >= nums[l]:
                if nums[l] <= target < nums[mid]:
                    r = mid - 1
                else:
                    l = mid + 1
            else:
                if nums[mid] < target <= nums[r]:
                    l = mid + 1
                else:
                    r = mid - 1
        return -1

```

!!! quote "牢记：二分搜索一定只能在有序的部分进行查找。而问题这么旋转了数组后，mid在的部分一定属于一个有序的部分。我们只需要判断target是否在这个有序的部分里，如果在，就缩小区间到这个有序的部分里，如果不在，就缩小区间到另一个部分。"

    ![](https://assets.leetcode-cn.com/solution-static/33/33_fig1.png)

    比如，有两种划分方法：第一种是mid属于前半部分，那么，只需要判断target是否在前半部分，也就是在left, mid之间：如果在，就缩小区间到前半部分，如果不在，就缩小区间到后半部分。第二种是mid属于后半部分，那么，只需要判断target是否在后半部分，也就是 mid, right 之间。如果在，就缩小区间到后半部分，如果不在，就缩小区间到前半部分。

----------

## [34. 在排序数组内查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/?envType=study-plan-v2&envId=top-interview-150)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 给你一个按照非递减顺序排列的整数数组 `nums`，和一个目标值 `target`。请你找出给定目标值在数组中的开始位置和结束位置。
> 
> 如果数组中不存在目标值 `target` ，返回 `[-1, -1]`。
> 
> 你必须设计并实现时间复杂度为 `O(log n)` 的算法解决此问题。

> 示例 1：
> 
> 输入：`nums = [5,7,7,8,8,10], target = 8`
> 
> 输出：`[3,4]`
> 


```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        result = [-1, -1]
        n = len(nums)
        l = 0; r = n - 1
        while l <= r:
            mid = l + ( r - l) // 2
            if nums[mid] > target:
                r = mid - 1
            elif nums[mid] < target:
                l = mid + 1
            else:
                result[0] = mid
                r = mid - 1
        
        l = 0; r = n - 1
        while l <= r:
            mid = l + ( r - l) // 2
            if nums[mid] > target:
                r = mid - 1
            elif nums[mid] < target:
                l = mid + 1
            else:
                result[1] = mid
                l = mid + 1
        return result


```

!!! quote "一开始题解没看懂，实际没那么复杂...要想找到最前面的那个，无非就是正常二分搜索过程中找到了一个目标值了，但是可能前面还有。此时我们保存下当前的位置，但是调整右边的pivot，依然继续搜索，直到两个指针逼近；要想找到最后面的那个，无非是正常二分搜索过程中找到目标值了，但是可能后面还有，此时我们保存下当前的位置，但是调整左边的poivot，依然继续搜索，直到两个指针逼近"
