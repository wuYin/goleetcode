---
title: 538. 把二叉搜索树转换为累加树
date: 2018-12-10 16:26:17
tags: [树, 简单]
---
累加即反向后序遍历操作。

<!-- more -->

### 题目描述

给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。

**例如：**

```
输入: 二叉搜索树:
              5
            /   \
           2     13

输出: 转换为累加树:
             18
            /   \
          20     13
```



### 思路与实现

原来的树是 BST，累加即从大到小向下累加，反方向后序遍历即可：

```go
func convertBST(root *TreeNode) *TreeNode {
	var curSum int
	traverse(root, &curSum)
	return root
}

func traverse(root *TreeNode, curSum *int) {
	if root == nil {
		return
	}

	traverse(root.Right, curSum) // 也是后序遍历，不过方向改了

	root.Val += *curSum
	*curSum = root.Val

	traverse(root.Left, curSum)
}
```

**276 ms, faster than 70.83%**



### 最佳实现

稍作修改少传点参数：

```go
func convertBST(root *TreeNode) *TreeNode {
	var curSum int
	var traverse func(node *TreeNode)

	traverse = func(node *TreeNode) {
		if node == nil {
			return
		}

		traverse(node.Right) // 也是后序遍历，不过方向改了
		curSum += node.Val
		node.Val = curSum
		traverse(node.Left)
	}

	traverse(root)
	return root
}
```

**236 ms, faster than 100.00%**



### 总结

在前边二叉树的前中后三种顺序遍历中，左节点和右节点的相对顺序不变，都是先左后右。不过在广义的二叉树遍历中，其实这两个子节点的相对位置无所谓，关键是父节点的位置。