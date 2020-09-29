---
title: leetcode[617] Merge Two Binary Trees Python3实现
date: 2020-09-23 11:37:14
tags: 二叉树
description: 经典递归，学习官方题解:先序遍历递归，合并二叉树
categories: LeetCode
---

```python
# Given two binary trees and imagine that when you put one of them to cover the 
# other, some nodes of the two trees are overlapped while the others are not. 
# 
#  You need to merge them into a new binary tree. The merge rule is that if two 
# nodes overlap, then sum node values up as the new value of the merged node. Othe
# rwise, the NOT null node will be used as the node of new tree. 
# 
#  Example 1: 
# 
#  
# Input: 
# 	Tree 1                     Tree 2                  
#           1                         2                             
#          / \                       / \                            
#         3   2                     1   3                        
#        /                           \   \                      
#       5                             4   7                  
# Output: 
# Merged tree:
# 	     3
# 	    / \
# 	   4   5
# 	  / \   \ 
# 	 5   4   7
#  
# 
#  
# 
#  Note: The merging process must start from the root nodes of both trees. 
#  Related Topics 树 
#  👍 486 👎 0

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def mergeTrees(self, t1: TreeNode, t2: TreeNode) -> TreeNode:
        # 如果某棵树的当前节点为空则返回另一棵树的当前节点；
        if (not t1): return t2
        if (not t2): return t1
        # 将t2合并到t1上
        t1.val += t2.val
        # 递归处理当前节点的左、右子树
        t1.left = self.mergeTrees(t1.left, t2.left)
        t1.right = self.mergeTrees(t1.right, t2.right)
        return t1

# leetcode submit region end(Prohibit modification and deletion)
# test case
t1 = TreeNode(1)
t2 = TreeNode(2)
t1.left = TreeNode(3)
t2.right = TreeNode(4)
t2.right.left = TreeNode(5)
t2.right.right = TreeNode(6)
s = Solution()
t = s.mergeTrees(t1, t2)
print(t.val, t.left.val, t.right.val)

```

第一反应是用两个队列同步bfs，写了一半放弃了…因为对Python3语法太不熟练了，需要补一补先。

参考官方题解，使用先序遍历（中-左-右），最终返回t1即可。

