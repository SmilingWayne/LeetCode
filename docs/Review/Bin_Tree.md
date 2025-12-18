---
tags:
  - 二叉树
  - 递归
---

# 勇闯秋招手撕系列 IX：二叉树 ｜ 递归

!!! abstract ""

    遍历：<span style="color:green;font-weight:bold">先序 / 中序 / 后序 / 层序遍历 / 二叉树的右视图</span>

    <span style="color:orange;font-weight:bold">从前序+先序 / 中序 + 后序遍历构造 / 二叉树层平均值</span>

    搜索树：<span style="color:orange;font-weight:bold">从有序数组构建搜索树 / 验证搜索树</span>

    <span style="color:orange;font-weight:bold">二叉搜索树的第 K 小元素（🌟）+</span>

    递归：<span style="color:orange;font-weight:bold">翻转二叉树 / 对称二叉树</span>

    <span style="color:orange;font-weight:bold">二叉树直径 / 高度 </span>

    <span style="color:orange;font-weight:bold">二叉树最大路径和</span>

    <span style="color:orange;font-weight:bold">从根节点到叶子结点的数字之和 </span>

    <span style="color:red;font-weight:bold">二叉树路径总和 III </span>

    <span style="color:red;font-weight:bold">二叉树的最近公共祖先（🌟）</span>

    其他树结构：<span style="color:red;font-weight:bold">实现前缀树</span>


## [94. 二叉树前序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/?envType=study-plan-v2&envId=top-100-liked)

```python
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        def dfs(head):
            if not head: return 
            dfs(head.left)
            res.append(head.val)
            dfs(head.right)
        dfs(root)
        return res
```

## [102. 层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/?envType=study-plan-v2&envId=top-100-liked)

```python
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        queue = deque()
        queue.append(root)
        res = []
        while queue:
            l = len(queue)
            temp = []
            for _ in range(l):
                curr = queue.popleft()
                temp.append(curr.val)
                if curr.left:
                    queue.append(curr.left)
                if curr.right:
                    queue.append(curr.right)
            res.append(temp)
        return res
```

## [105. 从前序与中序遍历构建二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/?envType=study-plan-v2&envId=top-100-liked)

```python
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder:
            return None 
        n = len(preorder)
        root = TreeNode(preorder[0])
        idx = inorder.index(preorder[0])
        root.left = self.buildTree(preorder[1 : idx + 1], inorder[: idx])
        root.right = self.buildTree(preorder[idx + 1:], inorder[idx + 1:])
        return root
```

## [106. 从中序与后续遍历构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/?envType=study-plan-v2&envId=top-interview-150)

```python
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
        if len(inorder) == 0:
            return None 
        if len(postorder) == 1:
            return TreeNode(inorder[0])
        
        curr = postorder[-1]
        root = TreeNode(curr)
        mid = inorder.index(curr)
        root.left = self.buildTree(inorder[: mid], postorder[: mid] )
        root.right = self.buildTree(inorder[mid + 1: ], postorder[mid: -1])
        return root
```

## [199. 二叉树右视图](https://leetcode.cn/problems/binary-tree-right-side-view/?envType=study-plan-v2&envId=top-interview-150)

给定一个二叉树的 根节点 root，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

```python
class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        queue = deque()
        queue.append(root)
        res = []
        while queue:
            l = len(queue)
            for i in range(l):
                curr = queue.popleft()
                if curr.left:
                    queue.append(curr.left)
                if curr.right:
                    queue.append(curr.right)
                if i == l - 1:
                    res.append(curr.val)
        return res
```

## [230. 二叉搜索树的第 K 小元素](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/?envType=study-plan-v2&envId=top-interview-150)

给定一个二叉搜索树的根节点 root ，和一个整数 k ，请你设计一个算法查找其中第 k 小的元素（k 从 1 开始计数）。

```python
class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        self.k = k 
        self.min_value = float('inf')
        def dfs(head):
            if not head:
                return 
            if self.k == 0:
                return 
            dfs(head.left)
            self.k -= 1
            if self.k == 0:
                self.min_value = head.val 
                return 
            dfs(head.right)
        
        dfs(root)
        return self.min_value
```


