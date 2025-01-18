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
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

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
