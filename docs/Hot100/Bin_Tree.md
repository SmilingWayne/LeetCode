## 101 对称二叉树 

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "二叉树｜迭代 + 递归"
    <!-- 这里记载考察的数据结构、算法等 -->
    - 🔑🔑 难度：<span style = "color:Green; font-weight:bold">Easy 简单</span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 给你一个二叉树的根节点 root ， 检查它是否轴对称。
>
> ![](https://pic.leetcode.cn/1698026966-JDYPDU-image.png)


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        queue = deque()
        queue.append(root)
        while queue:
            length = len(queue)
            cur_num = []
            for i in range(length):
                cur = queue.popleft()
                
                if cur.left:
                    queue.append(cur.left)
                    cur_num.append(cur.left.val)
                else:
                    cur_num.append(-101)
                if cur.right:
                    queue.append(cur.right)
                    cur_num.append(cur.right.val)
                else:
                    cur_num.append(-101)
            # print(cur_num)
            if cur_num != list(reversed(cur_num)):
                return False
        return True
```

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        
        def check(p, q):
            if p == None and q == None:
                return True
            if p == None or q == None:
                return False
            return p.val == q.val and check(p.left, q.right) and check(p.right, q.left)
        return check(root.left, root.right)
```

!!! quote "迭代的做法，每一层看看是否对称，需要一个队列；"
    递归的做法，：**它们的两个根结点具有相同的值，每个树的右子树都与另一个树的左子树镜像对称**

    我们可以实现这样一个递归函数，通过「同步移动」两个指针的方法来遍历这棵树，$p$ 指针和 $q$ 指针一开始都指向这棵树的根，随后 $p$ 右移时，$q$ 左移，$p$ 左移时，$q$ 右移。每次检查当前 $p$ 和 $q$ 节点的值是否相等，如果相等再判断左右子树是否对称。

