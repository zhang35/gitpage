---
3title: leetcode[235]Lowest Common Ancestor of a Binary Search Tree Python3实现
date: 2020-09-24 10:35:26
tags: 二叉树
description: 二叉搜索树最低公共祖先
categories: LeetCode
---



```python
# Given a binary search tree (BST), find the lowest common ancestor (LCA) of two
#  given nodes in the BST. 
# 
#  According to the definition of LCA on Wikipedia: “The lowest common ancestor 
# is defined between two nodes p and q as the lowest node in T that has both p and
#  q as descendants (where we allow a node to be a descendant of itself).” 
# 
#  Given binary search tree: root = [6,2,8,0,4,7,9,null,null,3,5] 
# 
#  
# 
#  Example 1: 
# 
#  
# Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
# Output: 6
# Explanation: The LCA of nodes 2 and 8 is 6.
#  
# 
#  Example 2: 
# 
#  
# Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
# Output: 2
# Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant o
# f itself according to the LCA definition.
#  
# 
#  
#  Constraints: 
# 
#  
#  All of the nodes' values will be unique. 
#  p and q are different and both values will exist in the BST. 
#  
#  Related Topics 树 
#  👍 427 👎 0

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

# leetcode submit region begin(Prohibit modification and deletion)

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root: return None
        if root.val < p.val and root.val < q.val:
            return self.lowestCommonAncestor(root.right, p, q)
        if root.val > p.val and root.val > q.val:
            return self.lowestCommonAncestor(root.left, p, q)
        return root

# leetcode submit region end(Prohibit modification and deletion)

```
从根节点向下查找，第一个值处于p、q之间的节点即为所求。

注意p、q的大小关系是未知的。