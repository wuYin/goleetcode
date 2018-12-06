---
title: 111. 二叉树的最小深度
date: 2018-12-06 16:23:23
tags: [树, 简单]
---
注意处理只有 1 棵子树的递归退出条件。

<!-- more -->

### 题目描述

给定一个二叉树，找出其最小深度。最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

**示例:** 给定二叉树 `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最小深度  2



### 思路与实现

与 [104. 二叉树的最大深度](https://goleetcode.io/2018/12/05/tree/104-maximum-depth-of-binary-tree) 相反，注意处理只有一棵子树的情况：

```go
func minDepth(root *TreeNode) int {
	if root == nil {
		return 0
	}

	l := minDepth(root.Left)
	r := minDepth(root.Right)
	// 只有 1 棵子树
	if l == 0 {
		return r + 1
	}
	if r == 0 {
		return l + 1
	}

	// 有 2 棵子树则返回较浅的一棵
	if l < r {
		return l + 1
	}
	return r + 1
}
```

**8 ms, faster than 100.00%**

其实代码可以简写，但直观逻辑不是很友好：

```
if l == 0 || r == 0 {
	return l+r+1
}
```



### 最佳实现

如上的递归。



### 总结

树的深度可递归计算，注意处理子树为空的情况。