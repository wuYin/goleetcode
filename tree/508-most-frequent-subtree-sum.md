---
title: 508. 出现次数最多的子树元素和
date: 2018-12-10 14:51:46
tags: [树, 中等]
---
子树求和，即后序遍历向上累加和。

<!-- more -->

### 题目描述

给出二叉树的根，找出出现次数最多的子树元素和。一个结点的子树元素和定义为以该结点为根的二叉树上所有结点的元素之和（包括结点本身）

求出出现次数最多的子树元素和。如果有多个元素出现的次数相同，返回所有出现次数最多的元素（不限顺序）。

**示例 1**：输入

```
  5
 /  \
2   -3
```

返回 [2, -3, 4]，所有的值均只出现一次，以任意顺序返回所有值。

**示例 2** ：输入

```
  5
 /  \
2   -5
```

返回 [2]，只有 2 出现两次，-5 只出现 1 次。



### 思路与实现

本题和 [501. 二叉搜索树中的众数](https://goleetcode.io/2018/12/10/tree/501-find-mode-in-binary-search-tree/) 是类似的题，树的和是子节点的值向上累计，如：

```
  1
 / \
2   3
```

对应的子树和为：

```
  6 // 还有 1
 / \
2   3
```

可看出求树的和，可以后序遍历将值累加到根节点，记下所有子树和的同时，记录下最大和：

```go

func findFrequentTreeSum(root *TreeNode) []int {
	var res []int
	var maxCount int
	sum2Count := make(map[int]int)

	traverse(root, &maxCount, sum2Count)
	for sum, count := range sum2Count {
		if count == maxCount { // bingo
			res = append(res, sum)
		}
	}
	return res
}

// 后序遍历将子节点的和累加到根节点
func traverse(root *TreeNode, maxCount *int, sum2Count map[int]int) int {
	if root == nil {
		return 0
	}

	lSum := traverse(root.Left, maxCount, sum2Count)
	rSum := traverse(root.Right, maxCount, sum2Count)

	rootSum := lSum + rSum + root.Val // 累加
	sum2Count[rootSum ]++
	count := sum2Count[rootSum ]
	if count > *maxCount { // 出现次数最多的和
		*maxCount = count
	}
	return rootSum
}
```

**16 ms, faster than 92.31%**



### 最佳实现

如上的后序遍历累计和



### 总结

有关二叉树的操作，用最简单的树分析清楚一般 case 后，可结合合适的遍历方式加操作来解决。