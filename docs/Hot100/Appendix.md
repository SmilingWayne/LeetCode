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


## 一份关于快速排序的手把手教学

我们对这样一个数组进行快速排序：`[5,7,4,9,2,5,8,0,4]`。我们的核心思想是：

1. `left` 指针只会向右移动；`right` 指针只会向左移动；
2. **保证 `left` 指针左边的一定不比 `pivot` 大，而 `right` 指针右边的一定不比 `pivot` 小。**

**第一次迭代：**

我们选择 `pivot` 为队首元素 `5`。保存为 `pivot` 变量。

我们声明的左右指针 `left, right`，分别指向 `5, 4`，处于队首和队尾。

接下来，我们首先移动右边的指针。我们希望右指针一直移动到一个“比pivot还要小的元素为止”。这里发现`right` 指向的 `4` 正好比 `pivot` 小，说明位置太靠后了，要往前挪，挪到哪？就是 `left` 指针。我们把这个 `4` 放到 `left` 指针的位置，也就变成了：

`[4,7,4,9,2,5,8,0,4]`。`left` 对应 idx = 0, `right` 对应 idx = 8.

既然这时候 `left` 指针已经被替换成一个比 `pivot` 更小的元素，既然我们要保证“核心思想”成立，我们当然应该把 `left` 继续向右移动。所以 `left += 1`，也就是：

`[4,7,4,9,2,5,8,0,4]`。`left` 对应 idx = 1, `right` 对应 idx = 8.

现在，我们移动左边的指针。我们希望左指针一直移动到一个 “比pivot还要大的元素为止”。诶，正好`left` 指针现在指向的元素是 `7`，所以 `left` 指针也不需要动。这个元素比 `pivot` 大，说明位置太靠前了，要往后挪，挪到哪？就是 `right` 指针。我们把这个 `7` 放到 `right` 指针的位置，也就变成了：

`[4,7,4,9,2,5,8,0,7]`。`left` 对应 idx = 1, `right` 对应 idx = 8.

既然这时候 `right` 指针已经被替换成一个比 `pivot` 更大的元素，既然我们要保证“核心思想”成立，我们当然应该把 `right` 继续向左移动。所以 `right -= 1`，也就是：

`[4,7,4,9,2,5,8,0,7]`。`left` 对应 idx = 1, `right` 对应 idx = 7.

----

**第二次迭代：**

我们按照第一次迭代的顺序，首先移动 `right`。我们希望右指针一直移动到一个“比pivot还要小的元素为止”。这里 `right` 指向的 `0` 正好比 `pivot` 小，说明位置太靠后了，要往前挪，挪到哪？就是 `left` 指针。我们把这个 `0` 放到 `left` 指针的位置，也就变成了：

`[4,0,4,9,2,5,8,0,7]`。`left` 对应 idx = 1, `right` 对应 idx = 7.

既然这时候 `left` 指针已经被替换成一个比 `pivot` 更小的元素，既然我们要保证“核心思想”成立，我们当然应该把 `left` 继续向右移动。所以 `left += 1`，也就是：

`[4,0,4,9,2,5,8,0,7]`。`left` 对应 idx = 2, `right` 对应 idx = 7.

现在，我们移动左边的指针。我们希望左指针一直移动到一个 “比pivot还要大的元素为止”。首先，`0` 比pivot小，继续向右(left++)，`4` 比 pivot 小，继续向右(left++)。现在 `left` 指针现在指向的元素是 `9`。这个元素比 `pivot` 大，说明位置太靠前了，要往后挪，挪到哪？就是 `right` 指针。我们把这个 `9` 放到 `right` 指针的位置，也就变成了：

`[4,0,4,9,2,5,8,9,7]`。`left` 对应 idx = 3, `right` 对应 idx = 7.

既然这时候 `right` 指针已经被替换成一个比 `pivot` 更大的元素，既然我们要保证“核心思想”成立，我们当然应该把 `right` 继续向左移动。所以 `right -= 1`，也就是：

`[4,0,4,9,2,5,8,9,7]`。`left` 对应 idx = 3, `right` 对应 idx = 6.

