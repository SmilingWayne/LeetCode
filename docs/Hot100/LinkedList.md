---
tags:
  - 链表
---

# 手撕系列 I：链表


!!! abstract ""
    <span style="color:green;font-weight:bold">相交链表</span>

    <span style="color:green;font-weight:bold">🌟🌟 反转链表</span>

    <span style="color:green;font-weight:bold">回文链表</span>

    <span style="color:green;font-weight:bold">环形链表 I / II  </span>

    
    <span style="color:green;font-weight:bold">合并两个有序链表</span>

    <span style="color:orange;font-weight:bold">两数相加</span>

    <span style="color:orange;font-weight:bold">删除链表的倒数第 N 个节点</span>

    <span style="color:orange;font-weight:bold">随机链表的复制</span>

    <span style="color:orange;font-weight:bold">两两交换链表</span>

    <span style="color:orange;font-weight:bold">删除排序链表中的重复元素 II</span> (150)

    <span style="color:orange;font-weight:bold">旋转链表 K 个位置</span> (150)

    <span style="color:orange;font-weight:bold">分隔链表</span> (150)

    <span style="color:orange;font-weight:bold">🌟🌟 重排链表</span> (No 100 or 150)

    <span style="color:red;font-weight:bold">🌟🌟 K 个一组翻转链表</span>

    <span style="color:red;font-weight:bold">LRU 缓存</span>
    


## [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:Green; font-weight:bold">Easy 简单</span>

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

## [🌟206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:Green; font-weight:bold">Easy 简单</span>

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

## 234. 回文链表

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:Green; font-weight:bold">Easy 简单</span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给你一个单链表的头节点 head ，请你判断该链表是否为回文链表。如果是，返回 true ；否则，返回 false 。


```python
class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        if not head:
            return True 
        post = self.partition(head)    
        post = self.reverseList(post)
        curr = head
        while post and curr:
            if post.val != curr.val:
                return False 
            else:
                post = post.next 
                curr = curr.next 
        return True
    
    def partition(self, head):
        slow = head 
        fast = head 
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next 
        return slow 
    
    def reverseList(self, head):
        prev = None 
        curr = head 
        while curr:
            post = curr.next 
            curr.next = prev 
            prev = curr
            curr = post 
        return prev
```

!!! quote "思路是先从整条链表中间切开，对其中的一半做反转，然后两段链表同步前进看是否相同。"

---

## [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:Green; font-weight:bold">Easy 简单</span>

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
        slow = head 
        fast = head 
        while fast and fast.next:
            slow = slow.next 
            fast = fast.next.next 
            if slow == fast:
                return True 
        return False
        
```

!!! quote "快慢指针。fast 总比 slow 快一格。只要是环就一定会遇上，如果不是环，fast 一定会先走到头。"

---


## [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

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
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head 
        fast = head 
        while fast and fast.next:
            slow = slow.next 
            fast = fast.next.next 
            if slow == fast:
                tmp = head 
                while slow != tmp:
                    tmp = tmp.next 
                    slow = slow.next 
                return tmp 
        return None

```

!!! quote "记忆：相遇的时候，慢的走了 `N`,快的走了`2 * N`。只需要此时用一临时指针temp从head开始继续走，slow 也在当前位置继续走，两个指针就能在相交的地方再度相聚了。"

----


## [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:Green; font-weight:bold">Easy 简单</span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 


```python
class Solution:
    def mergeTwoLists(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        hair = ListNode(0)
        res = hair
        while l1 and l2:
            if l1.val < l2.val:
                hair.next = ListNode(l1.val)
                l1 = l1.next 
            else:
                hair.next = ListNode(l2.val)
                l2 = l2.next 
            hair = hair.next 
        hair.next = l1 if l1 else l2
        return res.next
```

!!! quote "很简单，两个链表同时前进，铺铁轨一样，优先把小的接着就行。"

---

## [2. 两数相加](https://leetcode.cn/problems/add-two-numbers/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.

```python
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        hair = ListNode(0)
        added = 0
        res = hair
        while l1 and l2:
            val = l1.val + l2.val + added 
            v, added = val % 10, val // 10
            l1 = l1.next
            l2 = l2.next 
            hair.next = ListNode(v)
            hair = hair.next    
        temp = l1 if l1 else l2 
        while temp:
            val = temp.val + added 
            v, added = val % 10, val // 10
            hair.next = ListNode(v)
            hair = hair.next    
            temp = temp.next 
        if added > 0:
            hair.next = ListNode(added)
        return res.next
```

