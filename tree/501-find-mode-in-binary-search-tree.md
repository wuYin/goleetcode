---
title: 501. 二叉搜索树中的众数
date: 2018-12-10 12:22:09
tags: [树, 简单]
---
BST 的中序遍历结果是有序递增序列。

<!-- more -->

### 题目描述

给定一个有相同值的二叉搜索树（BST），找出 BST 中的所有众数（出现频率最高的元素）

例如：给定 BST `[1,null,2,2]`

```
   1
    \
     2
    /
   2
```

返回 `[2]`，**提示**：如果众数超过1个，不需考虑输出顺序



### 思路与实现

一边遍历一边计数即可：

```go
func findMode(root *TreeNode) []int {
	var res []int
	var max int
	traverse(root, &res, &max, make(map[int]int))
	return res
}

func traverse(root *TreeNode, res *[]int, max *int, m map[int]int) {
	if root == nil {
		return
	}
	m[root.Val]++
	switch {
	case m[root.Val] == *max:
		*res = append(*res, root.Val)
	case m[root.Val] > *max: // bingo
		*max = m[root.Val]
		*res = []int{root.Val} // 重置
	}
	traverse(root.Left, res, max, m)
	traverse(root.Right, res, max, m)
}
```

**16 ms, faster than 100.00%** 



### 最佳实现

如上的中序遍历。



### 总结

BST 的中序遍历结果是有序递增序列，可直接计数。