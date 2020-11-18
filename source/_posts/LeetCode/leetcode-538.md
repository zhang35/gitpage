---
title: Leetcode[538]把二叉搜索树转换为累加树 Python3实现
date: 2020-09-23 10:44:18
tags: 
- 二叉树
description: 简单dfs，nonlocal用法
categories: LeetCode
---

```python
# 给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节
# 点值之和。 
# 
#  
# 
#  例如： 
# 
#  输入: 原始二叉搜索树:
#               5
#             /   \
#            2     13
# 
# 输出: 转换为累加树:
#              18
#             /   \
#           20     13
#  
# 
#  
# 
#  注意：本题和 1038: https://leetcode-cn.com/problems/binary-search-tree-to-greater-s
# um-tree/ 相同 
#  Related Topics 树 
#  👍 368 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def convertBST(self, root: TreeNode) -> TreeNode:
        sum = 0
        def dfs(node):
            nonlocal sum;
            if node.right: dfs(node.right)
            node.val += sum
            sum = node.val
            if node.left: dfs(node.left)
        if root: dfs(root)
        return root

# leetcode submit region end(Prohibit modification and deletion)

```

开始刷LeetCode每日一题，这是第一道。

准备放弃C艹拥抱Python，以后就用Python3刷题，顺便学语法。



这个题目倒是不难：

按照**“右-中-左”**的顺序中序遍历，用`sum`记录到达当前节点时已有的累加值，加到这个节点上，更新`sum`即可。



注意下`nonlocal`的用法，它是Python3.2之后引入的一个关键字，简单来说：

`nonlocal`使用在闭包中，能使内部变量操纵外层的同名变量

详见：

[Python nonlocal Keyword](https://www.w3schools.com/python/ref_keyword_nonlocal.asp)

[Python中global和nonlocal区别](https://zhuanlan.zhihu.com/p/41030153)

