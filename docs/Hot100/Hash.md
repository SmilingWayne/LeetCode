# Hash

## [1. 两数之和](https://leetcode.cn/problems/two-sum/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

> 你可以假设每种输入只会对应一个答案，并且你不能使用两次相同的元素。
> 
> 你可以按任意顺序返回答案。

 

> 示例 1：
> 
> 输入：`nums = [2,7,11,15]`, `target = 9`
> 
> 输出：`[0,1]`
> 
> 解释：因为 `nums[0] + nums[1] == 9` ，返回 `[0, 1]` 。



```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        record = dict()
        for idx, num in enumerate(nums):
            if target - num in record:
                return [idx, record[target - num]]
            if num not in record:
                record[num] = idx
            
```

!!! quote "非常简单，记录每个数字下标就行了。"


---

## [49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/?envType=study-plan-v2&envId=top-100-liked)

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
> 给你一个字符串数组，请你将 字母异位词 组合在一起。可以按任意顺序返回结果列表。
> 
> 字母异位词 是由重新排列源单词的所有字母得到的一个新单词。

> 示例 1:

> 输入: `strs = ["eat", "tea", "tan", "ate", "nat", "bat"]`
> 
> 输出: `[["bat"],["nat","tan"],["ate","eat","tea"]]`



```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        record = dict()
        for cur_str in strs:
            key_ = "".join(sorted(cur_str))
            if key_ in record:
                record[key_].append(cur_str)
            else:
                record[key_] = [cur_str ]
        result = []
        for k, v in record.items():
            result.append(v)
        return result
```

!!! quote ""


---

## [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 给定一个未排序的整数数组 nums ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

> 请你设计并实现时间复杂度为 O(n) 的算法解决此问题。
> 
> 示例 1：
> 
> 输入：`nums = [100,4,200,1,3,2]`
> 
> 输出：4
> 
> 解释：最长数字连续序列是 `[1, 2, 3, 4]`。它的长度为 `4`。
> 


```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        res = 0
        set_nums = set(nums)
        for num in set_nums:
            if num - 1 not in set_nums:
                cur_num = num 
                curr_len = 1
                while cur_num + 1 in set_nums:
                    cur_num += 1
                    curr_len += 1
                res = max(res, curr_len)
        return res
```

!!! quote "不需要排序，从集合中开始iter，如果遇到一个数字，在集合中不存在更小的值了，说明可以作为一个起点，那么直接开始iter，如果存在更小的值，那么不往前走。"
