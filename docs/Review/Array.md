# 数组与矩阵

## [🌟52. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

> 子数组:是数组中的一个连续部分。

 

示例 1：

> 输入：`nums = [-2,1,-3,4,-1,2,1,-5,4]`
> 
> 输出：`6`
> 
> 解释：连续子数组 `[4,-1,2,1]` 的和最大，为 `6` 。



```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        premax = 0
        res = nums[0]
        for i in range(len(nums)):
            premax = max(premax + nums[i], nums[i])
            res = max(premax, res)
        return res

```

!!! quote "前缀和，每个元素要么接在前缀上，要么独立开来单独算一个子数组。所以不需要辅助list，只需要边走边比较就行了。"



## [189. 轮转数组](https://leetcode.cn/problems/rotate-array/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

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

!!! quote "有凑巧的做法：3次首尾颠倒即可。"

---

## [238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/?envType=study-plan-v2&envId=top-100-liked)


<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

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

```python
# Better!
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        left = [1 for _ in range(n)]
        for i in range(1, n):
            left[i] = left[i - 1] * nums[i - 1]
        result = []
        R = 1
        for i in range(n - 1, -1, -1):
            result.append(R * left[i])
            R *= nums[i]
        return result[::-1]
```

!!! quote "一种方法是维护两个列表，分别记录前缀和后缀乘积，第二种方法是，只用一个列表，然后保存的时候从后向前，一边走一边记录后缀。"

----

## [41. 缺失的第一个正数](https://leetcode.cn/problems/first-missing-positive/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:crisma; font-weight:bold">High 困难</span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 给你一个未排序的整数数组 nums ，请你找出其中没有出现的最小的正整数。

> 请你实现时间复杂度为 O(n) 并且只使用常数级别额外空间的解决方案。
 

> 示例 1：
> 
> 输入：`nums = [1,2,0]`
> 输出：3
> 解释：范围 `[1,2]` 中的数字都在数组中。

> 


```python
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        n = len(nums)
        for i in range(n):
            while 1 <= nums[i] <= n and nums[nums[i] - 1] != nums[i]:
                nums[nums[i] - 1], nums[i] =  nums[i] , nums[nums[i] - 1]
        for i in range(n):
            if nums[i] != i + 1:
                return i + 1
        return n + 1
```

!!! quote "思路很重要：（1）如果数组长度是N，那么目标最大是 N+1，也就说1～N全部出现了；所以，我们可以每次交换一个在这个区间内的数字到它该有的位置上。到上面的方法可能会陷入死循环。如果 `nums[i]` 恰好与 `nums[x−1]` 相等，那么就会无限交换下去。此时我们有 `nums[i]=x=nums[x−1]`，说明 `x` 已经出现在了正确的位置。因此我们可以跳出循环，开始遍历下一个数。"


----


## [54. 螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 给你一个 `m`行 `n` 列的矩阵 `matrix` ，请按照 顺时针螺旋顺序 ，返回矩阵中的所有元素。
>
> 输入：`matrix = [[1,2,3],[4,5,6],[7,8,9]]`
> 
> 输出：`[1,2,3,6,9,8,7,4,5]`


```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        
        if len(matrix) == 0 or len(matrix[0]) == 0:
            return []
        if len(matrix) == 1:
            return matrix[0]
        if len(matrix[0]) == 1:
            return [matrix[i][0] for i in range(len(matrix))]
        

        lbx = 0; ubx = len(matrix) - 1
        lby = 0; uby = len(matrix[0]) - 1
        result = []
        for i in range(lby, uby):
            result.append(matrix[0][i])
        for i in range(lbx, ubx):
            result.append(matrix[i][uby])
        for i in range(uby, 0, -1):
            result.append(matrix[ubx][i])
        for i in range(ubx, 0, -1):
            result.append(matrix[i][0])
        
        newMat = []
        for i in range(1, ubx):
            tmp = []
            for j in range(1, uby):
                tmp.append(matrix[i][j])
            newMat.append(tmp)
        return result + self.spiralOrder(newMat)
        
```

!!! quote "递归做法，注意，如果一次遍历就能完成，则不需要再次调用了。这里的一次遍历有两种情况，首先，如果某一列完全没元素，或者矩阵本身为空，则不必跑了，如果只有一列或者只有一行，那么分别处理，其他时候递归即可。"

-----

## [240. 搜索二维数组](https://leetcode.cn/problems/search-a-2d-matrix-ii/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 编写一个高效的算法来搜索 `m x n` 矩阵 `matrix` 中的一个目标值 `target` 。该矩阵具有以下特性：
> 
> 每行的元素从左到右升序排列。
> 
> 每列的元素从上到下升序排列。

> 


