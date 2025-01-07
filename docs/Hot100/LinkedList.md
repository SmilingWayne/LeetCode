---
tags:
  - 链表
---

# 链表

## [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "链表"
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:Green; font-weight:bold">Easy 简单</span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 null 。
> 
> 图示两个链表在节点 c1 开始相交：
>
> ![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

输入：`intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3`
输出：`Intersected at '8'`
解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 `[4,1,8,4,5]`，链表 B 为 `[5,6,1,8,4,5]`。


```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        tmpA, tmpB = headA, headB 
        while tmpA != tmpB:
            if tmpA == None:
                tmpA = headB
            else:
                tmpA = tmpA.next
            if tmpB == None:
                tmpB = headA
            else:
                tmpB = tmpB.next
        return tmpA
            
```

!!! quote "思路：直接背吧.. 从两个链表头开始遍历，一直到尾，如果先为None了，就指向另一个链表头，继续遍历，直到两个指针相等，就是相交的节点。如果一直没有相交，说明它们两个都是独立的。自然直接返回None."

-----

## [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "必看经典题"
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:Green; font-weight:bold">Easy 简单</span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。
>
> ![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)


```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prev = None
        curr = head
        while curr != None:
            tmp = curr.next # 保存下后面还没处理的
            curr.next = prev # 把未来替换成过去的 
            prev = curr # 把过去的扩展到现在
            curr = tmp # 剩下的都是后面还没处理的
        return prev

```

!!! quote "个人更习惯迭代的做法。重要的是要记得引入 prev 指针。总共 4 行！"

----

## [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "经典题！"
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:Green; font-weight:bold">Easy 简单</span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 给你一个链表的头节点 `head` ，判断链表中是否有环。
> 
> 如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。注意：`pos` 不作为参数进行传递 。仅仅是为了标识链表的实际情况。

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

> 如果链表中存在环 ，则返回 `true` 。 否则，返回 `false` 。
> 


```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        if head == None:
            return False
        l = head
        r = head
        while r != None:
            if r.next != None:
                l = l.next 
                r = r.next.next
            else:
                return False
            if r == l:
                return True
        return False
        
```

!!! quote "注意快慢指针。一开始都从head出发，一定要注意判断 `fast.next` 是否为空！另一个值得注意的事情是，还有一种快慢指针是 fast 直接赋 `head.next`。但是这种判断在下一道题中就有所不同了。"

---


## [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "经典题！2！"
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

> 示例1:
> 给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`。
> 
> 如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 `pos` 是 -1，则在该链表中没有环。注意：`pos` 不作为参数进行传递，仅仅是为了标识链表的实际情况。
> 
> 不允许修改链表。
> 
> 输入：`head = [3,2,0,-4]`, `pos = 1`
> 
> 输出：返回索引为 1 的链表节点
> 
> 解释：链表中有一个环，其尾部连接到第二个节点。

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        
        if head == None:
            return None
        l = head
        r = head
        while r != None:
            l = l.next
            if r.next == None:
                return None
            r = r.next.next
            if l == r:
                tmp = head
                while tmp != l:
                    tmp = tmp.next
                    l = l.next
                return tmp
        return None

```

!!! quote "记忆：相遇的时候，慢的走了 `N`,快的走了`2 * N`。只需要此时从head开始继续走一个指针，就能在相交的地方再度相聚了。"

----

## [19. 删除链表的倒数第k个节点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> ![](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)
>
> 给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。
>
> 


```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        result = ListNode(0)
        result.next = head 
        slow = head 
        fast = head 
        for i in range(n):
            fast = fast.next
            if fast == None:
                break 
        if fast == None:
            return head.next 
        
        while fast.next != None:
            fast = fast.next
            slow = slow.next 
        slow.next = slow.next.next 
        return result.next
```

!!! quote "要注意，可能要删除的是第一个节点！所以分情况讨论，如果删除的是第一个节点，那么`head.next`，否则就利用快慢指针进行操作。"



