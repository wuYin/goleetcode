---
title: 199. 二叉树的右视图
date: 2018-12-07 14:41:18
tags: [树, 中等]
---
考虑每层只选一个节点，可利用 res 数组的长度做筛选限制。

<!-- more -->

### 题目描述

给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

**示例:**

```
输入: [1,2,3,null,5,null,4]
输出: [1, 3, 4]
解释:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```



### 思路与实现

可直接像 [102. 二叉树层序遍历](https://goleetcode.io/2018/12/04/tree/102-binary-tree-level-order-traversal) 一样，取每层的最右元素即可：

```go
func rightSideView(root *TreeNode) []int {
	var floors [][]int
	traverse(root, 0, &floors)
	
	res := make([]int, len(floors))
	for i, floor := range floors {
		res[i] = floor[len(floor)-1]
	}
	
	return res
}

// 层序遍历
func traverse(node *TreeNode, depth int, floors *[][]int) {
	if node == nil {
		return
	}

	if depth == len(*floors) {
		*floors = append(*floors, []int{})
	}
	(*floors)[depth] = append((*floors)[depth], node.Val)
	
	traverse(node.Left, depth+1, floors)
	traverse(node.Right, depth+1, floors)
}
```

**0 ms, faster than 100.00%**



### 最佳实现

其实层序遍历被我们想复杂了，分析题中的最右侧概念：

- 每层只取一个节点
- 尽可能地从右子树上取

还是从最简单的 case 分析：

```
  1
 / \
2   3
```

右侧视图是 `[1, 3]`，需向下一层一层地处理，优先处理右子树，借助 `res` 数组的长度来取最右侧节点：

```go
func rightSideView(root *TreeNode) []int {
	var res []int
	view(root, 0, &res)
	return res
}

func view(node *TreeNode, depth int, res *[]int) {
	if node == nil {
		return
	}

	if depth == len(*res) {
		*res = append(*res, node.Val) // 选取最右侧节点
	}

	view(node.Right, depth+1, res) // 优先处理右子树，选取最右节点
	view(node.Left, depth+1, res)
}
```

**0 ms, faster than 100.00%**



同理调换一下层次处理顺序，便是查看左视图：

```go
func view(node *TreeNode, depth int, res *[]int) {
	if node == nil {
		return
	}

	if depth == len(*res) {
		*res = append(*res, node.Val) // 选取最左侧节点
	}

	view(node.Left, depth+1, res) // cool
	view(node.Right, depth+1, res)
}
```



### 总结

二叉树的层序遍历一般需借助二维数组存储遍历的中间结果，不过对于层的边缘节点则是没有必要全遍历的。