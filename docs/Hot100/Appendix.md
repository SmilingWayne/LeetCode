# 一些难点和重点

## [215. 数组中第K大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "信息量和细节很多的一个题"
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 给定整数数组 nums 和整数 k，请返回数组中第 k 个最大的元素。
> 
> 请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。
> 
> 你必须设计并实现时间复杂度为 O(n) 的算法解决此问题。

 

> 示例 1:
> 
> 输入: `[3,2,1,5,6,4], k = 2`
> 输出: `5`

```python hl_lines="7 12"
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        def partition(arr, low, high):
            pivot = arr[low]
            left, right = low, high
            while left < right:
                while left < right and pivot < arr[right]:
                    right -= 1
                if left < right:
                    arr[left] = arr[right]
                    left += 1
                while left < right and pivot > arr[left]:
                    left += 1
                if left < right:
                    arr[right] = arr[left]
                    right -= 1
            arr[left] = pivot
            return left
        
        def randomPartition(arr, low, high):
            pivot_idx = random.randint(low, high)
            arr[low], arr[pivot_idx] = arr[pivot_idx], arr[low]
            return partition(arr, low, high)

        def topK(arr, low, high, k):
            mid = randomPartition(arr, low, high)
            if mid == k - 1:
                return arr[mid]
            elif mid < k - 1:
                return topK(arr, mid + 1, high, k)
            else:
                return topK(arr, low, mid - 1, k)
        
        n = len(nums)
        return topK(nums, 0, n - 1, n - k  + 1)
            
        
```

!!! quote "这个很容易考到。而且是快排中的经典，困难中的困难。"
    1. 每次快排用到`partition`，实际上是锁定了一个pivot所在的最终位置。这个位置能保证，我前面的数字一定都不会比他大，后面的数字都不比它小。好了，**如果锁定的最终位置还不够远（没到第K大），那么我们就需要在这个锁定位置的后面的元素中继续去寻找，前面的（即使是乱序的）也不管了，因为肯定找不到。反之，如果这个位置太远了，我们就需要到前面找，后面的（即使是乱序的）也不管了，因为肯定找不到。**
    
    > 强迫自己复习快排的写法。

    2. 第二个trick是不能每次都以头为pivot。意外情况是完全倒序的列表，会退化。需要加入随机性。

    3. 第三个trick是重复元素的问题。这个就很精巧了，我们必须跳过那些重复的元素，所以必须要所有与这个pivot相等的，都放到前面去。见7/12行。


