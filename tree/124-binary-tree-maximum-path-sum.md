---
title: 124. 二叉树中的最大路径和
date: 2018-12-07 10:15:27
tags: [树, 困难]
---
经过子树根节点的路径只有 4 种，找出最大路径和，累加并比较和，即从叶子节点开始，逐层向上递归分析。

<!-- more -->

### 题目描述

给定一个**非空**二叉树，返回其最大路径和。

本题中，路径被定义为一条从树中任意节点出发，达到任意节点的序列。该路径**至少包含一个**节点，且不一定经过根节点。

**示例 1:**

```
输入: [1,2,3]

       1
      / \
     2   3

输出: 6
```

**示例 2:**

```
输入: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

输出: 42
```



### 思路与实现

这个题困难在于路径的头尾节点不确定，不像 112 和 113 路径总是从 root 出发到叶子节点，124 中可从任意节点走到任意节点。

细想一下，对于任意节点 `node`，它包含的最大和路径只会有 4 种情况：

- 左子树的最大和路径 -> node 本身值 -> 右子树最大和路径 
- 左子树的最大和路径 -> node 本身值 
- node 本身值 -> 右子树最大和路径 

- node 本身的值就已足够大，一个节点就是最大和路径

实现：

```go
func maxPathSum(root *TreeNode) int {
	if root == nil {
		return 0
	}

	maxSum := -1 << 31
	pathSum(root, &maxSum)
	return maxSum
}

func pathSum(node *TreeNode, maxSum *int) int {
	if node == nil {
		return 0
	}

	lSum := pathSum(node.Left, maxSum)  // 递归求解左子树上的最大子路径和
	rSum := pathSum(node.Right, maxSum) // 递归求解右子树上的最大子路径和

	// 四种路径和 case：root / left->root / root->right / left->root->right
	// 更新局部解的最大和到 maxSum
	curMax := node.Val
	if lSum > 0 {
		curMax += lSum
	}
	if rSum > 0 {
		curMax += rSum
	}
	*maxSum = max(*maxSum, curMax)

	// 经过 node 的路径只有 3 条
	// node / left->node / node->right
	return max(node.Val, node.Val+lSum, node.Val+rSum)
}

func max(nums ...int) int {
	m := nums[0]
	for _, n := range nums[1:] {
		if n > m {
			m = n
		}
	}
	return m
}
```

**40 ms, faster than 49.12%**



### 最佳实现

如上的递归实现。



### 总结

其实看到 label 是困难，而且首尾节点不定的时候，就知道这个题不好做，不过代码却很清爽。其实一般对二叉树的操作分析可以从最简单的 case 开始，比如：

```
  1
 / \
2   3
```

最大路径就是 2+1+3

```
   1
  / \
-2   3
```

最大路径是 1+3

显然对于上边的情形，节点 1 的路径和会将路径和小于 0 的 -2 给舍弃掉，转而累加有利的 3。从叶子节点开始逐层向上递归分析（后序遍历）