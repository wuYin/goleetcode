---
title: 226. 翻转二叉树
date: 2018-12-07 15:35:04
tags: [树, 简单]
---
所谓翻转即交换左右子节点。

<!-- more -->

### 题目描述

翻转一棵二叉树。

输入：

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

输出：

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```



### 思路与实现

所谓翻转即是交换左右子节点，高效简洁的 DFS：

```go
func invertTree(root *TreeNode) *TreeNode {
	if root == nil {
		return nil
	}

	root.Left, root.Right = root.Right, root.Left // 翻转
	invertTree(root.Left)
	invertTree(root.Right)
	
	return root
}
```

**0 ms, faster than 100.00%**

简单明了的 BFS：

```go
func invertTree(root *TreeNode) *TreeNode {
	if root == nil {
		return nil
	}

	q := []*TreeNode{root}
	for len(q) > 0 {
		node := q[0]
		q = q[1:]

		node.Left, node.Right = node.Right, node.Left // 翻转

		if node.Left != nil {
			q = append(q, node.Left)
		}
		if node.Right != nil {
			q = append(q, node.Right)
		}
	}
	return root
}
```

**0 ms, faster than 100.00%**



### 最佳实现

DFS



### 总结

 对于二叉树的操作，用最简单的二叉树 case 分析清楚子问题的解决逻辑后，结合 DFS 就是高效简洁的解决方案。

```
  1
 / \
2   3
```

