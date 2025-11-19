# 勇闯秋招手撕系列 III：滑动窗口/子串

!!! abstract ""
    <span style="color:orange;font-weight:bold">长度最小的子数组 (150)</span>

    <span style="color:orange;font-weight:bold">🌟 无重复字符的最长子串</span>

    <span style="color:orange;font-weight:bold">找到字符串中所有字母异位词</span>

    <span style="color:orange;font-weight:bold">和为K的子数组</span>

    <span style="color:red;font-weight:bold">🌟🌟 滑动窗口最大值</span>

    <span style="color:red;font-weight:bold">🌟🌟 最小覆盖子串</span>

    <span style="color:red;font-weight:bold">串联所有单词的子串 (150)</span>


## [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/?envType=study-plan-v2&envId=top-interview-150)

中等题；给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其总和大于等于 target 的长度最小的 子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

> 输入：target = 7, nums = [2,3,1,2,4,3]
> 输出：2
> 解释：子数组 [4,3] 是该条件下的长度最小的子数组。

```python 
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        if sum(nums) < target:
            return 0
        
        l = 0; r = 0
        n = len(nums)
        cur = 0
        res = n + 1
        while r < n:
            cur += nums[r]
            while cur >= target and l <= r:
                cur -= nums[l]
                res = min(res, r - l + 1)
                l += 1
                
            r += 1
        
        return res
```

<br>
<br>
<br>
<br>

## [🌟 3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/description/?envType=study-plan-v2&envId=top-interview-150)

中等题；给定一个字符串 s ，请你找出其中不含有重复字符的 最长 子串 的长度。

> 输入: s = "abcabcbb"
> 
> 输出: 3 
> 
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。注意 "bca" 和 "cab" 也是正确答案。

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        res = 0
        visited = set()
        l = 0 
        for i, ch in enumerate(s):
            if ch not in visited:
                visited.add(ch)
                res = max(res, i - l + 1)
            else:
                while l < i and s[l] != s[i]:
                    visited.remove(s[l])
                    l += 1
                l += 1
        return res
```

<br>
<br>
<br>
<br>


<br>
<br>
<br>
<br>

## [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/description/?envType=study-plan-v2&envId=top-100-liked)


中等题，给定两个字符串 s 和 p，找到 s 中所有 p 的 异位词 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。


输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词；起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。

```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        
        result = []
        m = len(s)
        n = len(p)
        if m < n:
            return result
        cnt_s = [0 for _ in range(26)]
        cnt_p = [0 for _ in range(26)]
        for i in range(n):
            cnt_s[ord(s[i]) - ord('a')] += 1
            cnt_p[ord(p[i]) - ord('a')] += 1
        
        if cnt_s == cnt_p:
            result.append(0)
        for i in range(n, m):
            cnt_s[ord(s[i]) - ord('a')] += 1
            cnt_s[ord(s[i - n]) - ord('a')] -= 1
            if cnt_s == cnt_p:
                result.append(i - n + 1)
        return result
        

```

!!! quote "注意，判别两个列表是否完全相同，可以直接用等于号！同时，每次动一个字母，删去前面的，新增保留后面的，与目标做对比。"


## [560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/description/?envType=study-plan-v2&envId=top-100-liked) 



中等题；给你一个整数数组 nums 和一个整数 k ，请你统计并返回 该数组中和为 k 的子数组的个数 。子数组是数组中元素的连续非空序列。

> 示例 1：
> 
> 输入：nums = [1,1,1], k = 2;输出：2



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


## [🌟239. 滑动窗口最大值](https://leetcode.cn/problems/subarray-sum-equals-k/description/?envType=study-plan-v2&envId=top-100-liked)


<span style = "color:red; font-weight:bold">困难题</span>；给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回 滑动窗口中的最大值；

输入：`nums = [1,3,-1,-3,5,3,6,7], k = 3`

输出：`[3,3,5,5,6,7]`


```python
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

## [🌟76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/?envType=study-plan-v2&envId=top-100-liked)

给你一个字符串 s 、一个字符串 t 。返回 s 中涵盖 t 所有字符的最小子串。如果 s 中不存在涵盖 t 所有字符的子串，则返回空字符串 "" 。对于 t 中重复字符，我们寻找的子字符串中该字符数量必须不少于 t 中该字符数量。；如果 s 中存在这样的子串，我们保证它是唯一的答案。
输入：s = "ADOBECODEBANC", t = "ABC"；输出："BANC"

```python
from collections import Counter
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        if len(s) < len(t):
            return "" 
        m, n = len(s), len(t)
        cnt_t = Counter(t)
        cnt_s = Counter("")
        cur_l, cur_r = -1, m + 1
        l = 0
        for r in range(m):
            cnt_s[s[r]] += 1
            while True:
                check = True 
                for k, v in cnt_t.items():
                    if cnt_s[k] < v:
                        check = False 
                        break 
                if check:
                    if cur_r - cur_l > r - l:
                        cur_r, cur_l = r, l 
                    cnt_s[s[l]] -= 1
                    l += 1
                else:
                    break
        return "" if cur_l == -1 else s[cur_l: cur_r + 1]

```

## [30. 串联所有单词的子串](https://leetcode.cn/problems/substring-with-concatenation-of-all-words/?envType=study-plan-v2&envId=top-interview-150)

给定一个字符串 s 和一字符串数组 words。 words 中所有字符串 长度相同。s 中的 串联子串 是指一个包含  words 中所有字符串以任意顺序排列连接起来的子串。
**困难题。** 例如，如果 words = ["ab","cd","ef"]， 那么 "abcdef"， "abefcd"，"cdabef"， "cdefab"，"efabcd"， 和 "efcdab" 都是串联子串。 "acdbef" 不是串联子串，因为他不是任何 words 排列的连接。
返回所有串联子串在 s 中的开始索引。你可以以 任意顺序 返回答案。
输入：s = "barfoothefoobarman", words = ["foo","bar"]；输出：[0,9]

```python
class Solution:
    def findSubstring(self, s: str, words: List[str]) -> List[int]:
        word_len = len(words[0])
        window_len = len(words) * word_len 
        word_cnt = Counter(words)
        result = []
        for left in range(word_len):
            cnt_app = defaultdict(int)
            overload = 0
            for right in range(left + word_len, len(s) + 1, word_len):
                cur_word = s[right - word_len : right]
                if cnt_app[cur_word] == word_cnt[cur_word]:
                    overload += 1
                cnt_app[cur_word] += 1
                l = right - window_len
                if l < 0:
                    continue
                if overload == 0:
                    result.append(l)
                prev = s[l : l + word_len]
                cnt_app[prev] -= 1
                if cnt_app[prev] == word_cnt[prev]:
                    overload -= 1
        return result
                
```