---
title: 112. 路径总和
date: 2018-12-06 16:47:00
tags: [树, 简单]
---
当树的递归逻辑需要单独写 `traverse` 处理时，应思考有的参数能否通过计算省略掉。

<!-- more -->

### 题目描述

给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

**示例:** 给定如下二叉树，以及目标和 `sum = 22`

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```

返回 `true`, 因为存在目标和为 22 的根节点到叶子节点的路径 `5->4->11->2`



### 思路与实现

其实还是递归向下遍历的那一套，不过需要带上路径的累加和，如果是叶子节点并且和为 sum，那就 ok

```go
func hasPathSum(root *TreeNode, sum int) bool {
	return traverse(root, 0, sum)
}

func traverse(node *TreeNode, cunSum, target int) bool {
	if node == nil {
		return false
	}

	cunSum += node.Val
	if node.Left == nil && node.Right == nil && cunSum == target { // bingo
		return true
	}

	return traverse(node.Left, cunSum, target) || traverse(node.Right, cunSum, target) // 继续向下累加和并比较值
}
```

**8 ms, faster than 100.00%**



### 最佳实现

同样是递归，有的人就是写得比你简洁：

```go
func hasPathSum(root *TreeNode, sum int) bool {
	if root == nil {
		return false
	}

	if root.Left == nil && root.Right == nil && root.Val == sum {
		return true
	}

	// 递减 sum 比递增 curSum 简洁
	return hasPathSum(root.Left, sum-root.Val) || hasPathSum(root.Right, sum-root.Val)
}
```

**8 ms, faster than 100.00%**



### 总结

以后递归逻辑写到单独的 `traverse` 函数时，就应该反思一下，有的参数是不是通过计算省略掉，从而简化代码。