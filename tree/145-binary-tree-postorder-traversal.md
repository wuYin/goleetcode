---
title: 145. 二叉树的后序遍历
date: 2018-12-07 12:04:57
tags: [树, 困难]
---
前序之于后序，即 `append` 之于 `prepend`

<!-- more -->

### 题目描述

给定一个二叉树，返回它的 *后序* 遍历。

**示例:**

```
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [3,2,1]
```

**进阶:** 递归算法很简单，你可以通过迭代算法完成吗？



### 思路与实现

递归简单：

```go
func postorderTraversal(root *TreeNode) []int {
	var res []int
	traverse(root, &res)
	return res
}

func traverse(root *TreeNode, res *[]int) {
	if root == nil {
		return
	}
	traverse(root.Left, res)
	traverse(root.Right, res)
	*res = append(*res, root.Val)
}
```

**0 ms, faster than 100.00%**



### 最佳实现

从最简单的 case 树出发：

```
  1
 / \
2   3
```

与 145 的 stack 只存储右子节点不同，这里左右子节点都放到 stack 中。处理完根节点 1 后，依次将左右子节点入栈，但是在弹出栈时进行 prepend 操作：

- 弹出 3：`[3, 1]`
- 弹出 2：`[2, 3, 1]`

如此向下遍历直到栈空，所有节点处理完毕：

```go
func postorderTraversal(root *TreeNode) []int {
	if root == nil {
		return nil
	}

	var res []int
	stack := []*TreeNode{root}
	for len(stack) > 0 {
		cur := stack[len(stack)-1]
		stack = stack[:len(stack)-1]
		res = append([]int{cur.Val}, res...) // prepend

		if cur.Left != nil {
			stack = append(stack, cur.Left) // 最先进，最后出，prepend 后放到最前边
		}
		if cur.Right != nil {
			stack = append(stack, cur.Right)
		}
	}

	return res
}
```

**0 ms, faster than 100.00%**



### 总结

后序遍历的栈迭代实现很酷，其实后序遍历和前序遍历思维刚好是反过来的，联想到 `append` 的反向操作 `prepend` 就好处理了。

