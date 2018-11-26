---
title: 95. 不同的二叉搜索树 II
date: 2018-11-26 10:13:05
tags: [树, 中等]
---

递归需分析清楚退出的 case 多多练习。

<!-- more -->

### 题目描述

给定一个整数 *n*，生成所有由 1 ... *n* 为节点所组成的**二叉搜索树**。**示例:**

```
输入: 3
输出:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]

解释: 以上的输出对应以下 5 种不同结构的二叉搜索树：
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```



### 思路与实现
这道题是上一道 96 的衍生，需建立所有 BST，本题使用递归构造：

```go
func generateTrees(n int) []*TreeNode {
	if n == 0 {
		return nil
	}
	return generate(1, n)
}

func generate(start, end int) []*TreeNode {
	var roots []*TreeNode

	switch {
	case start > end: // 某一边子树无节点，左斜树或右斜树
		roots = append(roots, nil)

	case start == end: // 某一边子树只有一个节点
		roots = append(roots, &TreeNode{Val: start})

	case start < end:
		for rootV := start; rootV <= end; rootV++ { // 以 rootV 为根节点
			leftTrees := generate(start, rootV-1)
			rightTrees := generate(rootV+1, end)

			// 组合左右子树
			for _, ltree := range leftTrees {
				for _, rtree := range rightTrees {
					root := &TreeNode{ // 以 rootV 为根节点的一棵 BST
						Val:   rootV,
						Left:  ltree,
						Right: rtree,
					}
					roots = append(roots, root)
				}
			}
		}
	}

	return roots
}
```

执行耗时：**56 ms (60%)**



### 最佳实现

如上的递归构造。



### 总结

递归写法需分清 case 多练习。