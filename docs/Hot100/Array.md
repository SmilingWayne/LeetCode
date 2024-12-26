# 数组与矩阵

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

----

## [41. 缺失的第一个正数](https://leetcode.cn/problems/first-missing-positive/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:crisma; font-weight:bold">High 困难</span>

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
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

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
        m = len(matrix)

        if m == 0:
            return []
        n = len(matrix[0])

        if m == 1 and n == 1:
            return [matrix[0][0]]
        elif m == 1:
            return [matrix[0][i] for i in range(n)]
        elif n == 1:
            return [matrix[i][0] for i in range(m)]
        l = 0
        r = n
        lb = 0
        ub = m
        result = []
        for k in range(1):
            for i in range(l, r - 1):
                result.append(matrix[lb][i])
            
            for i in range(lb, ub - 1):
                result.append(matrix[i][r - 1])
            
            for i in range(r - 1, l, - 1):
                result.append(matrix[ub - 1][i])

            for i in range(ub - 1, lb, - 1):
                result.append(matrix[i][lb])
            lb += 1
            ub -= 1
            l += 1
            r -= 1
        if len(result) == m * n:
            return result
        else:
            newmat = [[0 for _ in range(n - 2)] for _ in range( m - 2)]
            for i in range(1,  m - 1):
                for j in range(1, n - 1):
                    newmat[i - 1][j - 1] = matrix[i][j]

            return result + self.spiralOrder(newmat)
        
```

!!! quote "递归做法，注意，如果一次遍历就能完成，则不需要再次调用了。这里的一次遍历有两种情况：$1 \times n \lor n \times 1$,或者`2`行、或者 `2` 列"

