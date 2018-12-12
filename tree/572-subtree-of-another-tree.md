---
title: 572. 另一个树的子树
date: 2018-12-12 13:41:52
tags: [树, 简单]
---
是否为子树即比较两棵树是否相同，递归分解到左右子树上比较，是 DP 很好的例子。

<!-- more -->

### 题目描述

给定两个非空二叉树 **s** 和 **t**，检验 **s** 中是否包含和 **t** 具有相同结构和节点值的子树。**s** 的一个子树包括 **s** 的一个节点和这个节点的所有子孙。**s** 也可以看做它自身的一棵子树。

**示例 1:**
给定的树 s:

```
     3
    / \
   4   5
  / \
 1   2
```

给定的树 t：

```
   4 
  / \
 1   2
```

返回 **true**，因为 t 与 s 的一个子树拥有相同的结构和节点值。

**示例 2:**
给定的树 s：

```
     3
    / \
   4   5
  / \
 1   2
    /
   0
```

给定的树 t：

```
   4
  / \
 1   2
```

返回 **false**



### 思路与实现

本题和 [100. 相同的树](https://goleetcode.io/2018/12/03/tree/100-same-tree/) 类似，都是比较两棵二叉树之间的相等关系，直接比较即可：

```go
func isSubtree(s *TreeNode, t *TreeNode) bool {
	if isSame(s, t) {
		return true // bingo
	}
	
	if s == nil {
		return false // 向下不会再相等
	}
	
	return isSubtree(s.Left, t) || isSubtree(s.Right, t)
}

func isSame(t1, t2 *TreeNode) bool {
	if t1 == nil && t2 == nil {
		return true
	}
	if t1 == nil || t2 == nil {
		return false
	}
	
	return t1.Val == t2.Val && isSame(t1.Left, t2.Left) && isSame(t1.Right, t2.Right)
}
```

**24 ms, faster than 100.00%**



### 最佳实现

如上的递归向下判断



### 总结

本题是前边题的拓展，也可遍历找到 t 树的根节点后再比较树是否相等，不过对树的操作优先考虑使用递归将大问题化解为子问题解决。

