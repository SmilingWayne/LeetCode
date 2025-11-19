---
tags:
  - 双指针
  - 滑动窗口
  - 前缀和
---

# 双指针 / 滑窗 / 单调栈

## [56. 合并区间](https://leetcode.cn/problems/merge-intervals/description/?envType=study-plan-v2&envId=top-interview-150)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

> 以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]` 。请你合并所有重叠的区间，并返回 一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间 。

> 示例 1：
> 
> 输入：`intervals = [[1,3],[2,6],[8,10],[15,18]]`
> 
> 输出：`[[1,6],[8,10],[15,18]]`
> 
> 解释：区间 `[1,3]` 和 `[2,6]` 重叠, 将它们合并为 `[1,6].`
> 

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key = lambda x: x[0])
        l = intervals[0][0]
        r = intervals[0][1]
        result = []
        n = len(intervals)
        for idx, (a,b) in enumerate(intervals):
            if a > r:
                result.append([l, r])
                l = a
                r = b
            else:
                r = max(r, b)
            
            if idx == n - 1:
                result.append([l, r])
        return result

```

!!! quote "先排序，后合并，始终维护“当前的最左边”和“当前的最右边”。注意右侧项变化的逻辑：`max(r, b)`"

----


!!! quote "以下为滑动窗口部分的题目"




## [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/description/?envType=study-plan-v2&envId=top-interview-150)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

> 给定一个含有 `n 个正整数的数组和一个正整数 `target` 。
> 
> 找出该数组中满足其总和大于等于 `target` 的长度最小的 子数组`[numsl, numsl+1, ..., numsr-1, numsr]` ，并返回其长度。如果不存在符合条件的子数组，返回 `0` 。

 

示例 1：

> 输入：`target = 7, nums = [2,3,1,2,4,3]`
> 
> 输出：`2`
> 
> 解释：子数组 `[4,3]` 是该条件下的长度最小的子数组。
> 


```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        l = 0
        r = 0
        sum_inside = 0
        res = len(nums)
        for i in range(len(nums)):
            sum_inside += nums[i]
            if sum_inside < target:
                if i == len(nums) - 1:
                    return 0
                continue
            while l < i and sum_inside - nums[l] >= target:
                sum_inside -= nums[l]
                l += 1
            res = min(res, i - l + 1)
            

        return res

```

!!! quote "滑动窗口：始终滑动那个能够满足“ >= target ”的窗口! for循环记录最右侧的位置。"


## [🌟239. 滑动窗口最大值](https://leetcode.cn/problems/subarray-sum-equals-k/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:crisma; font-weight:bold">High 困难</span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回 滑动窗口中的最大值 。

 

示例 1：

输入：`nums = [1,3,-1,-3,5,3,6,7], k = 3`

输出：`[3,3,5,5,6,7]`

解释：

滑动窗口的位置                最大值
---------------               -----
>[1  3  -1] -3  5  3  6  7    ->   3
> 1 [3  -1  -3] 5  3  6  7     ->  3
> 1  3 [-1  -3  5] 3  6  7     ->  5
> 1  3  -1 [-3  5  3] 6  7     ->  5
> 1  3  -1  -3 [5  3  6] 7     ->  6
> 1  3  -1  -3  5 [3  6  7]    ->  7
示例 2：

输入：nums = [1], k = 1
输出：[1]

```python hl_lines="6 7 9 10"
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        queue = deque()
        result = []
        for idx, num in enumerate(nums):
            if queue and idx - k == queue[0]:
                queue.popleft()

            while queue and num > nums[queue[-1]]:
                queue.pop()
            queue.append(idx)
            if idx >= k - 1: 
                result.append(nums[queue[0]])
        return result
```

!!! quote "用一个优先队列进行处理：每当我们向右移动窗口时，我们就可以把一个新的元素放入优先队列中，并保证最上层的元素是所有元素的最大值。如果这个值足够大，那么前面那些小的值就永远不可能出现在滑动窗口中了，我们可以将其永久地从优先队列中移除。以此类推，如果一个新加入的比前面的某些大，那么可以直接取代这些前面的。同样，如果这个元素足够老，那么它会被剔除。"



# 前缀和

## [🌟560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/description/?envType=study-plan-v2&envId=top-100-liked) 

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
> 


```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        n = len(nums)
        prevSum = [0 for _ in range(n + 1)]
        for i in range(n):
            prevSum[i + 1] = prevSum[i] + nums[i]
        record = defaultdict(int)
        result = 0
        for i in range(n + 1):
            result += record[prevSum[i] - k]
            record[prevSum[i]] += 1
        return result
```

!!! quote ""