!!! question "这里你可能发现了，怎么每次迭代后，数组都有一个数字重复了？比如，原来应该只有1个9，但是这个结果里怎么有2个？"
    因为我们本来一开始就拿了一个 `pivot = 5` 在外面！**我们在迭代终止时得到的位置，也就是 `left` 和 `right` 相遇的位置，就是 `pivot` 应该在的位置！** 这个位置足够好，好到能够满足那两个“核心思想”！所以，实际上每次迭代的结果，都有一个重复的元素。

----

**第三次迭代：**

我们按照第一次迭代的顺序，首先移动 `right`。我们希望右指针一直移动到一个“比pivot还要小的元素为止”。这里 `right` 指向的 `8` 比 `pivot` 小，它要一直挪挪...挪，挪到 `2` 为止，诶，可以了。这个`2`的位置太靠后了，要往前挪，挪到哪？就是 `left` 指针。我们把这个 `0` 放到 `left` 指针的位置，也就变成了：

`[4,0,4,2,2,5,8,9,7]`。`left` 对应 idx = 3, `right` 对应 idx = 4.

既然这时候 `left` 指针已经被替换成一个比 `pivot` 更小的元素，既然我们要保证“核心思想”成立，我们当然应该把 `left` 继续向右移动。所以 `left += 1`，也就是：

`[4,0,4,2,2,5,8,9,7]`。`left` 对应 idx = 4, `right` 对应 idx = 4.

现在，我们移动左边的指针 .... 等等。我们不能再挪了，left 和 `right` 已经相遇了，我们找到了一个合适的，放置 `pivot` 元素的位置了！我们可以终止迭代了！

此时，我们得到了一个看起来好像不那么有序的序列：`[4,0,4,2,2,5,8,9,7]`，以及一个双指针相遇的位置索引，`4`.

我们把pivot元素放到索引的位置处，形成了新的序列：`[4,0,4,2,5,5,8,9,7]`。看起来似乎没那么整齐，但是你可以注意到，我们刚刚插入的 `5` 之前的元素，都不比 `5` 大，而位于插入的 `5` 之后的元素，都不比 `5` 小。

换句话说，对于 `5` 这个元素而言，如果我们把它之前的元素都排好序，再把它之后的元素都排好序 ... 拼起来 ... 那么整个数组就是有序的了！

这就是快速排序的一个idea.

我们可以如法炮制，对 `[4,0,4,2]`，`[5,8,9,7]` 分别排序，就可以了！

> 此处的排序留作习题。你可以复制下面的代码，你也可以让Deepseek给你生成一个。


```python

def partition(arr, low, high):
    pivot = arr[low]
    left, right = low, high
    while left < right:
        while left < right and arr[right] > pivot:
            right -= 1
        if left < right:
            arr[left] = arr[right]
            left += 1
        while left < right and arr[left] < pivot:
            left += 1
        if left < right:
            arr[right] = arr[left]
            right -= 1
    arr[left] = pivot 
    return pivot
    
def quicksort(arr, left, right):
    if left < right:
        idx = partition(arr, left, right)
        quicksort(arr, left, idx - 1)
        quicksort(arr, idx + 1, right)
    

if __name__ == "__main__":
    a = [5,7,4,9,2,5,8,0,4]
    quicksort(a, 0, len(a) - 1)
    for i in a:
        print(i)
```

----

## [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "最小堆"
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给你一个整数数组 nums 和一个整数 k ，请你返回其中出现频率前 k 高的元素。你可以按 任意顺序 返回答案。

 

示例 1:

> 输入: `nums = [1,1,1,2,2,3], k = 2`
> 
> 输出: `[1,2]`
> 
> 示例 2:
> 
> 输入: `nums = [1], k = 1`
> 
> 输出: `[1]`
> 
 

提示：

进阶：你所设计算法的时间复杂度 必须 优于 `O(n log n)` ，其中 `n` 是数组大小。


```python
import heapq
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        freqmap = dict()
        for num in nums:
            if num in freqmap:
                freqmap[num] += 1
            else:
                freqmap[num] = 1
        minHeap = []
        for num, freq in freqmap.items():
            if len(minHeap) < k:
                heapq.heappush(minHeap, (freq, num))
            elif freq > minHeap[0][0]:
                heapq.heappushpop(minHeap, (freq, num))
        
        topK = [num for freq, num in minHeap]
        return topK
```

!!! quote "只需要维护一个大小为 K 的堆即可，每次按需插入，只要发现某个频率比堆顶的元素更小，就pop / push进去。"
