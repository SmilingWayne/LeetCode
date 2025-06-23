# 堆



## [347. 前K个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span> 

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回其中出现频率前 `k` 高的元素。你可以按 任意顺序 返回答案。

示例 1:

> 输入: `nums = [1,1,1,2,2,3], k = 2`
> 
> 输出: `[1,2]`



```python
import heapq
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        record = defaultdict(int)
        # heappush
        # heapppushpop
        for num in nums:
            record[num] += 1
        
        array = []
        for val, cnt in record.items():
            if len(array) < k:
                heapq.heappush(array, (cnt, val))
            elif  cnt > array[0][0]:
                heapq.heappushpop(array, (cnt, val))
        
        result = []
        for val in array:
            result.append(val[1])
        return result
```

!!! quote "维护一个固定大小的大顶堆即可。"


---
