---
title: Leetcode-968 Binary Tree Cameras Python3实现(动态规划，分状态递归，hard，学习官方题解)
date: 2020-09-23 11:07:39
tags: 二叉树
description: hard，学习官方题解，待掌握
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

from typing import List

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

# leetcode submit region begin(Prohibit modification and deletion)

# 官方题解…
class Solution:
    def minCameraCover(self, root: TreeNode) -> int:
         def dfs(root: TreeNode) -> List[int]:
             if not root:
                 return [float("inf"), 0, 0]
             la, lb, lc = dfs(root.left)
             ra, rb, rc = dfs(root.right)
             a = lc + rc + 1
             b = min(a, la + rb, ra + lb)
             c = min(a, lb + rb)
             return [a, b, c]
         a, b, c = dfs(root)
         return b
# leetcode submit region end(Prohibit modification and deletion)

```

dfs返回三个值：

a：root放置摄像头，覆盖整棵树需要的最少摄像头数量

b：覆盖整棵树需要的最少摄像头数量

c：覆盖左右子树需要的最少摄像头数量



根据定义可得：

```python
# root放1个，那么root.left 和 root.right都不需要了，只需把root.left的左右子树和root.right的左右子树覆盖掉就ok了。
# 总数 = root.left的俩子树需要的数量 + root.right的俩子树需要的数量 + root上的1个
a = lc + rc + 1 

# 3种情况取最小
# a：root放1个时
# la + rb: root不放时，root.left放1个 + root.right所需的最小数量 
# ra + lb: root不放时，root.right放1个 + root.left所需的最小数量 
b = min(a, la + rb, ra + lb)

# 2种情况取最小
# a: root放1个时，
# lb + rb: root不放时，覆盖左子树需要的数量 +覆盖右子树需要的数量
c = min(a, lb + rb)
```
通过dfs递归，自下而上得出root的a、b、c，其中b即为所求。

---

该题目还有贪心解法，待学习。

https://leetcode-cn.com/problems/binary-tree-cameras/