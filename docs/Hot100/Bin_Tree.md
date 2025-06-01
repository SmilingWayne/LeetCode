---
tags:
  - 二叉树
  - 递归
---

# 二叉树

## [101 对称二叉树 ](https://leetcode.cn/problems/symmetric-tree/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "二叉树｜迭代 + 递归"
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:Green; font-weight:bold">Easy 简单</span>

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

------

## [437. 路径总和 III](https://leetcode.cn/problems/path-sum-iii/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "定义子函数"
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->



> 示例1:
> 给定一个二叉树的根节点 root ，和一个整数 targetSum ，求该二叉树里节点值之和等于 targetSum 的 路径 的数目。
> 
> 路径 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。
>
> ![](https://assets.leetcode.com/uploads/2021/04/09/pathsum3-1-tree.jpg)
>
> 输入：root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
> 输出：3
> 解释：和等于 8 的路径有 3 条，如图所示。


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:

        def rootSum(root, target):
            if root == None:
                return 0
            cnt = 0
            if root.val == target:
                cnt += 1
            cnt += rootSum(root.left, target - root.val)
            cnt += rootSum(root.right, target - root.val)
            return cnt
        
        if root is None:
            return 0
        rv = rootSum(root, targetSum)
        riv = self.pathSum(root.right, targetSum)
        lev = self.pathSum(root.left, targetSum)
        return rv + riv + lev
        
        
```

!!! quote "注意，最终结果的来源具体有：包含root，同时从root向右；包含root，同时从root向左；不包含root，此时就是递归了，分别检查左右各有几个。所以有一个函数要单独实现：包含root的路径搜寻。（对应上面的rootSum函数）"


## [105. 从先序和中序遍历构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note "十分经典的递归：截断位置。"
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> 给定两个整数数组 preorder 和 inorder ，其中 preorder 是二叉树的先序遍历， inorder 是同一棵树的中序遍历，请构造二叉树并返回其根节点。
>
> 输入: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
> 
> 输出: [3,9,20,null,null,15,7]


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if len(inorder) == 0:
            return None
        cut = 0
        for i in range(len(inorder)):
            if inorder[i] == preorder[0]:
                cut = i
        
        
        root = TreeNode(preorder[0])
        root.left = self.buildTree(preorder[1: cut + 1], inorder[:cut])
        root.right = self.buildTree(preorder[cut + 1: ], inorder[cut + 1: ])
        return root
```

!!! quote ""


----


## [226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:Green; font-weight:bold">Easy 简单</span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


> 示例1:
> ![](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)
>
> 输入：`root = [4,2,7,1,3,6,9]`
> 
> 输出：`[4,7,2,9,6,3,1]`


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if root == None:
            return None
        right = self.invertTree(root.right)
        left = self.invertTree(root.left)
        root.left = right
        root.right = left
        return root
```

!!! quote ""

----

## [208. 实现前缀树](https://leetcode.cn/problems/implement-trie-prefix-tree/description/?envType=study-plan-v2&envId=top-100-liked)

<!-- 所有文件名必须是该题目的英文名 -->

!!! note ""
    <!-- 这里记载考察的数据结构、算法等 -->
    🔑🔑 难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span>

<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->


`Trie`（发音类似 "try"）或者说 前缀树 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补全和拼写检查。

请你实现 `Trie` 类：

- `Trie()` 初始化前缀树对象。
- `void insert(String word)` 向前缀树中插入字符串 `word` 。
- `boolean search(String word)` 如果字符串 `word` 在前缀树中，返回 `true`（即，在检索之前已经插入）；否则，返回 `false` 。
- `boolean startsWith(String prefix)` 如果之前已经插入的字符串 `word` 的前缀之一为 `prefix` ，返回 `true` ；否则，返回 `false` 。
 

示例：

> 输入
> `"Trie", "insert", "search", "search", "startsWith", "insert", "search"]`
> `[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]`
> 输出
> `null, null, true, false, true, null, true]`
> 
> 解释
> rie trie = new Trie();
> rie.insert("apple");
> rie.search("apple");   // 返回 True
> rie.search("app");     // 返回 False
> rie.startsWith("app"); // 返回 True
> rie.insert("app");
> rie.search("app");     // 返回 True
> 


```python
class Trie:

    def __init__(self):
        self.children = [None] * 26
        self.isEnd = False

    def insert(self, word: str) -> None:
        node = self
        for ch in word:
            ch_ = ord(ch) - ord('a')
            if not node.children[ch_]:
                node.children[ch_] = Trie()
            node = node.children[ch_]
        node.isEnd = True

    def searchPrefix(self, prefix):
        node = self
        for ch in prefix:
            ch_  = ord(ch) - ord('a')
            if node.children[ch_] == None:
                return None 
            node = node.children[ch_]
        return node 

    def search(self, word: str) -> bool:
        node = self.searchPrefix(word)
        return node is not None and node.isEnd

    def startsWith(self, prefix: str) -> bool:
        return self.searchPrefix(prefix) is not None
# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```

!!! quote "这里注意，我们的每个children 都是一个Trie类，构成一个大树。还需要注意，由于需要查找特定结尾的字符串是否在这个树中，所以需要添加 isEnd 属性，不然插入 'apple'，会发现 'app' 这个词也能查找到，但是实际上这只是一个前缀；第三，注意我们自己实现了 `searchPrefix` 这个方法用来查找前缀。"

