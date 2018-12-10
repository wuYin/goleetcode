---
title: 563. 二叉树的坡度
date: 2018-12-10 17:45:41
tags: [树, 简单]
---
坡度是左右节点值之差，即左右子树和之差。

<!-- more -->

### 题目描述

给定一个二叉树，计算**整个树**的坡度。

一个树的**节点的坡度**定义即为，该节点左子树的结点之和和右子树结点之和的**差的绝对值**。空结点的的坡度是0。

**整个树**的坡度就是其所有节点的坡度之和。

**示例:**

```
输入: 
         1
       /   \
      2     3
输出: 1
解释: 
结点的坡度 2 : 0
结点的坡度 3 : 0
结点的坡度 1 : |2-3| = 1
树的坡度 : 0 + 0 + 1 = 1
```

**注意:**

1. 任何子树的结点的和不会超过32位整数的范围。
2. 坡度的值不会超过32位整数的范围。



### 思路与实现

后序遍历向上累计差值即可：

```go
func findTilt(root *TreeNode) int {
	var titlSum int
	var traverse func(node *TreeNode) int
	traverse = func(node *TreeNode) int {
		if node == nil {
			return 0
		}
		lVal := traverse(node.Left)
		rVal := traverse(node.Right)
		titlSum += abs(lVal - rVal) // 需要所有差值，不断向上累计

		return node.Val + lVal + rVal // 返回整棵子树和
	}
	traverse(root)

	return titlSum
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

**16 ms, faster than 100.00%**



### 最佳实现

如上的后序遍历



### 总结

搞清楚坡度即节点值之差的概念，后序遍历向上累计差值即可。