## [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/description/?envType=study-plan-v2&envId=top-interview-150)

给你一个二叉树的根节点 root ，判断其是否是一个有效的二叉搜索树。

有效 二叉搜索树定义如下：节点的左子树只包含 严格小于 当前节点的数;节点的右子树只包含 严格大于 当前节点的数。所有左子树和右子树自身必须也是二叉搜索树。

```python
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        def check(head, lb, ub):
            if not head:
                return True 
            return lb < head.val < ub and check(head.left, lb, head.val) and \
            check(head.right, head.val, ub)
        
        return check(root, float('-inf'), float('inf'))
```

## [108. 有序数组转化为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree?envType=study-plan-v2&envId=top-100-liked)

给你一个整数数组 nums ，其中元素已经按 升序 排列，请你将其转换为一棵 平衡 二叉搜索树。

```python
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        n = len(nums)
        if n == 0:
            return None 
        
        if n == 1:
            return TreeNode(nums[0])
        
        mid = n // 2 
        head = TreeNode(nums[mid])
        head.left = self.sortedArrayToBST(nums[ : mid])
        head.right = self.sortedArrayToBST(nums[mid + 1:])
        return head
```

## [101 对称二叉树](https://leetcode.cn/problems/symmetric-tree/description/?envType=study-plan-v2&envId=top-100-liked)

给你一个二叉树的根节点 root ， 检查它是否轴对称。

```python
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True 

        def check(l, r):
            if not l and not r:
                return True 
            elif not l or not r:
                return False 
            else:
                return l.val == r.val and check(l.right, r.left) and check(l.left, r.right)
        
        return check(root.left, root.right)
```

## [226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/description/?envType=study-plan-v2&envId=top-100-liked)

给你一棵二叉树的根节点 root ，翻转这棵二叉树，并返回其根节点。

```python
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return root 
        l = self.invertTree(root.right)
        r = self.invertTree(root.left)
        root.left = l 
        root.right = r 
        return root
```

## [543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/description/?envType=study-plan-v2&envId=top-100-liked)

给你一棵二叉树的根节点，返回该树的 直径 。

二叉树的 直径 是指树中任意两个节点之间最长路径的 长度 。这条路径可能经过也可能不经过根节点 root 。

两节点之间路径的 长度 由它们之间边数表示。


```python
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        self.maxLen = 0
        def dfs(head):
            if not head:
                return 0
            l = dfs(head.left)
            r = dfs(head.right) 
            self.maxLen = max(self.maxLen, l + r)
            return max(l, r) + 1
        
        dfs(root)
        return self.maxLen
        
```

!!! quote "可以记模板：到当前节点为止，其直径是其左侧的链条长 + 右侧的链条长，左侧链条如果为None，则需要返回0，而以该节点为孩子的父节点，其长度为该节点左右最长链条里选择一个。"

-----

## [437. 路径总和 III](https://leetcode.cn/problems/path-sum-iii/?envType=study-plan-v2&envId=top-100-liked)



给定一个二叉树的根节点 root ，和一个整数 targetSum ，求该二叉树里节点值之和等于 targetSum 的 路径 的数目。路径 不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。


输入：root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8

输出：3

```python
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


---

## [124. 二叉树的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/description/?envType=study-plan-v2&envId=top-100-liked)



<!-- <span style = "color:gold; font-weight:bold">Medium 中等 </span> 中等 -->
<!-- <span style = "color:crisma; font-weight:bold">High 困难</span> 困难 -->
<!-- <span style = "color:Green; font-weight:bold">Easy 简单</span> 简单 -->

<!-- 题目简介 -->

二叉树中的 路径 被定义为一条节点序列，序列中每对相邻节点之间都存在一条边。同一个节点在一条路径序列中 至多出现一次 。该路径 至少包含一个 节点，且不一定经过根节点。

路径和 是路径中各节点值的总和。

给你一个二叉树的根节点 root ，返回其 最大路径和 。

![](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)

```python
class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        self.maxValue = float('-inf')

        def dfs(head):
            if not head:
                return 0
            max_l = max(0, dfs(head.left))
            max_r = max(0, dfs(head.right))
            self.maxValue = max(self.maxValue,  max_l + max_r + head.val)
            return max(max_l, max_r) + head.val
        
        dfs(root)
        return self.maxValue
