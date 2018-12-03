---
title: 101. 对称二叉树
date: 2018-12-03 16:36:58
tags: [树, 简单]
---

既可以递归地比较对称节点，也可以让对称节点 push 进栈后两两 pop 出栈比较。

<!-- more-->

### 题目描述

给定一个二叉树，检查它是否是镜像对称的。

例如，二叉树 `[1,2,2,3,4,4,3]` 是对称的。

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

但是下面这个 `[1,2,2,null,3,null,3]` 则不是镜像对称的:

```
    1
   / \
  2   2
   \   \
   3    3
```

**说明:**如果你可以运用递归和迭代两种方法解决这个问题，会很加分。



### 思路与实现

其实和 100 题一模一样，只不过相互比较的节点调换了而已：

```go
func isSymmetric(root *TreeNode) bool {
	if root == nil {
		return true
	}
	return symmetric(root.Left, root.Right)
}

func symmetric(p, q *TreeNode) bool {
	if p == nil && q == nil {
		return true
	}

	if p == nil || q == nil {
		return false
	}

	return p.Val == q.Val && symmetric(p.Left, q.Right) && symmetric(p.Right, q.Left) // 对称
}
```

**4 ms, faster than 100.00%**



### 最佳实现

说明里指出了还能用迭代实现，细想一下应该也是栈实现：每次迭代将左右节点入栈，再 pop 比较值：

```go

func isSymmetric(root *TreeNode) bool {
	if root == nil {
		return true
	}

	stack := []*TreeNode{root.Left, root.Right}
	for len(stack) > 0 {
		l := stack[len(stack)-1]
		stack = stack[:len(stack)-1]
		r := stack[len(stack)-1]
		stack = stack[:len(stack)-1] // pop

		if l == nil && r == nil {
			continue
		}
		if l == nil || r == nil { // 左右不对称
			return false
		}
		if l.Val != r.Val { // 左右值不等
			return false
		}

		stack = append(stack, l.Left, r.Right) // 对称入栈
		stack = append(stack, l.Right, r.Left)
	}

	return true
}
```

**4 ms, faster than 100.00%**



### 总结

树相关的迭代函数，注意处理好 `nil` 的情况，避免 panic