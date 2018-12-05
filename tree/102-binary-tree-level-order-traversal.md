---
title: 102. 二叉树的层次遍历
date: 2018-12-04 22:27:11
tags: [树, 中等]
---
拓展的 BFS 遍历，可使用 queue 和递归实现。

<!-- more -->

### 题目描述

给定一个二叉树，返回其按层次遍历的节点值。 （即逐层地，从左到右访问所有节点）。例如:

```
    3
   / \
  9  20
    /  \
   15   7
```

返回其层次遍历结果：`[[3], [9,20], [15,7]]`



### 思路与实现

层序遍历顾名思义是逐层地向下遍历，统一处理完一层的节点后处理下一层。相比简单的 BFS 遍历，还需按层存储节点值，这里选用队列实现：

```go
func levelOrder(root *TreeNode) [][]int {
	if root == nil {
		return nil
	}

	var floors [][]int
	q := []*TreeNode{root}

	for len(q) > 0 { // 逐层遍历
		var floor []int
		counts := len(q) // 关键：按层计数
		for i := 0; i < counts; i++ { // 处理本层
			cur := q[0]
			q = q[1:]
			floor = append(floor, cur.Val)

			// 处理下一层
			if cur.Left != nil {
				q = append(q, cur.Left)
			}
			if cur.Right != nil {
				q = append(q, cur.Right)
			}
		}
		floors = append(floors, floor)
	}

	return floors
}
```

**4 ms, faster than 100.00%**



### 最佳实现

除了直观的队列实现，其实还可以直接递归地按层处理：

```go
func levelOrder(root *TreeNode) [][]int {
	var floors [][]int

	var traverse func(node *TreeNode, depth int)
	traverse = func(node *TreeNode, depth int) {
		if node == nil {
			return
		}

		if len(floors) == depth {
			floors = append(floors, []int{})
		}

		floors[depth] = append(floors[depth], node.Val) // 处理同层节点
		traverse(node.Left, depth+1)
		traverse(node.Right, depth+1)
	}

	traverse(root, 0)
	return floors
}

func traverse(root *TreeNode, depth int, floors *[][]int) {
	// 无法对 floors 进行数组的 append 操作
	// *floors[depth] = append(*floors[depth], node.Val) // not ok
	// 所以直接在函数内部使用局部变量
}
```

**4 ms, faster than 100.00%**



### 总结

注意二维数组传参无法 append 单个值的问题。