!!! quote "合并有序链表的进阶版。模拟大数相加。注意总会有一个进位。这可能导致后续有1出现（比如 999 + 1）"


---

## [🌟19. 删除链表的倒数第k个节点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

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
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        if not head:
            return None 
        hair = ListNode(0)
        hair.next = head 
        slow = hair 
        fast = head 
        for _ in range(n):
            fast = fast.next 
        while fast:
            slow = slow.next 
            fast = fast.next 
        if slow:
            slow.next = slow.next.next 
        return hair.next
```

!!! quote "用快慢指针，需要虚拟节点在前面接一下。"

----

## [24. 两两交换链表](https://leetcode.cn/problems/swap-nodes-in-pairs/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

输入：head = [1,2,3,4]
输出：[2,1,4,3]

```python
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head or not head.next:
            return head 
        
        hair = ListNode(0)
        hair.next = head 
        res = hair
        slow = hair 
        fast = hair 
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next 
            if fast:
                post = fast.next 
                fast.next = slow 
                slow.next = post 
                hair.next = fast 
                fast = slow
                hair = slow
        return res.next 

```

!!! quote "开始想觉得有些难做实际上解耦成一个又一个的slow- fast - hair - next 的解耦，分别进行衔接。需要 slow / fast / hair/ head。"

---

## [82. 删除排序链表中的重复元素 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/description/?envType=study-plan-v2&envId=top-interview-150)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给定一个已排序的链表的头 head ， 删除原始链表中所有重复数字的节点，只留下不同的数字 。返回 已排序的链表 。

输入：head = [1,2,3,3,4,4,5]
输出：[1,2,5]

```python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(0)
        dummy.next = head 
        hair = dummy 
        curr = head 
        while curr:
            if not curr.next:
                break 
            nxt = curr.next 
            isdup = False
            if nxt and nxt.val == curr.val:
                isdup = True 
                while nxt and nxt.val == curr.val:
                    nxt = nxt.next
            if isdup:
                hair.next = nxt 
                curr = nxt
            else:
                hair = hair.next
                curr = curr.next
        return dummy.next
