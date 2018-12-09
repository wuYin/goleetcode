---
title: 404. 左叶子之和
date: 2018-12-08 12:13:02
tags: [树, 简单]
---
左叶子节点 = 叶子 + 左子节点

<!-- more -->

### 题目描述

计算给定二叉树的所有左叶子之和。

**示例：**在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24

```
    3
   / \
  9  20
    /  \
   15   7
```



### 思路与实现

这个题第一眼以为和 [199. 二叉树的右视图](https://goleetcode.io/2018/12/07/tree/199-binary-tree-right-side-view/) 一样，其实不是：

```
 1
  \
   2
```

如上的右斜树就没有左叶子，但是左视图依旧是 `[1, 2]`

理解左叶子：叶子节点 + 父节点的左子节点

```go
func sumOfLeftLeaves(root *TreeNode) int {
	var sum int
	traverse(root, &sum)
	return sum
}

func traverse(root *TreeNode, sum *int) {
	if root == nil {
		return
	}

	// 左叶子 
	if root.Left != nil && root.Left.Left == nil && root.Left.Right == nil {
		*sum += root.Left.Val // bingo
	}

	traverse(root.Left, sum)
	traverse(root.Right, sum)
}
```

**0 ms, faster than 100.00%**



### 最佳实现

既然有 DFS 的递归实现，那也有 BFS 的 queue 实现：

```go
func sumOfLeftLeaves(root *TreeNode) int {
	if root == nil {
		return 0
	}

	var sum int
	q := []*TreeNode{root}

	for len(q) > 0 {
		cur := q[0]
		q = q[1:]

		if cur.Left != nil && cur.Left.Left == nil && cur.Left.Right == nil {
			sum += cur.Left.Val // bingo
		}

		if cur.Left != nil {
			q = append(q, cur.Left)
		}
		if cur.Right != nil {
			q = append(q, cur.Right)
		}
	}
	return sum
}
```

**0 ms, faster than 100.00%**



### 总结

DFS 和 BFS 都是换汤不换药，左叶子节点都需要站在父节点的角度判断。