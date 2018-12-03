---
title: 96. 验证二叉搜索树
date: 2018-12-03 13:05:05
tags: [树, 中等]
---

递归比较子节点与父节点的值。

<!-- more -->

### 题目描述

给定一个二叉树，判断其是否是一个有效的二叉搜索树。**示例:**

```
输入:
    2
   / \
  1   3
输出: true

输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]
```



### 思路与实现

根据 BST 的特点，左子节点均小于当前节点，右子节点均大于当前节点。注意值越界溢出问题：

```go
func isValidBST(root *TreeNode) bool {
	return verify(root, -1<<63, 1<<63-1)
}

func verify(root *TreeNode, min, max int) bool {
	if root == nil {
		return true
	}

	if root.Val <= min || root.Val >= max {
		return false
	}

	return verify(root.Left, min, root.Val) && verify(root.Right, root.Val, max)
}
```

**12 ms (40.52%)**



### 最佳实现

上边的递归实现比较容易想到，递归判断左子树上的任意节点，值必须小于 `max`，同理对右子树的任意节点，值必须大于 `min`

在讨论区有一条优秀的解答，使用栈实现的同时还解决了另外两道题：[Learn one iterative inorder traversal, apply it to multiple tree questions](https://leetcode.com/problems/validate-binary-search-tree/discuss/32112/Learn-one-iterative-inorder-traversal-apply-it-to-multiple-tree-questions-(Java-Solution))

核心是二叉树中序遍历的迭代实现： 

```go

func inOrderTraverse(root *TreeNode) []int {
	if root == nil {
		return nil
	}

	var vals []int
	cur := root
	var stack []*TreeNode
	for cur != nil || len(stack) > 0 {
		for cur != nil {
			stack = append(stack, cur) // 依次将父节点、左子节点入栈
			cur = cur.Left
		}
		parent := stack[len(stack)-1]
		stack = stack[:len(stack)-1]
		vals = append(vals, parent.Val) // 将左子节点、父节点出栈
		cur = parent.Right              // 处理右子树
	}

	return vals
}
```



在本题中，可隔层检查是否为 BST：

```go
func isValidBST(root *TreeNode) bool {
	cur := root
	var stack []*TreeNode
	var grand *TreeNode
	for len(stack) > 0 || cur != nil {
		for cur != nil {
			stack = append(stack, cur)
			cur = cur.Left
		}

		parent := stack[len(stack)-1]
		stack = stack[:len(stack)-1]
		if grand != nil && parent.Val <= grand.Val {
			return false
		}
		grand = parent
		cur = parent.Right
	}
	return true
}
```

**8 ms, faster than 100.00%**



### 总结

BST 的中序遍历是升序序列，在迭代实现时可中途进行很多值的判断。