```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m = len(matrix)
        n = len(matrix[0])
        i = 0
        j = n - 1
        while i < m and j >= 0:
            if matrix[i][j] == target:
                return True
            elif matrix[i][j] > target:
                j -= 1
            else:
                i += 1
        return False
```

!!! quote "经典的“Z” 字形状搜索。从右上向左下搜索。如果比第一行的某列元素还要小，那么只可能在它左侧的列中。如果比这一列某行的值还小，那么继续向下一行走，直到走到头。"


-----


!!! warning "以下是面试经典150题的相关内容"

## [88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/description/?envType=study-plan-v2&envId=top-interview-150)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:Green; font-weight:bold">Easy 简单</span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给你两个按 非递减顺序 排列的整数数组 `nums1` 和 `nums2`，另有两个整数`m` 和 `n` ，分别表示 `nums1` 和 `nums2` 中的元素数目。

请你 合并 `nums2` 到 `nums1` 中，使合并后的数组同样按 非递减顺序 排列。

注意：最终，合并后数组不应由函数返回，而是存储在数组 `nums1` 中。为了应对这种情况，`nums1` 的初始长度为 `m + n`，其中前 `m` 个元素表示应合并的元素，后 `n` 个元素为 0 ，应忽略。`nums2` 的长度为 `n` 。


```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        i = m - 1
        j = n - 1
        tmp = m + n - 1
        while i >= 0 and j >= 0:
            if nums1[i] >= nums2[j]:
                nums1[tmp] = nums1[i]
                tmp -= 1
                i -= 1
            else:
                nums1[tmp] = nums2[j]
                tmp -= 1
                j -= 1
        while j >= 0:
            nums1[tmp] = nums2[j]
            j -= 1
            tmp -= 1

```

!!! quote "和最简单的那个题目是类似的，但是要从后往前iterate，这样可以避免覆盖掉nums1的值。注意有可能nums2更加小，while循环完了之后要补充检查一下指针的位置，是否存在比nums1还要小的。"


---

## [31. 下一个排列](https://leetcode.cn/problems/next-permutation/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度： <span style = "color:gold; font-weight:bold">Medium 中等 </span> 

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

整数数组的一个 排列  就是将其所有成员以序列或线性顺序排列。

例如，`arr = [1,2,3]` ，以下这些都可以视作 `arr` 的排列：`[1,2,3]、[1,3,2]、[3,1,2]、[2,3,1]` 。
整数数组的 下一个排列 是指其整数的下一个字典序更大的排列。更正式地，如果数组的所有排列根据其字典顺序从小到大排列在一个容器中，那么数组的 下一个排列 就是在这个有序容器中排在它后面的那个排列。如果不存在下一个更大的排列，那么这个数组必须重排为字典序最小的排列（即，其元素按升序排列）。

例如，`arr = [1,2,3]` 的下一个排列是 `[1,3,2]` 。
类似地，`arr = [2,3,1]` 的下一个排列是 `[3,1,2]` 。
而 `arr = [3,2,1]` 的下一个排列是 `[1,2,3]` ，因为 `[3,2,1]` 不存在一个字典序更大的排列。
给你一个整数数组 `nums` ，找出 `nums` 的下一个排列。

必须 原地 修改，只允许使用额外常数空间。



```python

class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        n = len(nums) 
        i = n - 2
        while i >= 0:
            if nums[i] >= nums[i + 1]:
                i -= 1
            else:
                break
        j = - 1
        if i >= 0:
            j = n - 1
            while j >= 0 and nums[i] >= nums[j]:
                j -= 1
            nums[i], nums[j] = nums[j], nums[i]
        
        l = i + 1; r = n - 1
        while l <= r:
            nums[l], nums[r] = nums[r], nums[l]
            l += 1
            r -= 1

```

!!! quote ""
    一个技巧题。如果想要找到一个“增加得尽可能小的排列，那么首先需要把一个大的数字换到前面，并且这个数字越靠后越好”（因为越靠前增加得越大），所以从后向前查找，看相邻两个元素 $a[i], a[i + 1]$ 是否是递增的，如果是递增的说明，我们至少可以在 $[i + 1, n)$ 这个部分找到一个更大的数字挪到前面；

    1. 但是我们未必一定挪相邻的这两个：比如 1,2,5,7,6,3，上述方法找发现5，7是升序的，所以：1,2,7,5,6,3，这个数字确实更大了，但是增加太快了，1,2,6,7,5,3 也是增加的，而且增加得幅度更小！此时你意识到，你可以通过找 $[i + 1, n)$ 区间内从后向前第一个比 $a[i]$ 大的元素交换一下，就可以了！
    2. 这样也不够，万一你找到的后向前第一个比 $a[i]$ 大的元素，前面还有更大的元素怎么处理？你只需要把 $[i + 1, n)$ 的元素重新反转一下就行了。