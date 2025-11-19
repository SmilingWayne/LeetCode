# 勇闯秋招手撕系列 II：双指针

!!! abstract ""
    <span style="color:green;font-weight:bold">移动零</span>

    <span style="color:green;font-weight:bold">验证回文串 (150) </span>

    <span style="color:green;font-weight:bold">判断子序列 (150)</span>

    <span style="color:orange;font-weight:bold">盛最多水的容器</span>

    <span style="color:orange;font-weight:bold">🌟 三数之和</span>

    <span style="color:orange;font-weight:bold">🌟 最接近的三数之和</span>

    <span style="color:red;font-weight:bold">接雨水</span> 

    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>


## [283. 移动零](https://leetcode.cn/problems/move-zeroes/description/?envType=study-plan-v2&envId=top-100-liked)

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

请注意 ，必须在不复制数组的情况下原地对数组进行操作。

 

示例 1:

输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        l = 0; r = 0
        n = len(nums)
        while r < n: 
            if nums[r] != 0:
                nums[l], nums[r] = nums[r], nums[l]
                l += 1
            r += 1
```

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>


## [125. 验证回文串](https://leetcode.cn/problems/valid-palindrome/?envType=study-plan-v2&envId=top-interview-150)

如果在将所有大写字符转换为小写字符、并移除所有非字母数字字符之后，短语正着读和反着读都一样。则可以认为该短语是一个 回文串 。

字母和数字都属于字母数字字符。

给你一个字符串 s，如果它是 回文串 ，返回 true ；否则，返回 false 。

> 输入: s = "A man, a plan, a canal: Panama"
> 输出：true
> 解释："amanaplanacanalpanama" 是回文串。

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        n = len(s)
        s = s.lower()
        l = 0; r = n - 1
        while l <= r:
            while l < r and not s[l].isdigit() and not s[l].isalpha():
                l += 1
            while l < r and not s[r].isdigit() and not s[r].isalpha():
                r -= 1
            if s[l] != s[r]:
                return False
            l += 1
            r -= 1
        return True
```

<br>
<br>
<br>
<br>

## [167. 两数之和 II：输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/?envType=study-plan-v2&envId=top-interview-150)

给你一个下标从 1 开始的整数数组 numbers ，该数组已按 非递减顺序排列  ，请你从数组中找出满足相加之和等于目标数 target 的两个数。如果设这两个数分别是 numbers[index1] 和 numbers[index2] ，则 1 <= index1 < index2 <= numbers.length 。

以长度为 2 的整数数组 [index1, index2] 的形式返回这两个整数的下标 index1 和 index2。你可以假设每个输入 只对应唯一的答案 ，而且你 不可以 重复使用相同的元素。你所设计的解决方案必须只使用常量级的额外空间。

> 输入：numbers = [2,7,11,15], target = 9
> 输出：[1,2]
> 解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。返回 [1, 2].

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        result = []
        n = len(numbers)
        l = 0; r = n - 1
        while l < r:
            while l > 0 and l < r and numbers[l] == numbers[l - 1]:
                l += 1
            while r < n - 1 and l < r and numbers[r] == numbers[r + 1]:
                r -= 1
            
            if l < r:
                if numbers[l] + numbers[r] == target:
                    return [l + 1, r + 1]
                elif numbers[l] + numbers[r] < target:
                    l += 1
                else:
                    r -= 1
        return [-1, -1]
