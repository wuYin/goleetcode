---
title: 100. 相同的树
date: 2018-12-03 16:17:08
tags: [树, 简单]
---

递归向下比较节点值即可。

<!-- more -->

### 题目描述

给定两个二叉树，编写一个函数来检验它们是否相同。如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

```
输入:       1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

输出: true
```



### 思路与实现

100 相同树与 101 对称树其实是一样的，都是递归向下地比较节点值是否相等：

```go
func isSameTree(p *TreeNode, q *TreeNode) bool {
	if p == nil && q == nil {
		return true
	}
	if p == nil || q == nil {
		return false
	}
	return p.Val == q.Val && isSameTree(p.Left, q.Left) && isSameTree(p.Right, q.Right)
}
```

**0 ms, faster than 100.00%**



### 最佳实现

如上的递归比较。



### 总结

在递归内部一定要有退出条件的判断，一般对 2 个可能为 `nil` 的变量来说，进行比较时有：

```go
if a == nil && b == nil {
    // 同时为 nil 的 case
}

if a == nil || b == nil {
    // 一次包含两种 case
    // if (a == nil && b != nil）|| (a != nil && b == nil) // 不优雅
}

// a != nil && b != nil 的 case
```

上边的代码将 4 种 case 写成了 3 个代码段，比 4 个代码段优雅