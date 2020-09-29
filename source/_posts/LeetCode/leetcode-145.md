---
title: leetcode[145]Binary Tree Postorder Traversal Python3实现
date: 2020-09-29 12:55:14
tags: 二叉树
description: 二叉树后序遍历，迭代版
categories: LeetCode
---

```python
# Given the root of a binary tree, return the postorder traversal of its nodes' 
# values. 
# 
#  
#  Example 1: 
# 
#  
# Input: root = [1,null,2,3]
# Output: [3,2,1]
#  
# 
#  Example 2: 
# 
#  
# Input: root = []
# Output: []
#  
# 
#  Example 3: 
# 
#  
# Input: root = [1]
# Output: [1]
#  
# 
#  Example 4: 
# 
#  
# Input: root = [1,2]
# Output: [2,1]
#  
# 
#  Example 5: 
# 
#  
# Input: root = [1,null,2]
# Output: [2,1]
#  
# 
#  
#  Constraints: 
# 
#  
#  The number of the nodes in the tree is in the range [0, 100]. 
#  -100 <= Node.val <= 100 
#  
# 
#  
# 
#  Follow up: 
# 
#  Recursive solution is trivial, could you do it iteratively? 
# 
#  
#  Related Topics 栈 树 
#  👍 424 👎 0
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        if not root: return None
        s1 = [root]
        s2 = []
        state = {} # none：not visited；1：right visited
        while s1:
            top = s1[-1]
            if not state.get(top):
                s2.append(top.val)
                if top.right: s1.append(top.right)
                state[top] = 1
            else:
                if top.left: s1[-1] = top.left
                else: s1.pop()
        s2.reverse()
        return s2
# leetcode submit region end(Prohibit modification and deletion)

```

后序遍历，要求非递归，琢磨了好久才写出来。

使用了两个栈+一个状态dict：
`s1`：模拟递归函数栈
`s2`：记录最终结果
`state`：已访问过该节点的几个孩子，None：未访问过；1：已访问过右孩子；

结果不很理想：
```
			执行耗时:44 ms,击败了47.51% 的Python3用户
			内存消耗:13.4 MB,击败了21.18% 的Python3用户
```

留个坑，后面学习大神解法。