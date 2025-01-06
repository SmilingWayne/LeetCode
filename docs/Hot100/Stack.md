# 栈

## [739. 每日温度](https://leetcode.cn/problems/daily-temperatures/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给定一个整数数组 `temperatures` ，表示每天的温度，返回一个数组 `answer` ，其中 `answer[i]` 是指对于第 `i` 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

 

示例 1:

> 输入: `temperatures = [73,74,75,71,69,72,76,73]`
> 
> 输出: `[1,1,4,2,1,1,0,0]`


```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        stack = deque()
        n = len(temperatures)
        result = [0 for _ in range(n)]
        for i in range(n):
            while stack and temperatures[stack[-1]] < temperatures[i]:
                idx = stack.pop()
                result[idx] = i - idx
            # print(len(stack))
            stack.append(i)
        return result


```

!!! quote "单调栈的经典题。不是在for循环的时候维护结果数组，而是在“出现了一个比栈顶元素更大的数字的时候，维护整个栈中存储的下标对应的结果。因为整个栈中下标对应的元素，从底向上一定是递减的。一直出栈到当前的这个元素无法比当前的栈顶元素大，或者栈为空。在这个过程中，栈中下标对应的元素始终是递减的。"


## [155. 最小栈](https://leetcode.cn/problems/min-stack/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度： <span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。
> 
> 实现 MinStack 类:
> 
> MinStack() 初始化堆栈对象。
> void push(int val) 将元素val推入堆栈。
> void pop() 删除堆栈顶部的元素。
> int top() 获取堆栈顶部的元素。
> int getMin() 获取堆栈中的最小元素。
 

示例 1:

输入：
> `["MinStack","push","push","push","getMin","pop","top","getMin"]`
> 
> `[[],[-2],[0],[-3],[],[],[],[]]`

输出：
[null,null,null,null,-3,null,0,-2]

解释：
> MinStack minStack = new MinStack();
> 
> minStack.push(-2);
> 
> minStack.push(0);
> 
> minStack.push(-3);
> 
> minStack.getMin();   --> 返回 -3.
> 
> minStack.pop();
> 
> minStack.top();      --> 返回 0.
> 
> minStack.getMin();   --> 返回 -2.
> 
> 


```python
class MinStack:

    def __init__(self):
        self.stack = []
        self.minStack = []

    def push(self, val: int) -> None:
        self.stack.append(val)
        if len(self.minStack) == 0 or self.minStack[-1] >= val:
            self.minStack.append(val)


    def pop(self) -> None:
        val = self.stack[-1]
        if val == self.minStack[-1]:
            self.minStack = self.minStack[:-1]
        self.stack = self.stack[:-1]

    def top(self) -> int:
        return self.stack[-1]
    
    def getMin(self) -> int:
        return self.minStack[-1]
        
        

# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(val)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()
```

!!! quote "这里有一个小trick，用一个额外list记录“截止到目前最小的元素，因为无论怎么pop，总会先pop到这个最小的元素，再pop到前面的小元素。注意`push` 函数中判别的时候加上等号！"
