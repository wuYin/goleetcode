---
title: 222. 完全二叉树的节点个数
date: 2018-12-07 15:08:40
tags: [树, 中等]
---
完全二叉树的子树也是完全二叉树。

<!-- more -->

### 题目描述

给出一个**完全二叉树**，求出该树的节点个数。在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~ 2h 个节点。

**示例:**

```
输入: 
    1
   / \
  2   3
 / \  /
4  5 6

输出: 6
```



### 思路与实现

如果不考虑完全二叉树的特殊形态，直接累计节点个数：

```go
func countNodes(root *TreeNode) int {
	var counter int
	if root != nil {
		counter += 1
		counter += countNodes(root.Left)
		counter += countNodes(root.Right)
	}
	return counter
}
```

**24 ms, faster than 100.00%**



不过对于完全二叉树，它的任意一棵子树都是完全二叉树。对于子树先检查是否为满二叉树，再递归计算：

```go

func countNodes(root *TreeNode) int {
	if root == nil {
		return 0
	}

	l, r := 0, 0
	for lNode := root; lNode != nil; lNode = lNode.Left {
		l++
	}
	for rNode := root; rNode != nil; rNode = rNode.Right {
		r++
	}

	if l == r {
		return 1<<uint(l) - 1 // 满二叉树
	}
	return 1 + countNodes(root.Left) + countNodes(root.Right)
}
```

**24 ms, faster than 100.00%**



### 最佳实现

如上的拆分子树分析，再递归计算。



### 总结

充分利用完全二叉树的特点：子树也是完全二叉树，注意做满二叉树的检查。