```

!!! quote ""

---

## [旋转链表 K 个位置](https://leetcode.cn/problems/rotate-list/description/?envType=study-plan-v2&envId=top-interview-150)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给你一个链表的头节点 head ，旋转链表，将链表每个节点向右移动 k 个位置。



```python
class Solution:
    def rotateRight(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        if not head or not head.next:
            return head 
        cnt = 0
        temp = head 
        while temp is not None:
            cnt += 1
            temp = temp.next 
        k = k % cnt 
        if k == 0:
            return head
        dummy = ListNode(0)
        dummy.next = head 
        fast = head 
        for i in range(k):
            fast = fast.next 
        slow = dummy 
        while fast is not None:
            fast = fast.next 
            slow = slow.next 
        prev = slow.next 
        slow.next = None
        dummy.next = prev 
        while prev and prev.next:
            prev = prev.next 
        prev.next = head 
        return dummy.next

```

!!! quote ""


---

## [86. 分隔链表]()

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

给你一个链表的头节点 head 和一个特定值 x ，请你对链表进行分隔，使得所有 小于 x 的节点都出现在 大于或等于 x 的节点之前。

你应当 保留 两个分区中每个节点的初始相对位置。

```python
class Solution:
    def partition(self, head: Optional[ListNode], x: int) -> Optional[ListNode]:
        post = ListNode(0)
        l1 = post
        prev = ListNode(0)
        l2 = prev
        temp = head 
        while temp:
            if temp.val < x:
                prev.next = ListNode(temp.val)
                prev = prev.next 
            else:
                post.next = ListNode(temp.val)
                post = post.next 
            temp = temp.next 
        prev.next = l1.next 
        return l2.next
```

!!! quote ""


---

## [🌟25. K 个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "链表中的困难题"
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度： <span style = "color:red; font-weight:bold">High 困难</span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->



> 示例1:
> 给你链表的头节点 `head` ，每 `k` 个节点一组进行翻转，请你返回修改后的链表。
> 
> `k` 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 `k` 的整数倍，那么请将最后剩余的节点保持原有顺序。
> 
> 你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。


> 


```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:

        hair = ListNode(0)
        hair.next = head 
        pre = hair 
        while head:
            tail = pre 
            for i in range(k):
                tail = tail.next 
                if not tail:
                    return hair.next 
            nxt = tail.next
            head, tail = self.reverseSub(head, tail)
            pre.next = head
            tail.nxt = nxt 
            pre = tail 
            head = tail.next 
        return hair.next


        
    def reverseSub(self, head, tail):
        prev = tail.next 
        curr = head 
        while prev != tail:
            post = curr.next 
            curr.next = prev 
            prev = curr
            curr = post 
        return tail, head 
    

```

!!! quote "首先，这一题，有两个比较坑的，第一个就是需要加Dummy Node，不然转的时候很麻烦。第二个就是，我们要对原来的链表，在给定head和tail的情况下进行旋转。所以和最基础的反转链表题有不同。我们的prev一开始不再是None了，而是tail后续的节点，我们的终止条件也变成了`prev != tail`，解释起来见下："


    ```python
    def reverseSub(self, head, tail):
        # 首先，将 prev 指针初始化为 tail 的下一个节点
        # 这一步是为了在反转过程中，将反转后的链表段重新连接回原链表
        prev = tail.next 
        # curr 指针初始化为 head，从链表段的起始节点开始进行反转操作
        curr = head 
        # 当 prev 指针不等于 tail 时，继续进行反转操作
        while prev != tail:
            # post 指针用于保存 curr 指针的下一个节点
            # 因为在反转过程中，curr.next 会被修改，所以需要提前保存
            post = curr.next 
            # 将 curr 节点的 next 指针指向 prev
            # 这一步是反转操作的核心，将当前节点指向前一个节点
            curr.next = prev 
            # prev 指针向后移动一位，指向当前节点 curr
            prev = curr
            # curr 指针向后移动一位，指向之前保存的下一个节点 post
            curr = post 
        # 当循环结束时，链表段已经反转完成
        # 返回反转后的新头节点 tail 和新尾节点 head
        return tail, head 
    ```

    **`while prev != tail:` 判断逻辑解释**

    在反转链表的过程中，我们需要一个终止条件来确定何时停止反转操作。这里使用 `prev != tail` 作为循环条件，原因如下：

    - **反转操作的边界**：我们要反转的是从 `head` 到 `tail` 这一段链表。在反转过程中，`prev` 指针初始化为 `tail.next`，表示反转后的链表段要连接的下一个节点。随着反转操作的进行，**`prev` 指针会不断向前移动，而 `curr` 指针也会不断向后移动。** （参考下面的举例）
    - **终止条件**：当 `prev` 指针移动到 `tail` 节点时，说明从 `head` 到 `tail` 这一段链表已经全部反转完成。因为在反转过程中，我们是通过不断将当前节点指向前一个节点来实现反转的，当 `prev` 等于 `tail` 时，意味着 `tail` 节点已经成为了反转后链表段的新头节点，此时反转操作完成，循环终止。

    **示例说明**

    假设我们有一个链表 `1 -> 2 -> 3 -> 4 -> 5`，要反转从节点 `2` 到节点 `4` 这一段，即 `2 -> 3 -> 4`。

    - 初始状态：`head = 2`，`tail = 4`，`prev = 5`，`curr = 2`。
    - 第一次循环：
    - `post = 3`，`curr.next = 5`，即 `2 -> 5`，`prev = 2`，`curr = 3`
    - 第二次循环：
    - `post = 4`，`curr.next = 2`，即 `3 -> 2`，`prev = 3`，`curr = 4`
    - 第三次循环：
    - `post = 5`，`curr.next = 3`，即 `4 -> 3`，`prev = 4`，`curr = 5`
    - 此时 `prev == tail`，循环终止，链表段 `2 -> 3 -> 4` 反转后变为 `4 -> 3 -> 2`。

    通过这种方式，我们可以有效地反转链表中的指定部分。

    -----

    **除此之外，还需要把翻转过的部分重新叠加到原来的LinkedList当中去。** 这就是一些比较基础的操作了。

!!! note "一些概念总结"
    **关于链表中节点相等的含义**
    
    在链表相关操作里，判断两个节点是否相等，通常就是判断它们是否为同一个节点，也就是是否指向内存中的同一个对象。在 Python 里，使用 == 来比较两个节点时，比较的是它们的引用（即内存地址）。如果两个节点变量指向内存中同一个节点对象，那么它们是相等的；反之则不相等。

---

## [143. 重排链表](https://leetcode.cn/problems/reorder-list/description/)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


给定一个单链表 L 的头节点 head ，单链表 L 表示为：

L0 → L1 → … → Ln-1 → Ln 
请将其重新排列后变为：

L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → …

不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

输入: head = [1,2,3,4,5]
输出: [1,5,2,4,3]

```python
class Solution:
    def reorderList(self, head: ListNode) -> None:
        """
        Do not return anything, modify head in-place instead.
        """
        n = 0
        cur = head
        while cur:
            n += 1
            cur = cur.next
        mid = (n + 1) // 2
        mid_node = head
        for _ in range(mid - 1):
            mid_node = mid_node.next
        
        reservsed_head = mid_node.next
        mid_node.next = None
        cur = reservsed_head
        while cur and cur.next:
            nxt = cur.next
            cur.next = nxt.next
            nxt.next = reservsed_head
            reservsed_head = nxt

        cur = head
        while reservsed_head:
            next = cur.next
            cur.next = reservsed_head
            rn = reservsed_head.next
            reservsed_head.next = next
            reservsed_head = rn
            cur = next

```

!!! quote "依然是老套路，拆分、反转、重排。"


---


## [138. 随机链表的复制](https://leetcode.cn/problems/copy-list-with-random-pointer/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

构造这个链表的 深拷贝。 深拷贝应该正好由 n 个 全新 节点组成，其中每个新节点的值都设为其对应的原节点的值。新节点的 next 指针和 random 指针也都应指向复制链表中的新节点，并使原链表和复制链表中的这些指针能够表示相同的链表状态。复制链表中的指针都不应指向原链表中的节点 。




```python
class Solution:
    def copyRandomList(self, head: 'Optional[Node]') -> 'Optional[Node]':
        if not head:
            return None 
        
        dic = dict()
        tmp = head 
        while tmp:
            dic[tmp] = Node(tmp.val)
            tmp = tmp.next 
        tmp = head 
        while tmp:
            dic.get(tmp).next = dic.get(tmp.next)
            dic.get(tmp).random = dic.get(tmp.random)
            tmp = tmp.next 
        return dic.get(head)
        
```

!!! quote "看起来困难做起来没那么困难系列，字典存，然后走2次遍历。"


---

## [146. LRU缓存](https://leetcode.cn/problems/lru-cache/description/)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

请你设计并实现一个满足  LRU (最近最少使用) 缓存 约束的数据结构。
实现 LRUCache 类：
LRUCache(int capacity) 以 正整数 作为容量 capacity 初始化 LRU 缓存
int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
void put(int key, int value) 如果关键字 key 已经存在，则变更其数据值 value ；如果不存在，则向缓存中插入该组 key-value 。如果插入操作导致关键字数量超过 capacity ，则应该 逐出 最久未使用的关键字。
函数 get 和 put 必须以 O(1) 的平均时间复杂度运行。

 

示例：

> 输入
> 
> ["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
> [[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]

> [null, null, null, 1, null, -1, null, -1, 3, 4]

```python
class DLinkedNdode:
    def __init__(self, key = 0, value = 0):
        self.key = key
        self.value = value 
        self.prev = None
        self.next = None 

class LRUCache:

    def __init__(self, capacity: int):
        self.cache = dict()
        self.head = DLinkedNdode()
        self.tail = DLinkedNdode()
        self.head.next = self.tail
        self.tail.prev = self.head 
        self.capacity = capacity 
        self.size = 0

    def get(self, key: int) -> int:
        
        if key not in self.cache:
            return -1
        node = self.cache[key]
        self.moveToHead(node)
        return node.value

    def put(self, key: int, value: int) -> None:
        if key not in self.cache:
            node = DLinkedNdode(key, value)
            self.cache[key] = node 
            self.size += 1
            self.addToHead(node)
            if self.size > self.capacity:
                node1 = self.removeTail()
                self.cache.pop(node1.key)
                self.size -= 1
        else:
            node = self.cache[key]
            node.value = value 
            self.moveToHead(node)


        
    def addToHead(self, node):
        node.prev = self.head 
        node.next = self.head.next 
        self.head.next.prev = node
        self.head.next = node 
        
    def removeNode(self, node):
        node.prev.next = node.next 
        node.next.prev = node.prev
        
    def moveToHead(self, node):
        self.removeNode(node)
        self.addToHead(node)

    def removeTail(self):
        node = self.tail.prev 
        self.removeNode(node)
        return node 
        


# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```

!!! quote ""
    双向链表，重点是新增的几种方法：

    1. addToHead: 用于在出现新的值的时候把它**加入**到头；
    2. removeNode: 删除某个节点；
    3. moveToHead: 用于在旧值出现时候**移动**到头；
    4. removeTail: 删除并返回尾；

    整个逻辑是用哈希表记录当前存在的cache。
