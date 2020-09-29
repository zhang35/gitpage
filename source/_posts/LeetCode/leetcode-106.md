---
title: leetcode[106]从中序与后序遍历序列构造二叉树 Python3实现
date: 2020-09-26 10:35:26
tags: 二叉树
description: 递归，中序+后序构造二叉树
categories: LeetCode
---

```python
# Given a binary tree, we install cameras on the nodes of the tree. 
# 
#  Each camera at a node can monitor its parent, itself, and its immediate child
# ren. 
# 
#  Calculate the minimum number of cameras needed to monitor all nodes of the tr
# ee. 
# 
#  
# 
#  Example 1: 
# 
#  
#  
# Input: [0,0,null,0,0]
# Output: 1
# Explanation: One camera is enough to monitor all nodes if placed as shown.
#  
# 
#  
#  Example 2: 
# 
#  
# Input: [0,0,null,0,null,0,null,null,0]
# Output: 2
# Explanation: At least two cameras are needed to monitor all nodes of the tree.
#  The above image shows one of the valid configurations of camera placement.
#  
# 
#  
# Note: 
# 
#  
#  The number of nodes in the given tree will be in the range [1, 1000]. 
#  Every node has value 0. 
#  
#  
#  
#  Related Topics 树 深度优先搜索 动态规划 
#  👍 171 👎 0

# leetcode submit region begin(Prohibit modification and deletion)

class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        def fun(root, l, r):
            if l > r : return
            node = TreeNode(postorder[root])
            k = inorder.index(postorder[root])
            node.left = fun(root-1-(r-k), l, k-1)
            node.right = fun(root-1, k+1, r)
            return node
        n = len(postorder)
        return fun(n-1, 0, n-1)

# leetcode submit region end(Prohibit modification and deletion)
```

定义了辅助函数

`fun(root, l, r, inorder, postorder)`

root代表postorder[]中的下标，指示树根；

l、r代表inorder[]中的下标，指示左右边界；

重点掌握：

```python
            k = inorder.index(postorder[root])
            node.left = fun(root-1-(r-k), l, k-1)
            node.right = fun(root-1, k+1, r)
```