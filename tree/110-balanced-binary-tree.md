---
title: 110. 平衡二叉树
date: 2018-12-06 15:13:47
tags: [树, 简单]
---
检查各节点的左右子树高度是否平衡。

<!-- more -->

### 题目描述

给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过1

**示例 1:** 给定二叉树 `[3,9,20,null,null,15,7]`，返回 `true` 

```
    3
   / \
  9  20
    /  \
   15   7
```

**示例 2: **给定二叉树 `[1,2,2,3,3,null,null,4,4]`，返回 `false` 

```
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
```



### 思路与实现

验证是否高度平衡，其实是对所有根节点，验证它们的左右子树的高度差是否不超过 1：

```go
func isBalanced(root *TreeNode) bool {
	if root == nil {
		return true
	}

	if diff := depth(root.Left) - depth(root.Right); diff > 1 || diff < -1 { // not ok
		return false
	}

	return isBalanced(root.Left) && isBalanced(root.Right) // 递归检查左右子树是否高度平衡
}

// node 的最大深度即高度
func depth(node *TreeNode) int {
	if node == nil {
		return 0
	}

	l := depth(node.Left) + 1
	r := depth(node.Right) + 1
	if l > r {
		return l
	}

	return r
}
```

**12 ms, faster than 100.00%**



### 最佳实现

如上的递归检验。



### 总结

node 的最大深度即 node 的高度，十分常用。