```

!!! quote "`dfs(head)` 返回的结果是“包含head的路径的最大值，因此，由两部分组成：左边的最大，右边的最大，加上自己。而每次递归的时候，都去判断一下，是否可以更新当前的value。而正因为是包含head的路径的最大值，所以这个函数本身返回的值，必须包括自己，剩下的，捡左右两边最大的（没有的话就都不选）。"


## [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/?envType=study-plan-v2&envId=top-100-liked)

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”


```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root:
            return None 
        if root == q or root == p:
            return root 
        l = self.lowestCommonAncestor(root.left, p, q)
        r = self.lowestCommonAncestor(root.right, p, q)
        if l and r:
            return root 
        elif not l and not r:
            return None 
        else:
            return l if l else r
```


## [208. 实现前缀树](https://leetcode.cn/problems/implement-trie-prefix-tree/description/?envType=study-plan-v2&envId=top-100-liked)



`Trie`（发音类似 "try"）或者说 前缀树 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补全和拼写检查。

请你实现 `Trie` 类：

- `Trie()` 初始化前缀树对象。
- `void insert(String word)` 向前缀树中插入字符串 `word` 。
- `boolean search(String word)` 如果字符串 `word` 在前缀树中，返回 `true`（即，在检索之前已经插入）；否则，返回 `false` 。
- `boolean startsWith(String prefix)` 如果之前已经插入的字符串 `word` 的前缀之一为 `prefix` ，返回 `true` ；否则，返回 `false` 。
 



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
```

!!! quote "这里注意，我们的每个children 都是一个Trie类，构成一个大树。还需要注意，由于需要查找特定结尾的字符串是否在这个树中，所以需要添加 isEnd 属性，不然插入 'apple'，会发现 'app' 这个词也能查找到，但是实际上这只是一个前缀；第三，注意我们自己实现了 `searchPrefix` 这个方法用来查找前缀。"





## [114. 二叉树展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/description/?envType=study-plan-v2&envId=top-100-liked)


难度：<span style = "color:gold; font-weight:bold">Medium 中等 </span> ，给你二叉树的根结点 `root` ，请你将它展开为一个单链表：展开后的单链表应该同样使用 `TreeNode` ，其中 `right` 子指针指向链表中下一个结点，而左子指针始终为 `null` 。展开后的单链表应该与二叉树 先序遍历 顺序相同。

```python

class Solution:
    def flatten(self, root: Optional[TreeNode]) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        result = []
        def preorder(head):
            if not head:
                return 
            result.append(head)
            preorder(head.left)
            preorder(head.right)
        preorder(root)
        for idx, node in enumerate(result):
            if idx == len(result) - 1:
                node.left = None 
                node.right = None 
            else:
                node.left = None 
                node.right = result[idx + 1]


```

!!! quote "先过一遍先序遍历，然后根据这个顺序，重新连接二叉树的节点。"


---

## [129. 从根节点到叶子结点的数字之和](https://leetcode.cn/problems/sum-root-to-leaf-numbers/description/?envType=study-plan-v2&envId=top-interview-150)

给你一个二叉树的根节点 root ，树中每个节点都存放有一个 0 到 9 之间的数字。
每条从根节点到叶节点的路径都代表一个数字：

例如，从根节点到叶节点的路径 1 -> 2 -> 3 表示数字 123 。
计算从根节点到叶节点生成的 所有数字之和 。

叶节点 是指没有子节点的节点。

```python
class Solution:
    def sumNumbers(self, root: Optional[TreeNode]) -> int:
        self.result = 0
        self.dfs(root, 0)
        return self.result

    def dfs(self, head, prev):
        if not head:
            return 
        curr = prev * 10 + head.val
        if not head.left and not head.right:
            self.result += curr 
        self.dfs(head.left, curr)
        self.dfs(head.right, curr)
```

