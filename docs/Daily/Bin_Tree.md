# 二叉树


## [1367. 二叉树中的链表](https://leetcode.cn/problems/linked-list-in-binary-tree/)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> ![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/29/sample_1_1720.png)
>
> 给你一棵以 `root` 为根的二叉树和一个 `head` 为第一个节点的链表。
> 
> 如果在二叉树中，存在一条一直向下的路径，且每个点的数值恰好一一对应以 `head` 为首的链表中每个节点的值，那么请你返回 `True` ，否则返回 `False` 。
> 
> 一直向下的路径的意思是：从树中某个节点开始，一直连续向下的路径。
>
> 输入：`head = [4,2,8]`, `root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]`
> 输出：`true`
> 
> 解释：树中蓝色的节点构成了与链表对应的子路径。




```python hl_lines="25 26 27"
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubPath(self, head: Optional[ListNode], root: Optional[TreeNode]) -> bool:

        def dfs(r, h):
            if h == None:
                return True
            else:
                if r == None:
                    return False
                elif r.val == h.val:
                    return dfs(r.left, h.next) or dfs(r.right, h.next)    
                else:
                    return False
        if root == None:
            return False
        return dfs(root, head) or self.isSubPath(head, root.left) or self.isSubPath(head, root.right)
```

!!! quote "这里有几个坑点。第一，这个链表的头可能在二叉树中出现好几次，直到后面的某一次才真的可以向下延伸。所以我们在每个节点都需要check一下是否头和头对应上，也就是return后为什么要两次调用自己。同时，既然我们是检查“在这个节点能否延伸”，所以一旦沿着这个头开始检查了，一旦发现不对，就直接停下来返回False即可。"
