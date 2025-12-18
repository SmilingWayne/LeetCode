# 勇闯秋招手撕系列 VIII：栈/堆/队列


!!! abstract ""
    <span style="color:green;font-weight:bold">有效的括号</span>

    <span style="color:orange;font-weight:bold">最小栈</span>

    <span style="color:orange;font-weight:bold">字符串解码</span>

    <span style="color:orange;font-weight:bold">每日温度（🌟）</span>

    <span style="color:orange;font-weight:bold">柱状图中的最大矩形</span>

    <span style="color:orange;font-weight:bold">数组中的第 K个最大元素（🌟🌟🌟）</span>

    <span style="color:orange;font-weight:bold">前K个高频元素（🌟）</span>

    <span style="color:red;font-weight:bold">数据流的中位数</span>


## [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/description/?envType=study-plan-v2&envId=top-100-liked)

给定一个只包括 `'('，')'，'{'，'}'，'['，']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：1. 左括号必须用相同类型的右括号闭合。2.左括号必须以正确的顺序闭合。3. 每个右括号都有一个对应的相同类型的左括号。

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        for ch in s:
            if ch in "([{":
                stack.append(ch)
            else:
                if ch == ")":
                    if not stack or stack[-1] != "(":
                        return False 
                    else:
                        stack.pop()
                elif ch == "]":
                    if not stack or stack[-1] != "[":
                        return False 
                    else:
                        stack.pop()
                
                else:
                    if not stack or stack[-1] != "{":
                        return False 
                    else:
                        stack.pop()
        return len(stack) == 0
```



## [155. 最小栈](https://leetcode.cn/problems/min-stack/?envType=study-plan-v2&envId=top-100-liked)

设计一个支持 `push`, `pop`, `top` 操作，并能在常数时间内检索到最小元素的栈。

实现 `MinStack` 类:

- `MinStack()` 初始化堆栈对象。
- `void push(int val)` 将元素val推入堆栈。
- `void pop()` 删除堆栈顶部的元素。
- `int top()` 获取堆栈顶部的元素。
- `int getMin()` 获取堆栈中的最小元素。

```python
class MinStack:

    def __init__(self):
        self.stack = []
        self.min_stack = []
        
    def push(self, val: int) -> None:
        self.stack.append(val)
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)
        

    def pop(self) -> None:
        num = self.stack.pop()
        if num == self.min_stack[-1]:
            self.min_stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min_stack[-1]

```

---

## [394. 字符串解码](https://leetcode.cn/problems/decode-string/?envType=study-plan-v2&envId=top-100-liked)

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 `encoded_string` 正好重复 `k` 次。注意 `k` 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

```python
class Solution:
    def decodeString(self, s: str) -> str:
        n = len(s)
        def dfs(idx):
            multi = 0
            curr = ""
            if idx == n:
                return multi, curr
            i = idx 
            while i < n:
                if s[i] in "0123456789":
                    multi = multi * 10 + int(s[i])
                    i += 1
                elif s[i] == "]":
                    return curr, i 
                elif s[i] == "[":
                    temp, i = dfs(i + 1)
                    curr += temp * multi 
                    multi = 0
                    i += 1
                else:
                    curr += s[i]
                    i += 1
            return multi, curr 
        _, res = dfs(0)
        return res
```


## [739. 每日温度](https://leetcode.cn/problems/daily-temperatures/description/?envType=study-plan-v2&envId=top-100-liked)

给定一个整数数组 temperatures ，表示每天的温度，返回一个数组 answer ，其中 answer[i] 是指对于第 i 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 0 来代替。

```python
class Solution:
    def dailyTemperatures(self, arr: List[int]) -> List[int]:
        n = len(arr)
        res = [0 for _ in range(n)]
        stack = []
        for i, num in enumerate(arr):
            while stack and arr[stack[-1]] < num:
                j = stack.pop()
                res[j] = i - j 
            stack.append(i)
        return res
```


## [84. 柱状图中的最大矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/?envType=study-plan-v2&envId=top-100-liked)

给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。


```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        n = len(heights)
        heights += [-1]
        res = 0 

        stack = []
        for i, h in enumerate(heights):
            while stack and h < heights[stack[-1]]:
                j = stack.pop()
                if len(stack) > 0:
                    width = i - stack[-1] - 1
                else:
                    width = i 
                res = max(res, width * heights[j])
            stack.append(i)
        return res
```



---

## [215. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/description/?envType=study-plan-v2&envId=top-100-liked)

给定整数数组 nums 和整数 k，请返回数组中第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。时间复杂度为 O(n).


```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        n = len(nums)
        def partition(left, right):
            l, r = left, right 
            pivot = nums[left]
            while l < r:
                while l < r and nums[r] > pivot:
                    r -= 1
                if l < r:
                    nums[l] = nums[r]
                    l += 1
                while l < r and nums[l] < pivot:
                    l += 1
                if l < r:
                    nums[r] = nums[l]
                    r -= 1
            nums[l] = pivot 
            return l 
        def topK(left, right, target):
            pos = partition(left, right)
            if pos == target:
                return nums[pos]
            elif pos < target:
                return topK(pos + 1, right, target)
            else:
                return topK(left, pos - 1, target)
        n = len(nums)
        return topK(0, n - 1, n - k)
```

## [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/?envType=study-plan-v2&envId=top-100-liked)

给你一个整数数组 nums 和一个整数 k ，请你返回其中出现频率前 k 高的元素。你可以按 任意顺序 返回答案。

```python
import heapq 
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        res = []
        dic = defaultdict(int)
        for num in nums:
            dic[num] += 1
        
        for key, value in dic.items():
            if len(res) < k:
                heapq.heappush(res, (value, key))
            else:
                heapq.heappushpop(res, (value, key))
        ret = []
        for _, v in res:
            ret.append(v)
        return ret
```



## [295. 数据流的中位数](https://leetcode.cn/problems/find-median-from-data-stream/description/?envType=study-plan-v2&envId=top-100-liked)

中位数是有序整数列表中的中间值。如果列表的大小是偶数，则没有中间值，中位数是两个中间值的平均值。例如 `arr = [2,3,4]` 的中位数是 `3` 。例如 `arr = [2,3]`的中位数是 `(2 + 3) / 2 = 2.5` 。
`MedianFinder()` 初始化 MedianFinder 对象。

`void addNum(int num)` 将数据流中的整数 num 添加到数据结构中。

`double findMedian()` 返回到目前为止所有元素的中位数。

```python
from heapq import * 
class MedianFinder:

    def __init__(self):
        self.A = []
        self.B = []

    def addNum(self, num: int) -> None:
        if len(self.A) != len(self.B):
            heappush(self.B, - heappushpop(self.A, num))
        else:
            heappush(self.A, - heappushpop(self.B, - num))
        

    def findMedian(self) -> float:
        if (len(self.A) + len(self.B)) % 2 == 1:
            return self.A[0]
        else:
            return (self.A[0] - self.B[0]) / 2.0

```

