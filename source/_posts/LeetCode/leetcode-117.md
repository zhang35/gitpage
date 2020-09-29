---
title: [117]Populating Next Right Pointers in Each Node II Python3实现
date: 2020-09-24 10:35:26
tags: 二叉树
description: 利用已有next指针，使用虚指针连接，空间复杂度O(1)
categories: LeetCode
---

```python
# Given a binary tree 
# 
#  
# struct Node {
#   int val;
#   Node *left;
#   Node *right;
#   Node *next;
# }
#  
# 
#  Populate each next pointer to point to its next right node. If there is no ne
# xt right node, the next pointer should be set to NULL. 
# 
#  Initially, all next pointers are set to NULL. 
# 
#  
# 
#  Follow up: 
# 
#  
#  You may only use constant extra space. 
#  Recursive approach is fine, you may assume implicit stack space does not coun
# t as extra space for this problem. 
#  
# 
#  
#  Example 1: 
# 
#  
# 
#  
# Input: root = [1,2,3,4,5,null,7]
# Output: [1,#,2,3,#,4,5,7,#]
# Explanation: Given the above binary tree (Figure A), your function should popu
# late each next pointer to point to its next right node, just like in Figure B. T
# he serialized output is in level order as connected by the next pointers, with '
# #' signifying the end of each level.
#  
# 
#  
#  Constraints: 
# 
#  
#  The number of nodes in the given tree is less than 6000. 
#  -100 <= node.val <= 100 
#  
#  Related Topics 树 深度优先搜索 
#  👍 263 👎 0

# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        if not root: return None
        list = [root]
        while len(list) > 0:
            newList = []
            if list[0].left : newList.append(list[0].left)
            if list[0].right : newList.append(list[0].right)
            for i in range(1, len(list)):
                list[i-1].next = list[i]
                if list[i].left : newList.append(list[i].left)
                if list[i].right : newList.append(list[i].right)
            list = newList
        return root
# leetcode submit region end(Prohibit modification and deletion)

```
第一反应是层序遍历，将每层的节点前后连接起来，代码也能通过：

		解答成功:
		执行耗时:64 ms,击败了64.81% 的Python3用户
		内存消耗:14.5 MB,击败了50.95% 的Python3用户

但显然没有达到题目要求，空间复杂度没有降下去。

---

2020年9月29日更新。

如何将空间复杂度降至常数级别呢？

答案是利用已经建立的next指针：假如第n行已经建好了next指针，就可以通过next指针遍历这一行，同时把n+1层的子节点们串起来。这就省下了queue或list的空间。有点类似数学归纳法。

思路有了，实现起来不是那么容易，如何通过遍历第n层，建立起n+1层节点的连接呢？一开始写的代码有点复杂，几十分钟过去了，果断去看大佬们的题解，ColdMe利用**虚节点**的方式十分清晰易懂，仿照着写了下面的代码：

```python
# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        if not root: return None
        leftmost = root # 第n层最左节点
        while leftmost:
            head = Node(0) # 第n+1层的虚拟头结点
            pre = head # 第n+1层的当前节点
            cur = leftmost # 第n层当前节点
            while cur:
                if cur.left:
                    pre.next = cur.left
                    pre = cur.left
                if cur.right:
                    pre.next = cur.right
                    pre = cur.right
                cur = cur.next
            leftmost = head.next # leftmost指向第n+1层最左节点
        return root
# leetcode submit region end(Prohibit modification and deletion)
```
搞懂几个变量的意义就明白算法思路了：
`leftmost`：第n层最左节点；
`cur`：第n层当前节点；
`head`：第n+1层的虚拟头结点；
`pre`：第n+1层的当前节点；