---
title: 129. 求根到叶子节点数字之和
date: 2018-12-07 11:02:49
tags: [树, 中等]
---
递归遍历，顺带进行和的累加。

<!-- more -->

### 题目描述

给定一个二叉树，它的每个结点都存放一个 `0-9` 的数字，每条从根到叶子节点的路径都代表一个数字。计算从根到叶子节点生成的所有数字之和。

**示例 1:**

```
输入: [1,2,3]
    1
   / \
  2   3
输出: 25
解释:
从根到叶子节点路径 1->2 代表数字 12.
从根到叶子节点路径 1->3 代表数字 13.
因此，数字总和 = 12 + 13 = 25.
```

**示例 2:**

```
输入: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
输出: 1026
解释: 数字总和 = 495 + 491 + 40 = 1026.
```



### 思路与实现

其实就是一个遍历问题：

```go
func sumNumbers(root *TreeNode) int {
	var sum int
	traverse(root, 0, &sum)
	return sum
}

func traverse(root *TreeNode, cur int, sum *int) {
	if root == nil {
		return
	}
	
	cur *= 10
	cur += root.Val
	if root.Left == nil && root.Right == nil { // 到叶子节点，累加和
		*sum += cur
		return
	}
	
	traverse(root.Left, cur, sum)
	traverse(root.Right, cur, sum)
}
```

**0 ms, faster than 100.00%**



### 最佳实现

如上的递归累加和。



### 总结

递归遍历十分常用。