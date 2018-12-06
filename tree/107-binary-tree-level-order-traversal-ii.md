---
title: 107. 二叉树的层次遍历 II
date: 2018-12-06 12:37:03
tags: [树, 简单]
---
相比 102 将层遍历结果 prepend 处理。

<!-- more -->

### 题目描述

给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）例如：给定二叉树 `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回其自底向上的层次遍历为：

```
[
  [15,7],
  [9,20],
  [3]
]
```



### 思路与实现

这道题与 [102. 二叉树的层次遍历](https://goleetcode.io/2018/12/04/tree/102-binary-tree-level-order-traversal/) 一样，第一反应是把结果 `reverse()` 即可，或者在处理层结果时 `prepend` 而不是 `append`：

```go
func levelOrderBottom(root *TreeNode) [][]int {
	if root == nil {
		return nil
	}

	q := []*TreeNode{root}
	var floors [][]int
	for len(q) > 0 {
		level := len(q)
		var floor []int

		for i := 0; i < level; i++ {
			cur := q[0]
			q = q[1:]
			floor = append(floor, cur.Val)

			if cur.Left != nil {
				q = append(q, cur.Left)
			}
			if cur.Right != nil {
				q = append(q, cur.Right)
			}
		}
	
		floors = append([][]int{floor}, floors...) // prepend
	}
	
	return floors
}
```

**8 ms, faster than 28.57%**



### 最佳实现

102 `append` 递归，107 也可以直接 `prepend` 递归：

```go
func levelOrderBottom(root *TreeNode) [][]int {
	var floors [][]int
	var traverse func(node *TreeNode, depth int)

	traverse = func(node *TreeNode, depth int) {
		if node == nil {
			return
		}

		if len(floors) == depth {
			floors = append(make([][]int, 1), floors...) // prepend // cool
		}

		traverse(node.Left, depth+1)
		traverse(node.Right, depth+1)
		floors[len(floors)-1-depth] = append(floors[len(floors)-1-depth], node.Val) // 先遍历的放在底层 
	}
	traverse(root, 0)

	return floors
}
```

**4 ms, faster than 100.00%**



### 总结

注意对二维数组进行 `prepend` 的操作。