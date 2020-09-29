---
title: leetcode[501]Find Mode in Binary Search Tree Python3实现
date: 2020-09-24 10:35:26
tags: 二叉树
description: dfs，dict，Morris中序遍历待学
categories: LeetCode
---

```python
# Given a binary search tree (BST) with duplicates, find all the mode(s) (the mo
# st frequently occurred element) in the given BST. 
# 
#  Assume a BST is defined as follows: 
# 
#  
#  The left subtree of a node contains only nodes with keys less than or equal t
# o the node's key. 
#  The right subtree of a node contains only nodes with keys greater than or equ
# al to the node's key. 
#  Both the left and right subtrees must also be binary search trees. 
#  
# 
#  
# 
#  For example: 
# Given BST [1,null,2,2], 
# 
#  
#    1
#     \
#      2
#     /
#    2
#  
# 
#  
# 
#  return [2]. 
# 
#  Note: If a tree has more than one mode, you can return them in any order. 
# 
#  Follow up: Could you do that without using any extra space? (Assume that the 
# implicit stack space incurred due to recursion does not count). 
#  Related Topics 树 
#  👍 163 👎 0

class Solution:
    def findMode(self, root: TreeNode) -> List[int]:
        count = {}
        def dfs(root, count):
            if (not root): return
            count[root.val] = count.get(root.val, 0) + 1
            dfs(root.left, count)
            dfs(root.right, count)
        dfs(root, count)
        maxNum = 0
        list = []
        for item in count.items():
            if item[1] > maxNum:
                list.clear();
                list.append(item[0])
                maxNum = item[1]
            elif item[1] == maxNum:
                list.append(item[0])
        return list

# 	执行耗时:76 ms,击败了46.02% 的Python3用户
#	内存消耗:17.2 MB,击败了58.56% 的Python3用户
```



也可以用简洁的写法生成list，但效率很低：

```python
        maxValue = max(count.values())
        return [key for key in count.keys() if count[key]==maxValue]
        
        # 	执行耗时:84 ms,击败了25.66% 的Python3用户
		#	内存消耗:17.2 MB,击败了48.66% 的Python3用户
```



上面是最笨的想法，直接遍历节点并计数，但没有用到二分查找树的特性，也没能做到”不用额外空间“。



### 改进做法

二分查找树 => 中序遍历得到递增数列

问题可转化成在递增数列中找众数，空间复杂度可降为O(1)：

```python
class Solution:
    def findMode(self, root: TreeNode) -> List[int]:
        cur = None
        count = 0
        maxCount = 0
        list = []
        def inorder(root):
            nonlocal cur, count, maxCount, list
            if root.left: inorder(root.left)
            if (root.val == cur):
                count += 1
            else:
                cur = root.val
                count = 1
            if count > maxCount:
                maxCount = count
                list = [cur]
            elif count == maxCount:
                list.append(cur)
            if root.right: inorder(root.right)

        if root: inorder(root)
        return list

#	执行耗时:80 ms,击败了33.08% 的Python3用户
#	内存消耗:17.2 MB,击败了63.71% 的Python3用户
```



### 终极方法：Morris中序遍历

KMP算法的发明者之一Morris设计的神级遍历方法。

可将非递归遍历中的空间复杂度降为O(1)，从而实现时间复杂度为O(N)，而空间复杂度为O(1)的精妙算法。（普通递归算法要算上栈的空间，复杂度是O(n)）

morris遍历利用的是树的叶节点左右孩子为空（树的大量空闲指针），实现空间开销的极限缩减。

本题在遍历时若使用Morris算法，能将空间复杂度降到O(1)。

待学习，留个坑这周补上。