---
title: leetcode[113]路径总和 II Python3实现
date: 2020-09-26 10:35:26
tags: 二叉树
description: dfs记录路径， 累加，经典题目
categories: LeetCode
---

```python
# 给定一个二叉树和一个目标和，找到所有从根节点到叶子节点路径总和等于给定目标和的路径。 
# 
#  说明: 叶子节点是指没有子节点的节点。 
# 
#  示例: 
# 给定如下二叉树，以及目标和 sum = 22， 
# 
#                5
#              / \
#             4   8
#            /   / \
#           11  13  4
#          /  \    / \
#         7    2  5   1
#  
# 
#  返回: 
# 
#  [
#    [5,4,11,2],
#    [5,8,4,5]
# ]
#  
#  Related Topics 树 深度优先搜索 
#  👍 329 👎 0

# leetcode submit region begin(Prohibit modification and deletion)

class Solution:
    def pathSum(self, root: TreeNode, sum: int) -> List[List[int]]:
        list = []
        path = []
        def dfs(node, curSum):
            if not node: return
            curSum += node.val
            path.append(node.val)
            if not node.left and not node.right and curSum==sum:  #leave node
                list.append(path[:])  #复制出整个序列,否则是传引用，后面修改path会影响list里的值
            if node.left: dfs(node.left, curSum)
            if node.right: dfs(node.right, curSum)
            path.pop() #注意恢复path
        dfs(root, 0)
        return list
# leetcode submit region end(Prohibit modification and deletion)
```

注意：

```python
list.append(path[:])  #复制出整个序列,否则是传引用，后面修改path会影响list里的值
```

python的传值和传址是根据传入参数的类型来选择的：

**传值的参数类型：数字，字符串，元组（immutable）
传址的参数类型：列表，字典（mutable）**

对于list默认会传地址。