```

## [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/description/?envType=study-plan-v2&envId=top-100-liked)


难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>；给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。你不能倾斜容器。


> 示例1:
> 输入：[1,8,6,2,5,4,8,3,7]
> 
> 输出：49
> 
> 解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。 


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

!!! quote "==为什么可以两边向中间夹，每次只需要移动更小的指针== ：因为随着指针移动，能用来装水的范围只会越来越小，所以，就算移动最大的指针，找到的结果不会比现在的这个解更大的。"

## [15. 三数之和](https://leetcode.cn/problems/3sum/description/?envType=study-plan-v2&envId=top-100-liked)


<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->
<span style = "color:gold; font-weight:bold">Medium 中等 </span>；给你一个整数数组 nums ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j、i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请你返回所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。

> 输入：`nums = [-1,0,1,2,-1,-4]`
> 
> 输出：`[[-1,-1,2],[-1,0,1]]`


```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        n = len(nums)  
        res = []
        for first in range(n - 2):
            if first > 0 and nums[first] == nums[first - 1]:
                continue 
            if nums[first] > 0:
                break 
            second, third = first + 1, n - 1
            while second < third:
                curr = nums[first] + nums[second] + nums[third]
                if curr == 0:
                    res.append([nums[first], nums[second], nums[third]])
                    while second < third and nums[second] == nums[second + 1]:
                        second += 1
                    if second < third:
                        second += 1
                    while second < third and nums[third] == nums[third - 1]:
                        third -= 1
                    if second < third:
                        third -= 1
                elif curr > 0:
                    while second < third and nums[third] == nums[third - 1]:
                        third -= 1
                    if second < third:
                        third -= 1
                else:
                    while second < third and nums[second] == nums[second + 1]:
                        second += 1
                    if second < third:
                        second += 1
        return res
```

!!! quote "思路要清楚：先排序，然后双指针。双指针写的时候有个小技巧。就是用一个for循环充当第一个指针，然后在遍历过程中用另外的变量记录另一个（或另外几个）变量的位置。这个问题因为不允许重复，所以必须要对相同的元素进行消重。消重的时候为了避免不知道怎么处理，可以每次都把指针自增/自减1，然后判断加了之后是否会和加了之前的重复。见6～7行，见16～17行。也就是，消重的时候尽量按照 `i != i - 1` 的思路来写。"


## [16. 最接近的三数之和](https://leetcode.cn/problems/3sum-closest/description/)

给你一个长度为 n 的整数数组 nums 和 一个目标值 target。请你从 nums 中选出三个整数，使其和与 target 最接近。返回这个和。假定每组输入只存在恰好一个解。
输入：nums = [-1,2,1,-4], target = 1；输出：2；

```python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        gap = float('inf')
        res = float('-inf')
        n = len(nums)
        nums.sort()
        for first in range(n - 2):
            if first > 0 and nums[first] == nums[first - 1]:
                continue
            second, third = first + 1, n - 1
            while second < third:
                cur_res = nums[first] + nums[second] + nums[third]
                if cur_res == target:
                    return target 
                cur_gap = abs(cur_res - target)
                if cur_gap < gap:
                    gap = cur_gap
                    res = cur_res 
                if cur_res < target:
                    while second < third and nums[second] == nums[second + 1]:
                        second += 1
                    if second < third:
                        second += 1
                else:
                    while second < third and nums[third] == nums[third - 1]:
                        third -= 1
                    if second < third:
                        third -= 1
        return res
                    
```


## [42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/description/?envType=study-plan-v2&envId=top-100-liked)


> <span style = "color:red; font-weight:bold">High 困难</span>；给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。
> 
> 输入：`height = [0,1,0,2,1,0,1,3,2,1,2,1]`
> 
> 输出：`6`；解释：上面是由数组 `[0,1,0,2,1,0,1,3,2,1,2,1]` 表示的高度图，在这种情况下，可以接 `6` 个单位的雨水（蓝色部分表示雨水）。 
> 
> ![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)


```python
class Solution:
    def trap(self, height: List[int]) -> int:
        stack = []
        res = 0
        for i, h in enumerate(height):
            while stack and h >= height[stack[-1]]:
                j = stack.pop()
                if stack:
                    left = stack[-1]
                    curWidth = i - left - 1
                    curHeight = min(height[left], h) - height[j]
                    res += curWidth * curHeight
            stack.append(i)
        return res

```
