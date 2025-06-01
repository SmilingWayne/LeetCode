---
tags:
  - 图与图论相关
---


# 图/图论相关

## [207. 课程表](https://leetcode.cn/problems/course-schedule/?envType=study-plan-v2&envId=top-interview-150)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 你这个学期必须选修 `numCourses` 门课程，记为 `0` 到 `numCourses - 1` 。
> 
> 在选修某些课程之前需要一些先修课程。 先修课程按数组 `prerequisites` 给出，其中 `prerequisites[i] = [ai, bi]` ，表示如果要学习课程 `ai` 则 必须 先学习课程  `bi` 。
> 
> 例如，先修课程对 `[0, 1]` 表示：想要学习课程 `0` ，你需要先完成课程 `1` 。
> 
> 请你判断是否可能完成所有课程的学习？如果可以，返回 `true` ；否则，返回 `false` 。

 

> 示例 1：
> 
> 输入：`numCourses = 2, prerequisites = [[1,0]]`
> 输出：`true`
> 解释：总共有 `2` 门课程。学习课程 `1` 之前，你需要完成课程 `0` 。这是可能的。

```python
from collections import deque
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        visited = [0 for _ in range(numCourses)]
        adjacent = dict()
        count_income = [0 for _ in range(numCourses)]
        for a, b in prerequisites:
            if b not in adjacent:
                adjacent[b] = [a]
            else:
                adjacent[b].append(a)
            count_income[a] += 1

        queue = deque()
        for i in range(numCourses):
            if count_income[i] == 0:
                queue.append(i)
                visited[i] = 1

        while queue:
            length = len(queue)
            for i in range(length):
                cur = queue.popleft()
                if cur  in adjacent:
                    for k in adjacent[cur]:
                        count_income[k] -= 1
                        if count_income[k] == 0:
                            queue.append(k)
                            visited[k] = 1
                
        return sum(visited) == numCourses
                


```

!!! quote "统计入度表，每次统计“当前可以被上的那个课”，然后依次向后拓展。"


----


## [210. 课程表 II](https://leetcode.cn/problems/course-schedule-ii/description/?envType=study-plan-v2&envId=top-interview-150)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

> 现在你总共有 `numCourses` 门课需要选，记为 0 到 `numCourses - 1`。给你一个数组 `prerequisites` ，其中 `prerequisites[i] = [ai, bi]` ，表示在选修课程 `ai` 前 必须 先选修 `bi` 。
> 
> 
> 例如，想要学习课程 `0` ，你需要先完成课程 `1` ，我们用一个匹配来表示：`[0,1]` 。
> 
> 返回你为了学完所有课程所安排的学习顺序。可能会有多个正确的顺序，你只要返回 任意一种 就可以了。如果不可能完成所有课程，返回 一个空数组 。

 

> 示例 1：
> 
> 输入：`numCourses = 2, prerequisites = [[1,0]]`
> 输出：`[0,1]`
> 解释：总共有 2 门课程。要学习课程 1，你需要先完成课程 0。因此，正确的课程顺序为 `[0,1]` 。

```python
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        adjacent = dict()
        visited = [0 for _ in range(numCourses)]
        prev = [-1 for _ in range(numCourses)]
        cnt_income = [0 for _ in range(numCourses)]
        for a, b in prerequisites:
            if b not in adjacent:
                adjacent[b] = [a]
            else:
                adjacent[b].append(a)
            cnt_income[a] += 1
        
        queue = deque()
        result = []
        for course in range(numCourses):
            if cnt_income[course] == 0:
                queue.append(course)
                result.append(course)
                visited[course] = 1

        while queue:
            length = len(queue)
            for i in range(length):
                cur = queue.popleft()
                if cur in adjacent:
                    for next_ in adjacent[cur]:
                        cnt_income[next_] -= 1
                        if cnt_income[next_] == 0:
                            queue.append(next_)
                            result.append(next_)
                            visited[next_] = 1

        if sum(visited) == numCourses:
            return result
        return []
        
        
```

!!! quote "和前一道题几乎一模一样，差异在于需要每次记录一下目前可以采用的服务顺序，保存在列表中即可。"
